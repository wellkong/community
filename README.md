##码匠社区

##资料
[Spring 文档](https://spring.io/guides)
[Spring Web文档](https://spring.io/guides/gs/serving-web-content/)
[es文档](https://elasticsearch.cn/explore)
[Github deploy key文档](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-kys)
[Bootstrap](https://v3.bootcss.com/getting-started/)
[Github OAuth](https://developer.github.com/apps/building-oauth-apps/creating-an-oauth-app/)
[SpringBoot](https://docs.spring.io/spring-boot/docs/)
[Spring](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/web.html#mvc-handlermapping-interceptor)


##工具
[Git](https://git-scm.com/download)
[Visual Paradigm](https://www.visual-paradigm.com)
[Flyway](https://flywaydb.org/getstarted/firststeps/maven)
[Lombok](https://www.projectlombok.org)
[Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-attribute-values)

##配置H2内嵌数据库 重点注意
```
create table user
 (
 	id int auto_increment,
 	account_id varchar(100),
 	name varchar(50),
 	token char(36),
 	gmt_create bigint,
 	gmt_modified bigint,
 	constraint user_pk
 		primary key (id)
 );
 1.在application.properties文件中配置如下：
spring.datasource.url=jdbc:h2:~/community
spring.datasource.username=sa
spring.datasource.password=123
spring.datasource.driver-class-name=org.h2.Driver
出现错误为org.h2.Driver找不到类
解决方法：在pom文件中修改依赖引用为：
 <dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
  </dependency>
2.配置完上述步骤后操作数据库会出现错误：
org.springframework.jdbc.CannotGetJdbcConnectionException: Failed to obtain JDBC Connection; nested exception is org.h2.jdbc.JdbcSQLInvalidAuthorizationSpecException: Wrong user name or password 【28000-199】
解决方法：由于内嵌的H2数据库初始化一个用户，所以使用时需要修改一个用户，执行如下代码
 create user if not exists sa password '123';
3.修改完用户后，执行代码出现错误
Not enough rights for object
解决方法：在上面步骤1修改完用户后，同时执行下面代码，把用户赋予admin超级权限
 alter user sa admin true;
4.修改完上述步骤若使用idea右边的菜单栏Database查看或者操作数据库会出现错误：
org.h2.jdbc.JdbcSQLNonTransientConnectionException: Database may be already in use: null. Possible solutions: close all other connection(s); use the server mode [90020-200]
解决分析：H2内嵌数据库时为单连接，所以出错。
解决方法：在application.properties文件中配置H2的网页操作查看h2数据库
配置如下代码：
#控制台查看数据库的路径  localhost:8887/h2-console
spring.h2.console.path=/h2-console
#是否启用h2控制台
spring.h2.console.enabled=true
#外网是否能连接到h2
spring.h2.console.settings.web-allow-others=true
5.在网页中打开网址：localhost:8887/h2-console
输入用户名和密码启动数据库，至此就可以正常的使用h2内嵌数据库了。
```
```bash
..\src\main\resources\db/migration/V1__Create_user_table.sql
mvn flyway:migrate
```

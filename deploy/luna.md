# 运行环境
## 环境说明
本wiki只针对ubuntu(24.04 LTS)进行验证，其他系统请自行测试。
## Java
- openjdk: v21.0.5

参考[官方文档](https://nodejs.org/en/download/current)
```bash
# install openjdk
apt-get install openjdk-21-jdk
# Verify the java version:
java -version
```
## Apache Tomcat(可选)
- Tomcat: latest

参考[官方文档](https://tomcat.apache.org/download-11.cgi)

## Redis
- Redis: latest

参考[官方文档](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-linux/)
## MySQL
- mySQL: 8.0

参考[官方文档](https://dev.mysql.com/doc/refman/8.0/en/linux-installation-apt-repo.html)

# 编译安装
## 1. 创建数据库表
1. 创建名为**easyads**数据库database；
2. 使用src/main/resources/mysql目录下的sql文件创建数据表；

## 2. 修改配置中的数据库信息
配置文件为`src/main/resources/application-prod.properties`，将`MySQL`和`Redis`更改为自己的配置。
```
...
# easyads db
spring.datasource.easyads.driverClassName = com.mysql.cj.jdbc.Driver
spring.datasource.easyads.jdbc-url = jdbc:mysql://127.0.0.1:3306/easyads?allowMultiQueries=true&characterEncoding=utf-8
spring.datasource.easyads.username = youruser
spring.datasource.easyads.password = yourpassword
...
#primary_conf redis
spring.redis.primary.database = 0
spring.redis.primary.hostName = 127.0.0.1
spring.redis.primary.port = 6379
spring.redis.primary.password = yourpassword

##----------------------- online redis --------------------
# easyads redis
spring.redis.easyads.database = 1
spring.redis.easyads.hostName = 127.0.0.1
spring.redis.easyads.port = 6379
spring.redis.easyads.password = yourpassword
```
## 3. 编译部署
```bash
# 执行构建
mvn clean package -Pprod
# 建议通过Tomcat/Jetty托管
```

## 4.Tomcat托管（可选）
```bash
# 复制war到Tomcat的webapps下
cp target/Luna.war $TOMCAT_WEBAPPS/
```
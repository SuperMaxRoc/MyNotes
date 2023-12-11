# SpringBoot Project Build

更新时间：2021年6月8日14:59:11

注：本文件仅为参考，亦可为偷懒复制的作业，具体问题具体分析和解决。

==本文配合SuperMaster项目一同使用==

## 1. IDEA搭建基础的SpringBoot项目

参考链接：

```xml
https://blog.csdn.net/baidu_39298625/article/details/98102453
```

### 1.1基础环境及版本安装

**注：详见开发新机器搭建教程.md**

1. java：1.8
2. SpringBoot：2.5.0
3. maven
4. tomcat
5. Git
6. MySQL5.7

### 12编写Readme.md

==注：项目介绍，及本机部署步骤，不可或缺==

```markdown
本项目为个人学习项目，集成目前主流的项目技术，配有简单介绍，如有问题希望指出，共同进步。
May the force be with you forever！

本机开发环境：
1.java 1.8
2.maven（更改阿里云仓库/私库）
3.MySQL5.7
4.tomcat
5.Git/SVN

项目环境
1.springboot 2.5基础环境（springboot Starter+web+test）
2.版本控制:Git系/SVN
3.swagger 3.0（2.0同3.0配置及主启动类文件均不同，详情请见swagger）
4.持久层集成（DBS+Mybatis/Mybatis-plus）
5.日志
6.安全：集成shrio
7.多环境部署
8.CI/CD
9.Redis
10.MQ
11.热部署（选用）
12.Linux/Windows发布
13.Docker
14.K8S
15.项目管理：禅道/企业Git搭建
```

### 1.3更改项目文件及结构

#### 1.3.1项目文件

| NO   | 文件名称        | 作用                                     | 是否可删除                                                   |
| ---- | --------------- | ---------------------------------------- | ------------------------------------------------------------ |
| 1    | .gitignore      | 配置Git哪些文件不需要添加到版本管理      | 不使用git的可删除                                            |
| 2    | HELP.md         | 项目的帮助文档                           | 可删除，无影响                                               |
| 3    | mvnw            | linux上处理maven版本兼容问题的脚本       | 可删除，无影响                                               |
| 4    | mvnw.cmd        | windows上处理maven版本兼容问题的脚本     | 可删除，无影响                                               |
| 5    | projectName.iml | 用于保存IDEA对项目的配置，由IDEA自动生成 | 可删除，删除后重新进入项目自动生成，存在配置丢失导致程序异常的风险，不建议删除 |

#### 1.3.2项目结构

注：单体结构：项目结构必建。

1. config：系统配置类
2. domain--sys+app：实体类，与数据库中保持一致
3. entity--VO+struct：VO类和转换类
4. mapper：mapper
5. service-impl：服务层
6. utility：工具类，例如分页工具类
7. web：controller

## 2.版本控制

### 2.1Git系

**操作链接：**

```xml
https://www.cnblogs.com/javis-blog/p/13720042.html
```

#### 2.1.1Git远端平台创建仓库

#### 2.1.2本地创建Git仓库

#### 2.1.3本地项目重新加载

#### 2.1.4更改.gitignore配置文件

 排除不需要上传仓库的临时性文件

### 2.2SVN版

**操作链接：**

```xml
https://blog.csdn.net/alinekang/article/details/81184905
```

## 3.Swagger3.0.0

### 3.1加装swagger3.0.0

#### 3.1.1导入依赖

```java
<!--Swagger3.0.0-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>

        <!--Swagger2.x.x-->
        <!--
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        -->
```

#### 3.1.2启动项目进入Swagger3.0.0

链接：http://localhost:8080/swagger-ui/index.html

### 3.2swagger配置

#### 3.2.1新建SwaggerConfig类

```java
@Configuration
public class swaggerConfig3 {
    @Bean
    public Docket getDocket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(swaggerDemoApiInfo())
                .select()
                .build();
    }
    private ApiInfo swaggerDemoApiInfo(){
        return new ApiInfoBuilder()
                //添加联系人的联系方式
                .contact(new Contact("西米豆豆", "公司主页", "PM的email"))
                //文档标题
                .title("这里是Swagger的标题")
                //文档描述
                .description("这里是Swagger的描述")
                //文档版本
                .version("1.0.0")
                .build();
    }
}
```

#### 3.2.2进入项目的地址

链接：[Swagger UI](http://localhost:9000/swagger-ui/index.html)

### 3.N==注意事项==

#### 3.N.1根据公司/组织/项目要求选择配置swagger

1. 项目：安全性要求，验证用户登录
2. swagger注解的使用
    - @Api--注解到类
    - @ApiOperation--注解到方法
    - @Requestparam--注解到参数
3. swagger设置扫描指定的包

#### 3.N.2swagger与postman

	1. 相同：二者均可模拟调用接口，目前只用过swagger
	2. 区别：swagger可生成前后端通读的接口文档

## 4.持久层集成（DBS+Mybatis/Mybatis-plus）

### 4.1DBS

#### 4.1.1单数据源

##### 4.1.1.1导入依赖Mysql

```xml
<!--添加Mysql数据库的依赖-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.24</version>
</dependency>
<!--JDBC-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
    <version>2.5.0</version>
</dependency>
```

##### 4.1.1.2application.yml中数据库的配置

```xml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/school?useUnicode=true&characterEncoding=utf8
    username: root
    password: *******
```

##### 4.1.1.3IDEA连接数据库

 IDEA提供直连数据库的功能，需要注意配置下serverTimeZone=Asia/Shanghai。

#### 4.1.2多数据源

### 4.2Mybatis/Mybatis-plus

#### 4.2.1Mybatis

#### 4.2.2Mybatis-plus

##### 4.2.2.1导入依赖

```xml
<!--Mybatis-plus-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>Latest Version</version>
</dependency>

<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
</dependency>
```

##### 4.2.2.2application.yml配置Mybatis-plus

```xml
mybatis-plus:
  # Mapper.xml 文件位置 Maven 多模块项目的扫描路径需以 classpath*: 开头
  mapperLocations: classpath*:com/vanhr/**/xml/*Mapper.xml
  #  #MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名 实体扫描，多个package用逗号或者分号分隔
  #  typeAliasesPackage: com.vanhr.user.dao.entity
  #  #通过父类（或实现接口）的方式来限定扫描实体
  #  typeAliasesSuperType: com.vanhr.user.dao.entity.baseEntity
  #  #枚举类 扫描路径 如果配置了该属性，会将路径下的枚举类进行注入，让实体类字段能够简单快捷的使用枚举属性
  #  typeEnumsPackage: com.vanhr.user.dao.enums
  #  #启动时是否检查 MyBatis XML 文件的存在，默认不检查 仅限spring boot 使用
  #  checkConfigLocation : true
  #  #通过该属性可指定 MyBatis 的执行器，MyBatis 的执行器总共有三种：
  #  # ExecutorType.SIMPLE：该执行器类型不做特殊的事情，为每个语句的执行创建一个新的预处理语句（PreparedStatement）
  #  # ExecutorType.REUSE：该执行器类型会复用预处理语句（PreparedStatement）
  #  # ExecutorType.BATCH：该执行器类型会批量执行所有的更新语句
  #  executorType: SIMPLE
  #  # 指定外部化 MyBatis Properties 配置，通过该配置可以抽离配置，实现不同环境的配置部署
  #  configurationProperties:
  configuration: # MyBatis 原生支持的配置
    # 是否开启自动驼峰命名规则（camel case）映射
    mapUnderscoreToCamelCase: true
    # 枚举处理类,如果配置了该属性,枚举将统一使用指定处理器进行处理
    #   org.apache.ibatis.type.EnumTypeHandler : 存储枚举的名称
    #   org.apache.ibatis.type.EnumOrdinalTypeHandler : 存储枚举的索引
    #   com.baomidou.mybatisplus.extension.handlers.MybatisEnumTypeHandler : 枚举类需要实现IEnum接口或字段标记@EnumValue注解.(3.1.2以下版本为EnumTypeHandler)
    #    defaultEnumTypeHandler: com.baomidou.mybatisplus.extension.handlers.MybatisEnumTypeHandler
    # 配置JdbcTypeForNull, oracle数据库必须配置
    jdbc-type-for-null: null
  global-config: # 全局策略配置
    # 是否控制台 print mybatis-plus 的 LOGO
    banner: false
    db-config:
      # id类型
      id-type: auto
      # 表名是否使用下划线命名，默认数据库表使用下划线命名
      table-underline: true
      #是否开启大写命名，默认不开启
#      capital-mode: false
#      #逻辑已删除值,(逻辑删除下有效) 需要注入逻辑策略LogicSqlInjector 以@Bean方式注入
#      logic-not-delete-value: 0
#      #逻辑未删除值,(逻辑删除下有效)
#      logic-delete-value: 1
```

##### 4.2.2.3验证以上添加插件的功能完好

1. mybatis-plus自动生成实体类，mapper，mapper.xml，service及实现类
2. 书写controller测试返回数据

##### 4.2.2.4MP性能分析插件

1. 导入插件

   ```java
   <dependency>
       <groupId>p6spy</groupId>
       <artifactId>p6spy</artifactId>
       <version>3.8.7</version>
   </dependency>
   ```

2. 配置插件

   - 数据连接修改

     ```java
     #驱动需要改为p6spy的驱动
     driverClassName: com.p6spy.engine.spy.P6SpyDriver
     #连接串修改，jdbc后面添加p6spy，其他没有变化
     url: jdbc:p6spy:mysql://数据库地址
     ```

   - p6spy配置文件添加

     ==文件名为：spy.properties，存放路径和application.yml同级==

     ```java
     #3.2.1以上使用
     modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
     #3.2.1以下使用或者不配置
     #modulelist=com.p6spy.engine.logging.P6LogFactory,com.p6spy.engine.outage.P6OutageFactory
     # 自定义日志打印
     logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
     #日志输出到控制台
     appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
     # 使用日志系统记录 sql
     #appender=com.p6spy.engine.spy.appender.Slf4JLogger
     # 设置 p6spy driver 代理
     deregisterdrivers=true
     # 取消JDBC URL前缀
     useprefix=true
     # 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
     excludecategories=info,debug,result,commit,resultset
     # 日期格式
     dateformat=yyyy-MM-dd HH:mm:ss
     # 实际驱动可多个
     #driverlist=org.h2.Driver
     # 是否开启慢SQL记录
     outagedetection=true
     # 慢SQL记录标准 2 秒
     outagedetectioninterval=2
     ```

## 5.日志

### 5.1添加日志配置文件

注：日志分为info，warn，error等等，常用的为info和error，因此下面的配置文件主要针对于info和error，分别写入不同的文件中，方便上线排查问题。

**根据个人项目情况，更改文件存储位置及持久层框架的路径。**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <!-- magenta:洋红 -->
    <!-- boldMagenta:粗红-->
    <!-- cyan:青色 -->
    <!-- white:白色 -->
    <!-- magenta:洋红 -->

    <property name="CONSOLE_LOG_PATTERN"
              value="%date{yyyy-MM-dd HH:mm:ss.SSS} [%-5level] [%thread] [%X{traceId}] | %file:%line %cyan(%method) - %msg%n"/>

    <property name="FILE_LOG_PATTERN"
              value="%date{yyyy-MM-dd HH:mm:ss.SSS} [%-5level] [%thread] [%X{traceId}] | %file:%line %method - %msg%n"/>

    <property name="FILE_ERROR_PATTERN"
              value="%date{yyyy-MM-dd HH:mm:ss.SSS} [%-5level] [%thread] [%X{traceId}] | %file:%line %method - %msg %ex{10}%n"/>

    <property name="FILE_PATH"
              value="log"/>

    <!--==================控制台日志========================================================================================-->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder charset="UTF-8">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>
    <!--==================控制台日志========================================================================================-->


    <!--==================写入文件========================================================================================-->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        　　　　　　
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            　　　　　　　　　
            <fileNamePattern>${FILE_PATH}/file_log//%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            　　　　　　　　　
            <maxHistory>10</maxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>50MB</maxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
            　　　　　　
        </rollingPolicy>
        　　　　　　
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>
    <!--==================写入文件========================================================================================-->

    <!--==================异常日志==================-->
    <appender name="error_file" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 只打印错误日志 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${FILE_PATH}/error_log//%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <maxHistory>10</maxHistory>
            <TimeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>50MB</maxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <encoder>
            <pattern>${FILE_ERROR_PATTERN}</pattern>
        </encoder>
    </appender>

    <!--==================异常日志==================-->

    <logger name="com.roc.SuperMaster.mapper" level="DEBUG" additivity="false">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="error_file" />
    </logger>
    <root level="info">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="FILE"/>
        <appender-ref ref="error_file" />
    </root>

</configuration>
```

### 5.2控制台输出Sql语句

单纯在控制台输出sql语句

修改application.yml

```yaml
打印sql
logging:
  level:
    com.roc.SuperMaster.mapper: debug   # mapper/dao层的路径
```

## 6.Shiro

### 6.1导入依赖

## 7.多环境部署

## 8.CI/CD

## 9.Redis

## 10.MQ

## 11.热启动（选用）

## 12.Linux/windows发布

### 12.1Linux发布

#### 12.1.1Linux环境搭建（详情请见RD_Linux.md）

##### 12.1.1.1java

##### 12.1.1.2mysql

##### 12.1.1.3redis

##### 12.1.1.4nginx

##### 12.1.1.5tomcat

#### 12.1.2前后端项目打包

##### 12.1.2.1更改当前环境配置

##### 12.1.2.2发布到服务器上

1. 前端资源替换
2. 后端服务重启
3. 数据库脚本
4. 相关配置

### 12.2Windows发布

## 13.Docker

## 14.k8s

## 15.项目管理：禅道/企业Git搭建

## N.其他

### N.1定制banner.txt

**注：可有可无，佛系装***

resources文件夹下面创建一个banner.txt文件，springboot启动的时候默认会加载这个文件

banner.txt：佛祖保佑 永不宕机 永无Bug

```xml
${AnsiColor.BRIGHT_YELLOW}
////////////////////////////////////////////////////////////////////
//                          _ooOoo_                               //
//                         o8888888o                              //
//                         88" . "88                              //
//                         (| ^_^ |)                              //
//                         O\  =  /O                              //
//                      ____/`---'\____                           //
//                    .'  \\|     |//  `.                         //
//                   /  \\|||  :  |||//  \                        //
//                  /  _||||| -:- |||||-  \                       //
//                  |   | \\\  -  /// |   |                       //
//                  | \_|  ''\---/''  |   |                       //
//                  \  .-\__  `-`  ___/-. /                       //
//                ___`. .'  /--.--\  `. . ___                     //
//              ."" '<  `.___\_<|>_/___.'  >'"".                  //
//            | | :  `- \`.;`\ _ /`;.`/ - ` : | |                 //
//            \  \ `-.   \_ __\ /__ _/   .-` /  /                 //
//      ========`-.____`-.___\_____/___.-`____.-'========         //
//                           `=---='                              //
//      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^        //
//            佛祖保佑       永不宕机      永无BUG                //
////////////////////////////////////////////////////////////////////
```


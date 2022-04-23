# SpringBoot

## 微服务架构

## 第一个Springboot程序

环境

- jdk1.8
- maven
- springboot
- IDEA

![image-20211214153422383](../images/image-20211214153422383.png)

![image-20211214154720311](../images/image-20211214154720311.png)

![image-20211214154706335](../images/image-20211214154706335.png)

## 原理

[](https://www.cnblogs.com/theRhyme/p/11057233.html)

### pom.xml

- spring-boot-dependency：核心依赖在父工程中
- 引入依赖，不需要版本，因为上面已经搞好了

### starter

![image-20211214155347127](../images/image-20211214155347127.png)

### 主程序

### 自动装配

![image-20211214171907358](../images/image-20211214171907358.png)

### run()

![image-20211214172548811](../images/image-20211214172548811.png)

![img](https://img2018.cnblogs.com/blog/1158841/201907/1158841-20190707171658626-1389392187.png)



## yaml写法

<img src="../images/image-20211214173633193.png" alt="image-20211214173633193" style="zoom: 80%;" />

## 给属性赋值的几种方式

### properties

### yaml

```yaml
person:
  name: jing@qq.com
  age: 20
  cat:
    name: wuwu
    age: 3
  list: [f,u,c,k]
```

![image-20211215194643318](../images/image-20211215194643318.png)

## JSR303校验

![image-20211215194752278](../images/image-20211215194752278.png)

除此之外，还有多种。。。null ,not null等等

## 配置文件到处放

### 配置文件位置

![image-20211215195439228](../images/image-20211215195439228.png)

file:./config/ > file:./ > classpath:./config/ > classpath:./

### 多环境配置

<img src="../images/image-20211215195817677.png" alt="image-20211215195817677" style="zoom:50%;" />

## 自动装配原理再理解

<img src="../images/image-20211216110246042.png" alt="image-20211216110246042" style="zoom:50%;" />

xxxproperties

再yaml中直接xxx:   和person一样

![image-20211216110436851](../images/image-20211216110436851.png)

## Web开发

要解决的问题：

- 导入静态资源
- 首页
- jsp，模板引擎thymeleaf
- 装配扩展SpringMVC
- 拦截器
- 国际化

### 静态资源

new project

spring intialize

choose spring web

![image-20220207115553159](../images/image-20220207115553159.png)

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
    } else {
        this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
        this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
            registration.addResourceLocations(this.resourceProperties.getStaticLocations());
            if (this.servletContext != null) {
                ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                registration.addResourceLocations(new Resource[]{resource});
            }

        });
    }
}
```

总结：

1.在Springboot可以使用以下方式处理静态资源

- webjars   /webjars/
- public,static,resources    /**

2.优先级： resources > static(dft) > public

```properties
spring.mvc.static-path-pattern=/??????
```

### 首页定制

将index.html放入静态资源目录即可。

### 模板引擎Thyleaf

#### 1.使用

![image-20220226190951555](../images/image-20220226190951555.png)

- 添加依赖

- ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
  </dependency>
  ```

- 在templates中编写跳转的html
- ![image-20220226192428804](../images/image-20220226192428804.png)

- Controller测试

- ```java
  @Controller
  public class IndexController {
      @RequestMapping("/test")
      public String test(){
          return "test";
      }
  }
  ```

#### 2.语法

![image-20220226193522982](../images/image-20220226193522982.png)

### MVC自动装配

![image-20220226195608261](../images/image-20220226195608261.png)

![image-20220226201015586](../images/image-20220226201015586.png)

### 一个简单的CRUD项目

#### 国际化

![image-20220227100849282](../images/image-20220227100849282.png)

#### 登录以及展示列表

![image-20220301143402846](../images/image-20220301143402846.png)

#### 404

![image-20220301150455693](../images/image-20220301150455693.png)

### 连接数据库

#### JDBC

1.在创建项目时勾选jdbc

- 或者添加依赖

- ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-jdbc</artifactId>
  </dependency>	
  ```

2.配置链接数据库所需信息

- 在application的yaml

- ```yaml
  spring:
    datasource:
      username: root
      password: root
      url: jdbc:mysql://localhost:3306/jdbc
      driver-class-name: com.mysql.jdbc.Driver
  ```

3.如何查询数据？

- 假设在controller层，可以调用用Bean注册好的JdbcTemplate类。

- ```java
  @RestController
  public class JDBCController {
      @Autowired
      JdbcTemplate jdbcTemplate;
  
      @RequestMapping("/jdbc")
      public List<Map<String,Object>> getData(){
          String sql = "select * from users";
          List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
          return maps;
      }
  ```

增删改查

![image-20220303143752690](../images/image-20220303143752690.png)

#### 整合Druid数据源

[狂神说SpringBoot08：整合Druid (qq.com)](https://mp.weixin.qq.com/s?__biz=Mzg2NTAzMTExNg==&mid=2247483786&idx=1&sn=f5f4ca792611af105140752eb67ce820&scene=19#wechat_redirect)

![image-20220303144222046](../images/image-20220303144222046.png)

yaml配置在上方文档里面。

Druid配置类

![image-20220303150428408](../images/image-20220303150428408.png)

![image-20220303150512878](../images/image-20220303150512878.png)



![image-20220303150441206](../images/image-20220303150441206.png)

![image-20220303150740667](../images/image-20220303150740667.png)

#### 整合Mybatis

1.mybatiscongfig.xml编写

2.application.xml配置

- ![image-20220303152419281](../images/image-20220303152419281.png)
- classpath:指resources下。

3.pojo类、mapper类

- ![image-20220303152508685](../images/image-20220303152508685.png)

# SpringSecurity

## 简介

![image-20220304150915851](../images/image-20220304150915851.png)

## 授权和认证

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
//    链式编程
    @Override
//    授权
    protected void configure(HttpSecurity http) throws Exception {
//        首页所有人都可访问
//        权限页有权限的人才可访问
        http.authorizeHttpRequests().antMatchers("/").permitAll()
                .antMatchers("/vip/**").hasRole("vip")
                .antMatchers("/normal/**").hasRole("normal")
                .antMatchers("/trash/**").hasRole("trash");
//        没有权限的人到登录页面
        http.formLogin();
//        注销：
        http.logout();
    }

//    认证

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
//        可以从数据库或者内存中读入账户和密码

//        auth.inMemoryAuthentication().withUser("test").password("{noop}123456").roles("vip"); 不带加密
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("test").password(new BCryptPasswordEncoder().encode("123456")).roles("vip");
    }
}
```

如何让用户1只能看到用户1的功能？

- 导入thymeleaf+security的包

- 利用命名空间判断。

## 记住我和登录页面定制

![image-20220304163526786](../images/image-20220304163526786.png)

# Shiro


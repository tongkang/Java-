### 从零开始Spring web应用

#### 1、新建项目

1. 新建一个`pom.xml`文件，然后用idea打开它（Open as Project）

   文件内容 从`https://spring.io/guides/gs/spring-boot/`拷贝

2. 根据官网上的新建一个目录结构

   `src/main/java/`

3. 然后创建Application和Controller文件，内容直接复制官网中的。



#### 2、GET请求的查询字符串

1. 例如做一个请求：`http://localhost:8080/search?q=12345`

   search是一个请求路径，？后面的q接的是参数

2. 后端则用`@RequestParam`来接收`q`这个参数。

```java
 @RequestMapping("/search")
    public String search(@RequestParam("q")String key) {
        return "you search is "+key;
    }
```

#### 3、RESTful API

1、RESTful是一个规范

1. 使用HTTP动词来代表动作

- GET：获取资源
- POST：新建资源
- PUT：更新资源
- DELETE：删除资源

例如：`GET/search`

2、使用RESTful风格的参数

- 使用`@PathVariable`进行参数提取，参数是从DeletMapping路径中获取的。

```java
@RestController
@RequestMapping("repos")
public class IssueController {
    @DeleteMapping("/{owner}/{repo}/issues/{issueNumber}/lock")
    public void unlock(
            @PathVariable("owner") String owner,
            @PathVariable("repo") String repo,
            @PathVariable("issueNumber") String issueNumber
    ){
        System.out.println(owner);
        System.out.println(repo);
        System.out.println(issueNumber);
    }
}

-----
Postman请求：http://localhost:8080/repos/tong/kang/issues/12345/lock    
```

#### 4、从HTTP POST请求种提取body

1. 安装gsonformat插件，可以把json字符串生成为Java bean
2. 后台的body用`@RequestBody`注解来接收
3. 使用PostMan调试用Body种的raw选项，把json复制进去。并且要设置Header格式，如：Content-Type
4. Header中的属性，Accept用于来接收格式的，如：client设置json，服务器发送一个json，设置xml，就发送一个xml



#### 5、Spring MyBatis实战

1. ##### 引入SpringBoot的Mybatis的maven

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

2. ##### 引入H2数据库maven

3. ##### 建一个resources文件夹和`application.properties`文件

   ```properties
   spring.datasource.url=jdbc:h2:file:D:/Java_practice/Spring-web/target/test
   spring.datasource.username=root
   spring.datasource.password=root
   spring.datasource.driver-class-name=org.h2.Driver
   ```

4. ##### 创建migration（数据迁移），db/migration/V1_CreateTable.sql，建表，引入flyway maven plugin

   ```xml
   <plugin>
       <groupId>org.flywaydb</groupId>
       <artifactId>flyway-maven-plugin</artifactId>
       <version>6.2.1</version>
       <configuration>
               <url>jdbc:h2:file:${project.basedir}/target/test</url>
               <user>root</user>
               <password>root</password>
           </configuration>
   </plugin>
   ```

   运行flyway:migrate，迁移就成功了

5. ##### 配置和使用Mybatis

   1. 在`application.properties`中增加Mybatis的路径配置

      ```xml
      mybatis.config-location=classpath:db/mybatis/config.xml
      ```

   2. 新建`config.xml`添加配置信息

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration>
          <settings>
              <setting name="logImpl" value="LOG4J"/>
          </settings>
          <mappers>
              <mapper resource="db/mybatis/MyMapper.xml"/>
              <mapper class="xdml.dao.UserMapper"/>   注解使用的
          </mappers>
      </configuration>
      
      ```

   3. 建立`MyMapper.xml`

   4. 创建dao和entity实体类

   5. 如果我们创建Service服务，要和Dao中的服务的bean进行相关联，那么使用@Service注解，那么Spring就会把这两个文件当作bean来操作。

      比如我们要做一个排行榜，就新建一个实体类，dao和service服务

      如果我们想把dao和service服务相互依赖起来，

      - 传统的方式就会使用一个xml文件来进行相互关联
      - 在Springboot中，只要声明一个服务就行，加个`@Service`，这样就把bean进行管理了，这样`@Autowired`能进行自动装配识别

   

#### 6、后端渲染模板引擎

spring约定，放在resources/static下的东西，浏览器可以直接访问

**freemaker模板引擎**

1. 引入freemaker springboot starter的maven

   ```xml
   <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-freemarker</artifactId>
   </dependency>
   ```

2. 根据目录结构约束，建立，`resources/templates/index.ftl`文件

3. 在`application.properties`中增加配置，具体参照[官网](http://zetcode.com/springboot/freemarker/)

   ```xml
   spring.freemarker.template-loader-path=classpath:/templates
   spring.freemarker.suffix=.ftl
   ```

4. 模板引擎语法，使用一个名字去代替

   ```html
    <tr>
        <td>${index}</td>
        <td>${name}</td>
        <td>${score}</td>
    </tr>
   ```

5. Controller中使用ModelAndView来返回数据，有两个参数，

   1. 第一个是名字，会在templates中寻找

   2. 第二个是数据，数据格式类似Mybatis参数格式。

      ```java
       @RequestMapping("/")
          public ModelAndView index() {
              HashMap<String, Object> model = new HashMap<>();
              model.put("index", 1);
              model.put("name", "zhangsan");
              model.put("score", 100);
              return new ModelAndView("index", model);
          }
      ```

      


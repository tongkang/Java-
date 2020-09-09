### 动态代理和AOP

#### 1、AOP（Aspect-Oriented Programming）面向切面编程

1. 什么是AOP

   - 例如，一个Service有1000个方法，我们想在调用每个方法之前打印日志

     那么只要调用其中的某个方法，久拦截下来，这时候称**切面**，然后再做一些事情

   - AOP是面向切面编程，关注一个统一的切面

   - AOP和Spring是不同的东西

#### 2、AOP适合哪些场景

- 需要统一处理的场景

  - 日志
  - 缓存
  - 鉴权（有的人能访问，有的人不能访问）

- 如果用OOP来做需要怎么办？

  - 装饰器模式

    - 为了不破坏原来的方法，我们会在之前的方法上再套上一层方法，可以多层

      `static DataService service = new LogDecorator(new DataServiceImpl())`

    - 适用于**面向接口的编程**



#### 3、AOP的实现

1. JDK动态代理

- 优点：方便，不需要依赖任何第三方库	

- 缺点：功能受限，只适用于接口

  首先代理类要实现`InvocationHandler`类

2. CGLIB/ByteBuddy字节码生成

- 优点：强大，不受接口的限制
- 缺点：需要引用额外的第三方类库
  - 不能增强final类/final/private方法



#### 4、AOP与Spring

1. 引入springboot aop starter

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-aop</artifactId>
       <version>2.1.4.RELEASE</version>
   </dependency>
   
   ```

2. 如果是拦截的类方法，就要添加一个`spring.aop.proxy-target-class=true`

   这也是如何切换JDK动态代理和CGLIB的方式（**面试题**）

3. 假如要做一个Cache方法，新建`CacheAspect.java`来做切面的操作，

   1. 声明`@Aspect`注解，是告诉未来要出来有关切面的事情

   2. @Configuration是告诉Spring这个类是有关Spring的配置

   3. Cache切面做的操作类

      ```java
      @Aspect
      @Configuration
      public class CacheAspect {
          Map<String, Object> cache = new HashMap<>();
      
          @Around("@annotation(xdml.anno.Cache)")
          public Object cache(ProceedingJoinPoint joinPoint) throws Throwable {
              MethodSignature signature = (MethodSignature) joinPoint.getSignature();
      
              String methodName = signature.getName();
      
              Object cacheValue = cache.get(methodName);
      
              if (cacheValue != null) {
                  System.out.println("get value from cache");
                  return cacheValue;
              } else {
                  System.out.println("get value from database");
                  Object realValue = joinPoint.proceed();//让方法继续前进，做它应该做的事情
                  cache.put(methodName, realValue);
                  return realValue;
              }
          }
      }
      
      ```

      

4. 新建一个Cache注解类，是为了每一个标注了`@Cache`的方法，都能进入`CacheAspect.java`中来做操作

   ```java
   @Retention(RetentionPolicy.RUNTIME)
   public @interface Cache {
   }
   ```



#### 5、Redis

1、什么是Redis

- 广泛使用的内存缓存
- 常见的数据结构
  - String/List/Set/Hash/ZSet
- Redis为什么这么快
  - 完全基于内存
  - 优秀的数据结构设计
  - 单一线程，避免上下文切换开销
  - 事件驱动，非阻塞

2、安装和启动Redis

1. 安装：`docker pull redis`

   启动：`docker run -p 6739:6739 -d redis`

2. 添加Redis的配置，在`application.properties`

   ```xml
   spring.redis.host=localhost
   spring.redis.port=6379
   ```

3. 添加springboot redis starter

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

4. 添加一个Redis的`template`，springboot会自动扫描带bean注解的方法

   ```java
   @Configuration
   public class AppConfig {
   
       @Bean
       RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
           RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
           redisTemplate.setConnectionFactory(factory);
           return redisTemplate;
       }
   }
   ```

5. 使用redisTemplate

   ```java
   RedisTemplate<String, Object> redisTemplate; //声明对象
   redisTemplate.opsForValue().get()   //获取
   redisTemplate.opsForValue().set()   //写入
   ```

6. 有时候回遇到Java Object---->字节流，无法序列化问题

   在jdk自带有`Serializable`，进行序列化
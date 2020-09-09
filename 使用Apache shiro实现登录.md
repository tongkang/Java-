## 使用Apache shiro实现登录

#### 1、添加maven的core和spring两个依赖

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.5.1</version>
</dependency>
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.5.1</version>
</dependency>
```



#### 2、写一个config文件

- 这个里面配置哪些链接要拦截，哪些不要拦截
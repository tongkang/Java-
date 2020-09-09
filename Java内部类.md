### Java内部类

##### 1、成员内部类

> 成员内部类是定义类内部

```java 
public class Outer {
    
   public class Inner{
       
   }

}
```



1. 成员内部类可以被权限修饰符（public，private）所修饰

2. 成员内部类可以访问外部类的所有成员（包括private）成员

3. 成员内部类是默认包含了一个指向外部类对象的引用

4. 如同使用this一样，当成员名或方法名发生覆盖时，可以使用外部类的名字加this指定访问外部类成员，如：`Outer.this.name`

5. 成员内部类不可以定义`static`成员

6. 成员内部类创建语法：

   ```java
   Outer outer = new Outer();
   Outer.Inner inner = outer.new.Inner();
   ```

   

##### 2、局部内部类

> 局部内部类是定义在方法或者作用域中类，它和成员内部类的区别在于访问权限的不同

```java
public class Outer{
    public void test(){
        class Inner{
            
        }
    }
}
```

1. 局部内部类不能有访问权限修饰符

2. 局部内部类不能定义为`static`

3. 局部内部类不能定义`static`成员

4. 局部内部类默认包含外部类对象的引用

5. 局部内部类也可以使用`Outer.this`语法定制访问外部类成员

6. 局部内部类想要使用方法或域中的变量，该变量必须是`final`的

   > 在JDK 1.8后，没有`final`修饰，可以用`effectively final`，虽然使用`final`修饰编译器不会报错，

##### 3、匿名内部类

> 匿名内部类是与继承合并在一起的没有名字的内部类

```
public class Outer{
    public List<String> list=new ArrayList<String>(){
        {
            add("test");
        }
    };
}
```

1. 匿名内部类使用单独的块表示初始化块{}
2. 匿名内部类想要使用方法或域中的变量，该变量必须是`final`修饰的，JDK1.8后是`effectively final`也可以
3. 匿名内部类默认包含了外部类对象的引用
4. 匿名内部类表示继承所依赖的类

##### 4、静态内部类

> 嵌套类是用`static`修饰的成员内部类

```java
public class Outer {
    
   public static class Inner{
       
   }
}
```

1. 嵌套类是四种类中唯一一个不包含对外部类对象的引用的内部类

2. 嵌套类可以定义`static`成员

3. 嵌套类能访问外部类任何静态数据成员与方法

   > 构造函数可以看作静态方法，因此可以访问



#### 参考文章：

1、https://juejin.im/post/5a903ef96fb9a063435ef0c8
2、https://juejin.im/post/5dc69a39f265da4d365f33af#heading-8
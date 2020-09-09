Java抽象类

抽象类不能实例化，类的其他功能依然存在，成员变量，成员方法和构造方法的询问方式和普通类一样。

抽象类必须被继承，才能被使用。也是因为这个原因，通常在设计阶段决定要不要使用抽象类。

父类包含子类集合的常见的方法，但是由于父类本身是抽象类，所以不能使用。

在Java中抽象类表示是一种继承关系，一个类**只能继承一个**抽象类，而一个类却可以实现多个接口。

----

#### Lambda表达式

Lambad表达式样例

```java
button.addActionListener(new ActionListener() {
        public void actionPerformed (ActionEvent event){
            System.out.println("button clicked");
        }
 });

--变成Lambda表达式
button.addActionListener(event -> System.out.println("button clicked"));
```

Lambda表达式中无需指定类型，程序依然可以编译。因为javac根据程序的上下文在后台推断出参数（parame）的类型。



##### Lambda表达变体

1、不包含参数，使用空括号（）表示没有没有参数。该Lambda表达式实现了Runnable接口，该接口也只有一个run方法，没有参数，且返回类型未void。

`Runnable noArguments = () -> System.out.println("Hello World"); `

2、包含一个参数，可省略参数的括号

`ActionListener oneArgument = event -> System.out.println("button clicked"); `

3、Lambda表达式的主体不仅可以是一个表达式，而且可以是一段代码块，使用大括号（{}）将代码括起来。该代码块和普通方法遵守的规则别无二致，可以返回或抛出异常来退出。只有一行代码的Lambda表达式也可以使用大括号，用以明确Lambda表达式从何开始、到哪里结束。

```java
Runnable multiStatement = () -> {  
    System.out.print("Hello");     
    System.out.println(" World"); 
}; 
```

4、也可以包含多个多个参数的方法。这时就有必要思考怎样去阅读该Lambda表达式。这行代码并不是将两个数字相加，而是创建了一个函数，用来计算两个数字相加的接口，而是将两个数字相加的那行代码

```java
BinaryOperator<Long> add = (x, y) -> x + y; 
```

5、可以显式申明参数类型，此时就需要使用小括号将参数括起来，多个参数的情况也是如此。

```java
BinaryOperator<Long> addExplicit = (Long x, Long y) -> x + y; 
```

---

#### 2、引用值，而不是变量

 --Lambda 表达式中引用的局部变量必须是 final 或既成事实上的 final 变量

在匿名内部类中，如果要引用它所在方法里的变量，这时需要将变量什么未final。

Java8虽然放松了这一个限制，但是事实上必须是final。

虽然无需声明，但在Lambda表达式中，也无法作为非终态变量，坚持使用，就会报错。

这种行为也解释了为什么 Lambda 表达式也被称为**闭包**。未赋值的变量与周边环境隔离起来，进而被绑定到一个特定的值。

----

#### 3、流（stream）

1、区别**惰性求值**还是及**早求值**

- 只需看它的返回值。如果返回值是 Stream， 那么是惰性求值；
- 如果返回值是另一个值或为空，那么就是及早求值

##### collect()

​	是一个早求值操作，由于stream上很多都是惰性求值，在调用一系列方法后，需要d调用一个类似collect和及早求值的方法

##### map()

​	可以将一种类型的值转换成另外一种类型。map 操作就可以 使用该函数，将一个流中的值转换成一个新的流。

```java
例 3-9　使用 map 操作将字符串转换为大写形式
List<String> collected = Stream.of("a", "b", "hello")                                 
                        .map(string -> string.toUpperCase())                                                     .collect(toList()); 
```

##### flatMap()

flatMap 方法可用 Stream 替换值，然后将**多个** Stream 连接成**一个** Stream 

```java
List<Integer> together = Stream.of(asList(1, 2), asList(3, 4))                          	.flatMap(numbers -> numbers.stream())                                		    .collect(toList()); 

```

##### reduce

​	reduce 操作可以实现从一组值中生成一个值

```java
int count = Stream.of(1, 2, 3)                   
			.reduce(0, (acc, element) -> acc + element); 
 
assertEquals(6, count);
```


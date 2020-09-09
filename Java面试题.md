## Java面试题

#### 一、Java基础

##### 1、Java程序的运行原理

​	Java---->（通过编译complier==javac为：）`.class`字节码文件，存放于target/class目录下，--->JVM虚拟机中执行（JVM可以实现跨平台性）



##### 2、JDK和JRE的区别

- JDK=JRE+javac
- 如果仅仅只需要编译**JRE**就足够了，如果把编译文件转换为字节码.class文件，就需要**JDK**了



##### 3、Java的基本类型有哪些

- byte/short/int/long/float/double/boolean/char，一共8个
- String不是基本数据类型。可以看出基本数据类型都是关键字，而其他的都是一个类



##### 4、Java的参数传递是传值还是传引用

- Java世界中的一切对象都是指针（地址）
- 函数调用永远是传值

一般来说我们new一个对象（`Cat cat = new Cat()`），则个cat是一个地址，指向Cat()的指针，如果我们再调用函数（`foo(cat, 1, 0.1)`），这时候传递是值传递，但是是传递的是`Cat()`的**地址值**

```java
public class home {

   Cat cat;

    public static void main(String[] args) {
        //cat本身是一个号码牌，一个地址
        //例如，12345 地址
        Cat cat = new Cat();

        foo(cat, 1, 0123);
    }

    public static void foo(Cat cat, int i, double d) {
        //函数中拿到的cat是一个复制过来的号码牌，是12345地址
    }
}
```



##### 5、基本数据类型一定存放在栈中吗

- 如果在main中直接声明，就是放在栈中，如果`new Cat()`，这个对象中有定义的基本数据类型就是放在堆中



##### *6、StringBuffer/StringBuilder的区别/线程安全性？

- 如果没有额外声明，所有的类默认都是线程不安全的（除非文档明确指出）
- StringBuilder更快，但是线程不安全，更加常用
- StringBuffer稍慢，但是线程安全



##### 6、Object中有哪些方法

- equals()

  例如我们创建了两个字符串数据，我们知道他们是**相等**的，但不是**相同**的。

  判断相同的就用`==`来判断地址是否相同，相等就用`equals`

  Object默认的是`==`，所以我们需要对它进行覆盖，例如String的`equals()`方法

  **equals的约定：**

  1. 自反性，一个对象equals自己，一定返回true
  2. 对称性，`x.equals(y) == y.equals(x)`
  3. 传递性，`x.equals(y)` and `y.equals(z)` then `x.equals(z)`
  4. 持久性，对于多次调用x，y都应该返回一致的结果
  5. 非空：`x.equals(null)` should return `false`

- 方法上的`native`关键字

  例如写一个方法上有native，但是这个方法是和平台相关的，没有办法在class文件上显示出来，运行的时候在当前平台JVM（windows，Linux）上找一找这个方法的实现，找到了才能运行

  一般来说任何一个类文件都会被**类加载器**加载后到JVM上运行，在类加载器加载的过程中，native方法还是空白，只有在JVM上链接到这个方法，才能运行



#### 二、Java面向对象

##### 1、深拷贝和浅拷贝有什么区别

​	例如有一个对象1中，又引入了另一个对象2（地址），然后对象2又引入了另外一个对象3，我们称为这叫**引入链**

- **浅拷贝**：只复制对象1中的内容，包括引入地址，这个引入地址还是引入原来的连接对象
- **深拷贝**：复制引入链上的所有对象



##### 2、接口和抽象类有什么区别/联系？

- **Interface**：定义功能，只能包含方法（实现），不能包含成员变量，可以被实现若干次（implement）
- **Abstract class**：定义抽象的骨架实现，可以包含抽象方法或具体实现，也可以包含成员变量，单只能沿着一条路径继承（extends）

​	**interface**：

1. 一种功能，一种约定，一种合约
2. 最初的interface不能有实现（方法体{}），在Java8+后，就可以default方法体的 



##### 3、什么时候用接口/抽象类？

接口定义一个功能的时候更加灵活，还能实现别的接口

如果一旦定义了Abstract，如果要复用别的类的代码就不行的



##### 4、final的作用是什么

不可被覆盖性，不可被修改性



##### 5、override和overload有什么区别？

本质上是没有区别的

- overload：重载，同一个方法名有不同的参数
- override：重写，同一个方法名同一个参数，重写内容



#### 三、集合框架

##### 1、HashMap原理

[参考](https://www.bilibili.com/video/av71408100?from=search&seid=11367040715423843580)



##### 2、List/Set/Map区别

绝大部分时候使用List，去重的时候使用Set

- List：是一个有序的集合，所以list有个`get()`获取指定位置的元素。
- Set：是一个无序的集合，并且里面不能有相同的元素
- Map：是一个映射



##### 3、HashMap和HashTable区别？

- 都是基于哈希表的
- HashTable是线程安全的（因为加上了sychronized），HashMap不是（线程安全）。



##### 4、ConcurrentHashMap原理

首先，在并发的领域中，任何东西都需要锁，哪怕是无锁的操作（CAS），在x86的CPU中也需要一个锁来锁住主线

concurrentHashMap把里面的哈希桶分了很多区，我们把这个区称为**segment（分段锁）（比较重要的原理）**

如果一个数据来了，丢入其中一个segment，然后锁住，其他的线程来了就可以无干扰操作别的数据



##### 5、HashSet原理

像Hash Map的key值就是Set（不能包括重复的）

HashSet里面就有个HashMap，如果要往HashSet中传入值，它就直接传给了HashMap的key，value就随机建一个



##### 6、TreeSet原理

例：我们想要一串元素有序，并且插入/删除都高效

- ArrayList：有序，但是插入/删除损耗太大
- HashSet：插入删除是O(1)，但是无序
- LinkedHashSet：只能保证你插入的顺序，不能保证数据本身的数据，例如插入为：32451，那么就是32451

现在我们就来试试二叉平衡树（AVL），根节点的左边比他小，右边比他大，这么查找的效率就是O（logN），但是会出现一个种极端现象，成一条线，例如：2468，性能退化为链表，所以就让两边的子树高度相差不为1，

所以**TreeSet原理**中是基于红黑树（RB-tree），而红黑树相比于AVL，相对宽松的平衡，AVL平衡差不为1，所以红黑树有着更快的插入和删除，AVL则更适用于查找



##### 7、equals/hashCode区别/何时使用？

- hashCode约定：
  - 在一次Java程序运行种，同一个对象调用多次，始终返回相同的整数
  - 如果使用equels()判断两个对象相等，那么HashCode必须相等
  - 如果两个对象的equals不相等，那么hashCode也可能相等



##### 8、ArrayList/LinkedList区别

- ArrayList：是基于List的一个可扩容的数组
- LinkedList：它是一个双向链表和双端队列的实现



##### 9、哪些集合是线程安全的

- ~~HashTable~~/ConcurrentHashMap



##### 10、如何保证一个集合类不被修改？

- 使用Collections.unmodifiableXXX（List/Set），这个方法简单粗暴的把`set()`直接丢出异常`throw new UnsupportedOperationException()`，来控制不可修改
- Guava 的ImmutableXXX



##### 11、ArrayList/~~Vector~~区别

- 都是基于数组的自动扩容实现
- Vector线程安全，但是慢，被废弃了（Java1.2被List取代）
- ArrayList不安全，但是效率高



##### 12、Collection和Collections有什么区别？

Collection是一个类名的话，Collections就是对应的工具方法集合



#### 四、异常体系

##### 1、Java的异常体系结构

- Throwable 任何异常/错误的祖先类    （checked）
  - Exception 异常，可以从异常状态中恢复
    - RuntimeException 预料之外的异常，通常代表一个bug，unchecked
    - 其他Exception 预料之中的异常，代表编程中预期的异常状态，checked
  - Error无法恢复的异常状态 unchecked



##### 2、什么是checked/unchecked/runtime exception？

- checked：除RuntimeException之外的所有异常，都必须捕获
- unchecked：代表代码有bug
- runtime exception：是unchecked一个别的名字



##### 3、try/catch/finally的执行顺序？

try{}中尝试执行语句，一旦有异常抛出，后面的语句都不执行了，会以此从上到下依次捕捉异常，

finally中是无论前面发生了什么事情，finally里的语句还是会执行



##### 4、catch中return了，finally还会执行吗

会执行，finally类似做一个保险开关的作用

- ~~finally中return了，会怎么样？~~
  - 会覆盖前面的类容，idea会提示，非常不推荐这样做



##### 5、throw/throws的区别

- **throw**：在任何时候你都可以`throw new`一个异常出来，使当前方法被终止，就是一个普通语句
- **throws**：是在方法上声明一个签名，任何试图调用该方法的对象，都要抛出一个异常出来（checked/runtime Exception都行）



##### 6、final/finally/finalize的区别

- **final**：声明类的时候，类不能被继承，声明方法的时候，方法不能被覆盖，用于变量的时候，变量是不能更改的（这个更改指的是，不是数据不能更改，而是不能改变指向）

  ```java
  比如： final StringBuffer sb=new StringBuffer("abc");
  
  对sb重新赋值 sb=new StringBuffer("cde");
  
  会出现编译错误，被final修饰的变量是不能重新赋值的；
  
  但是 sb.append("ced");  //改变内容
  
  是可以编译通过的。
  ```

- **finally**：是在try{}catch{}中的一个保险开关，执行最后的资源清理工作

- **finalize**：

  在C++中，如果你声明了一个class A的函数，你就可以声明一个构造函数（constructor），并且你还可以声明一个析构函数（destructor）

  析构函数是用于类销毁的时候，做一些清理操作

  但是在Java中就没用了，因为一个程序的生命周期不归程序员来控制，有垃圾回收器来帮助处理

  所以在Java中有finalize，这是可以被用于做清理操作，实际上是没什么用的，你可以覆盖这个方法，来逃脱被回收的命运



#### 五、计算机体系原理

##### 1、进程和线程的区别？

- 进程占用独享的隔离的内存，文件等资源
- 同一进程内的线程共享内存/文件等资源

- 进程（process）：独享操作系统一块内存空间和文件描述符（file description），各个进程之间互不相干扰
- 线程：因为CPU >> Memory >> IO（他们之间的速度），因此在CPU在等待磁盘IO的时候，可以做更多事情，所以有了**线程（Thread）**。线程之间共享一块内存地址，文件等资源。



##### 2、Java的线程和操作系统线程是什么关系

- JVM的线程和OS的线程是一一对应的
- OS负责调度线程
- 因此，Java的线程模型饱受诟病
- 有了协程，并发问题还是会存在



#### 六，计算机网络

##### 1、TCP/UDP的区别？

- TCP：稳定可靠，纠错/重新发送，比较慢
  - 保证交付
  - 应用场景：对质量要求很高的场景
- UDP：快，质量低
  - 尽最大可能交付
  - 应用场景：对质量要求不高



##### 2、TCP协议的三次握手和四次挥手



##### 3、从浏览器发出请求到服务器接收到请求发生了什么

1. 首先解析域名的得到IP地址，首先在本机中的hosts文件中去寻找，如果没有，去DNS域名服务商中去找，后面的端口根据http（8080）或https（443）来获取
2. 这样就和远程的服务器建立了TCP连接，这个TCP连接有四个要素：来源IP(srcIP)，来源端口(srcPort)，目标IP(destIP)，目标端口(destPort)，最后建立HTTP连接



#### 七、算法与数据结构

##### 1、集合类中常见的数据结构

- **ArrayList**：自动扩容的数组，随机查找是常数时间
- **LinkedList**：双链表，可当作队列和栈
- **HashSet/HashMap**：哈希表
- **TreeSet/TreeMap**：红黑树
- **ConcurrentHashMap**：分段+哈希表
- **LinkedHashMap**：链表+哈希表



##### 2、链表和二叉查找树的时间复杂度区别

- ArrayList：寻址O(1)，更新O(n)，二分查找O(logn)
- LinkedList：寻址O(1)，更新O(1)，查找O(n)
- 哈希表：都是O(1)
- 红黑树：都是O(logn)
- 二叉查找树：都是O(logn)



#### 八、Web

##### 1、常见的状态码

- 2XX一切正常/3XX跳转/4XX客户端异常/5XX服务端异常
- 记：**200**：OK（请求已成功），**201**：Created（请求已被实现）
- **301**：Moved Permanently（被请求的资源已永久移动到新位置），**302**：Move Temporarily（请求的资源临时从不同的URI响应请求）
- **403**：Forbidden（服务器已经理解请求，但是拒绝执行它），**404**：Not Found（请求失败）
- **502**：Bad Gateway（作为网关或者代理工作的服务器尝试请求时，从上游服务器接收到无效的响应），**503**：Service Unavailable（由于临时的服务器维护或者过载，服务器当前无法处理请求，是临时的），**504**：Gateway Timeout（未能从上游服务器收到响应）



##### 2、GET/POST有什么区别？

- GET获取资源/POST用于发送数据
- GET请求信息都是header中，POST的表单数据则放在body中
- GET用于查询/获取资源，POST用于登录，修改数据
- GET是幂等的（多次发送和一次发送的结果完全相同）



##### 3、Cookie和Session有什么区别

- Cookie：随HTTP请求一起发送的一小段标识用户身份的信息
- Session：放在服务端的，用于通过Cookie和用户身份对应关系来鉴权的数据



##### 4、什么是跨域？如何解决跨域？

- 跨域：浏览器采用的**同源政策**，最初指，A网页设置的Cookie，B网页不能打开，除非这两个网页“同源”，同源指的是“三个相同”
  - 协议相同（http）
  - 域名相同（baidu.com）
  - 端口相同（80）
- 解决跨域
  - JSONP：只支持GET请求
  - CORS
  - Nginx转发



#### 九、类型与反射

##### 1、什么是反射

- 运行时行为，动态调用，和编译的时候没有关系 （运行时获取类中的所有方法）



##### 2、动态代理的原理是什么

例如，你要在原来一个类上进行功能的扩展，你这时候就需要代理

- 静态代理：直接拷贝代码过去，方便，但是会造成代码的冗余

- 动态代理：动态生成字节码完成一些功能扩展
- 动态代理可以使用：
  - JDK动态代理：方便，受限制（只能代理接口）
  - 字节码增强：不受限制，但是麻烦



##### 3、反射为什么性能较差

- JDK无法预测被调用的方法，因此无法实施优化



##### 4、什么是序列化

- 将Java对象变成字节流的过程叫序列化serialization
- 将字节流变成Java对象的过程叫做反序列化deserialization
- 可读的序列化：JSON/XML
- 不可读的序列化：Java自带的序列化



#### 10、Spring

##### 1、什么是IoC容器，为什么要IoC

- IoC控制反转，不需要手工控制各种对象的创建和依赖
- 只需要声明，这些都由容器帮你完成



##### 2、AOP与AOP原理

- 面向一个特定的切面编程，使得重复的逻辑可以在一起编写（比如：打印日志，缓存之类的），方便维护，减少重复



##### 3、bean的生命周期

- Bean从零开始：从指定的配置文件中加载BeanDefinition，然后Spring容器根据该定义开始进行Bean的装配工作
- Spring维护了一套完整的生命周期，每个Bean都可以定义在生命周期的任何一个阶段所完成的工作



##### 4、Spring Boot相对于Spring MVC有哪些改进？

- 自动化配置：
  - Spring MVC - 通过XML配置，手工指定
  - Spring Boot - 通过注解或自动化扫描
- 自带Servlet容器，可以直接启动
  - Spring MVC - 不自带Servlet容器，需要额外配置
  - Spring Boot - 直接打成jar包，可以直接启动
- Spring Boot对于微服务和快速开发更为友好
  - 优点：大大简化了开发配置的难度
  - 缺点：有很多潜在的默认约定，使得开发者更难深入了解其中的细节
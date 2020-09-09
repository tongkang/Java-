### ClassLoader

#### ClassLoader加载过程

- 自底向上检查类是否已经加载；
-   自顶向下尝试加载类。

1. 自定义类首先从缓存中查找Class是否已经加载，如果已经加载返回该Class；如果没有，则委托给父加载器，递归向上（App ClassLoader ---> Extensions ClassLoader ---> Bootstrap ClassLoader）
2. 委托到Bootstrap ClassLoader向下查找。如果Bootstrap ClassLoader缓存中没有找到Class，则在自己规定的路径`JAVA_HOME/JRE/lib`中或者`Xbootclasspath`选项指定路径的jar包中进行查找，找到了就返回，如果没有，交给子加载器Extensions ClassLoader。
3. Extensions ClassLoader查找`JAVA_HOME/jre/lib/ext`目录下或者`-Djava.ext.dirs`选项指定目录下的jar包；找到返回，找不到交给App ClassLoader。
4. App ClassLoader查找`classpath`目录下或者`-Djava.class.path`选项所指定的目录下的jar包或者class文件，如果找到就返回，没有就交给自定义的类加载器。
5. 自定义类的加载器还找不到的话，就抛出异常。

参考：https://www.jianshu.com/p/54e73aa94e4b

#### 双亲委托模式的好处

1. 避免重复加载，如果已经加载过一次的Class，就不需要再次加载，而从缓存中读取。
2. 更加的安全。如果不适用双亲委托模式，就可以自定一个String类来代替系统的String类，这回造成安全隐患，采用双亲委托模式会使String类在虚拟机启动时就被Bootstrap ClassLoader加载，所以用户自定的ClassLoader永远无法加载一个自己些的String，除非改变JDK中的ClassLoader搜索类的默认算法。并且两个类名一致并且被同一个类加载器加载的类，Java虚拟机才会认为他们是同一个类。

#### 自定义ClassLoader

```java
protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = loadClassData(name);
        return defineClass(name, classData, 0, classData.length);
    }

    private byte[] loadClassData(String className) throws ClassNotFoundException {
        try {
            return Files.readAllBytes(new File(className + ".class").toPath());
        } catch (IOException e) {
            throw new ClassNotFoundException(className);
        }
    }
```

代码地址：https://github.com/hcsp/implement-a-classloader/pull/27

#### 双亲委派加载模型在哪儿？为什么我们没有处理？

1. 双亲委派加载模型在ClassLoader类中的loadclass方法中，首先查询父类是否加载该class文件，有则直接返回，没有就调用findclass方法寻找；
2. 因为`Java`自带的基础class文件已经被预加载在JVM中，防止恶意修改基础文件；所以对于自定义ClassLoader大多数只需要覆盖findClass方法，自定义去加载class文件。


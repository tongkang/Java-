### Java的泛型

#### 1、泛型的擦除

#### 2、泛型的绑定

例如要做一个比较的方法

```java
public static Integer max(Integer a, Integer b) {
    return a > b ? a : b;
}

public static Long max(Long a, Long b) {
    return a > b ? a : b;
}

public static Double max(Double a, Double b) {
    return a > b ? a : b;
}
```

就要对参数类型进行重载，那么用了泛型后呢

```java
public static <T extends Comarable> T max(T a, T b){
    return a.compareTo(b) >= 0 ? a : b;
}
```

这就把a和b当作一个参数给T，这个T也就都继承了Comparable，使用compareTo来比较。
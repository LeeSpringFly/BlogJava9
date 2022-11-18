# Java 9 的一些特性

记录一些 java 9 的新用法

## 1  集合工厂方法

List，Set 和 Map 接口中新增 ```of()``` 方法用于快速创建实例

**例子**

``` java
public class DemoApplication {
    public static void main(String[] args) throws IOException {
        Set<String> set = Set.of("A", "B", "C");
        System.out.println(set);

        List<String> list = List.of("A", "B", "C");
        System.out.println(list);

        Map<String, String> map = Map.of("A","Apple","B","Boy","C","Cat");
        System.out.println(map);

        Map<String, String> map1 = Map.ofEntries (
                new AbstractMap.SimpleEntry<>("A","Apple"),
                new AbstractMap.SimpleEntry<>("B","Boy"),
                new AbstractMap.SimpleEntry<>("C","Cat"));
        System.out.println(map1);
    }
}
```

**输出**

> [C, B, A]
> [A, B, C]
> {C=Cat, B=Boy, A=Apple}
> {C=Cat, B=Boy, A=Apple}

**引用**

[Runoob Java 9  集合工厂方法](https://www.runoob.com/java/java9-collection-factory-methods.html)

## 2 try-with-resource

```try-with-resource``` 定义了一个或多个资源，一个资源必须是一个在程序结束后被关闭的对象，所有实现了 ```java.lang.AutoCloseable``` 的对象会被用于当作资源。

在 Java 9 中，如果有一个 final 或者等效于 final 的变量，也可用于 ``` try-with-resource``` 声名中

**例子**

``` java
public class DemoApplication {

    public static void main(String[] args) throws IOException {
        System.out.println(readDataJava9Earlier("test"));
        System.out.println(readDataJava9("test"));
    }

    static String readDataJava9Earlier(String message) throws IOException {
        BufferedReader br = new BufferedReader(new StringReader(message));
        try (BufferedReader brTmp = br) {
            return brTmp.readLine();
        }
    }

    static String readDataJava9(String message) throws IOException {
        BufferedReader br = new BufferedReader(new StringReader(message));
        try (br) {
            return br.readLine();
        }
    }

}
```

**输出**

> test
> test

**引用**

[Runoob Java 9  改进的 try-with-resources](https://www.runoob.com/java/java9-try-with-resources-improvement.html)

[Oracle 8 More Concise try-with-resources Statements](https://docs.oracle.com/en/java/javase/14/language/try-resources.html)

## 3 Optional 类

添加了 3 个改进的方法

- stream()
- ifPresentOrElse()
- or()

### 12.1 stream() 

**方法**

``` java
public Stream<T> stream()
```

**描述**

如果值存在，返回值的 Stream，否则返回 empty Stream

**例子**

``` java
public static void main(String[] args) {
        List<Optional<String>> list = Arrays.asList(
                Optional.empty(),
                Optional.of("A"),
                Optional.empty(),
                Optional.of("B")
        );

        // java 8
        List<String> listJava8 = list.stream()
                .flatMap(o -> o.isPresent() ? Stream.of(o.get()) : Stream.empty())
                .collect(Collectors.toList());
        System.out.println(listJava8);

        // java 9
        List<String> listJava9 = list.stream()
                .flatMap(Optional::stream)
                .collect(Collectors.toList());
        System.out.println(listJava9);
}
```

**输出**

> [A, B]
> [A, B]

### 12.2 ifPresentOrElse() 

**方法**

``` java
public void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)
```

**描述**

如果值存在，执行包含值得给定操作，否则执行基于 empty 的操作

**例子**

``` java
public static void main(String[] args) {
        Optional<Integer> optional = Optional.of(1);
        optional.ifPresentOrElse(
                x -> System.out.println("Value: " + x),
                () -> System.out.println("Not Present.")
        );

        optional = Optional.empty();
        optional.ifPresentOrElse(
                x -> System.out.println("Value: " + x),
                () -> System.out.println("Not Present.")
        );
}
```

**输出**

> Value: 1
> Not Present.

### 12.3 or() 

**方法**

```java
public Optional<T> or(Supplier<? extends Optional<? extends T>> supplier)
```

**描述**

如果值存在，返回该值的 Optional，否则返回给定值的 Optional

**例子**

``` java
public static void main(String[] args) {
        Optional<String> optional = Optional.of("Mahesh");
        optional = optional.or(() -> Optional.of("Not Present"));
        optional.ifPresent(x -> System.out.println("Value: " + x));

        optional = Optional.empty();
        optional = optional.or(() -> Optional.of("Not Present"));
        optional.ifPresent(x -> System.out.println("Value: " + x));
}
```

**输出**

> Value: Mahesh
> Value: Not Present

**引用**

[Runoob Java 9  改进的 Optional 类](https://www.runoob.com/java/java9-optional-class-improvements.html)

[Oracle](https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html)

## 扩展 A 更多内容

[runoob Java 9 新特性](https://www.runoob.com/java/java9-new-features.html)

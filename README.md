# Java 9 �I�ꍱ����

??�ꍱ java 9 �I�V�p�@

## 1  �W���H�ʕ��@

List�CSet �a Map �ڌ����V�� ```of()``` ���@�p������?��?��

**��q**

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

**?�o**

``` bash
[C, B, A]
[A, B, C]
{C=Cat, B=Boy, A=Apple}
{C=Cat, B=Boy, A=Apple}
```



**���p**

[Runoob Java 9  �W���H�ʕ��@](https://www.runoob.com/java/java9-collection-factory-methods.html)

## 2 try-with-resource

```try-with-resource``` ��?���꘢������?���C�꘢?���K?���꘢�ݒ���?���@��??�I?�ہC���L??�� ```java.lang.AutoCloseable``` �I?�ۉ��p������?���B

�� Java 9 ���C�@�ʗL�꘢ final ���ғ����� final �I?�ʁC��p�� ``` try-with-resource``` ������

**��q**

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

**?�o**

``` bash
test
test
```



**���p**

[Runoob Java 9  ��?�I try-with-resources](https://www.runoob.com/java/java9-try-with-resources-improvement.html)

[Oracle 8 More Concise try-with-resources Statements](https://docs.oracle.com/en/java/javase/14/language/try-resources.html)

## 3 Optional ?

�Y���� 3 ����?�I���@

- stream()
- ifPresentOrElse()
- or()

### 12.1 stream() 

**���@**

``` java
public Stream<T> stream()
```

**�`�q**

�@��?���݁C�ԉ�?�I Stream�C��?�ԉ� empty Stream

**��q**

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

**?�o**

``` bash
[A, B]
[A, B]
```



### 12.2 ifPresentOrElse() 

**���@**

``` java
public void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)
```

**�`�q**

�@��?���݁C?�s���?��?�葀��C��??�s� empty �I����

**��q**

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

**?�o**

``` bash
Value: 1
Not Present.
```



### 12.3 or() 

**���@**

```java
public Optional<T> or(Supplier<? extends Optional<? extends T>> supplier)
```

**�`�q**

�@��?���݁C�ԉ�??�I Optional�C��?�ԉ�?��?�I Optional

**��q**

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

**?�o**

``` bash
Value: Mahesh
Value: Not Present
```



**���p**

[Runoob Java 9  ��?�I Optional ?](https://www.runoob.com/java/java9-optional-class-improvements.html)

[Oracle](https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html)

## ?�W A �X�����e

[runoob Java 9 �V����](https://www.runoob.com/java/java9-new-features.html)
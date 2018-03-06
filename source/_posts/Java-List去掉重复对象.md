---
title: Java List去掉重复对象
date: 2018-03-06 23:01:08
categories: Java
tags: Java
---
### 一、 去除List中重复的String

```java
public List<String> removeStringListDupli(List<String> stringList) {
    Set<String> set = new LinkedHashSet<>();
    set.addAll(stringList);

    stringList.clear();

    stringList.addAll(set);
    return stringList;
}
```
或使用Java8的写法：

```java
List<String> unique = list.stream().distinct().collect(Collectors.toList());
```

### 二、 List中对象去重

比如现在有一个 Person类:

```java
public class Person {
    private Long id;

    private String name;

    public Person(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```
重写Person对象的equals()方法和hashCode()方法:

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    
    Person person = (Person) o;

    if (!id.equals(person.id)) return false;
    return name.equals(person.name);
}

@Override
public int hashCode() {
    int result = id.hashCode();
    result = 31 * result + name.hashCode();
    return result;
}
```

下面对象去重的代码：

```java
Person p1 = new Person(1l, "jack");
Person p2 = new Person(3l, "jack chou");
Person p3 = new Person(2l, "tom");
Person p4 = new Person(4l, "hanson");
Person p5 = new Person(5l, "胶布虫");

List<Person> persons = Arrays.asList(p1, p2, p3, p4, p5, p5, p1, p2, p2);

List<Person> personList = new ArrayList<>();
// 去重
persons.stream().forEach(
p -> {
    if (!personList.contains(p)) {
        personList.add(p);
    }
}
);
System.out.println(personList);
```
List 的contains()方法底层实现使用对象的equals方法去比较的，其实重写equals()就好，但重写了equals最好将hashCode也重写了。

可以参见:http://stackoverflow.com/questions/30745048/how-to-remove-duplicate-objects-from-java-arraylist 
http://blog.csdn.net/growing_tree/article/details/46622579

### 三、根据对象的属性去重

下面要根据Person对象的id去重，那该怎么做呢？ 写一个方法吧:

```java
public static List<Person> removeDupliById(List<Person> persons) {
    Set<Person> personSet = new TreeSet<>((o1, o2) -> o1.getId().compareTo(o2.getId()));
        personSet.addAll(persons);
    return new ArrayList<>(personSet);
}
```
通过Comparator比较器，比较对象属性，相同就返回0，达到过滤的目的。

再来看比较炫酷的Java8写法:

```java
import static java.util.Comparator.comparingLong;
import static java.util.stream.Collectors.collectingAndThen;
import static java.util.stream.Collectors.toCollection;

// 根据id去重
List<Person> unique = persons.stream().collect(
    collectingAndThen(
        toCollection(() -> new TreeSet<>(comparingLong(Person::getId))), ArrayList::new)
);
```
使用TreeSet去做去重，排序采用comparing比较器

保存原来的顺序，使用LinkedHashSet即可，如：

```java
List<Person> unique = persons.stream()..collect(Collectors.collectingAndThen(
    Collectors.toCollection(()-> new LinkedHashSet<>()), ArrayList::new));
```




还有一种写法:

```java
 public static <T> Predicate<T> distinctByKey(Function<? super T, Object> keyExtractor) {
    Map<Object, Boolean> map = new ConcurrentHashMap<>();
    return t -> map.putIfAbsent(keyExtractor.apply(t), Boolean.TRUE) == null;
}

// remove duplicate
persons.stream().filter(distinctByKey(p -> p.getId())).forEach(p -> System.out.println(p));
```

java8 确实简化了很多冗长的操作，精简了代码，小伙，研究java8去吧！

参考: 

[http://www.cnblogs.com/jizha/p/java_arraylist_duplicate.html](http://www.cnblogs.com/jizha/p/java_arraylist_duplicate.html)

[http://www.cnblogs.com/jizha/p/java_arraylist_duplicate.html](http://www.cnblogs.com/jizha/p/java_arraylist_duplicate.html)

来源:[http://blog.csdn.net/jiaobuchong/article/details/54412094](http://blog.csdn.net/jiaobuchong/article/details/54412094)
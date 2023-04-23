# 语法

## 基本I/O

```java
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String name = scanner.nextLine(); 
        System.out.printf("Hi, %s\n", name);
        scanner.close();
    }
}
```

输出包括：`System.out.println()` / `print()` / `printf()`

Scanner对象方便输入，读取对应的类型：`scanner.nextLine()` / `nextInt()` / `nextDouble()`

## 流程控制

Java12：switch表达式，没有fall-through，可以直接返回值

```java
String fruit = "apple";
int opt = switch (fruit) {
    case "apple" ->{
        System.out.println(1);
        yield 1;
    }
    case "pear", "mango" -> 2;
    default -> 0;
};
```

Java8：for each循环

```java
int[] ns = { 1, 4, 9, 16, 25 };
for (int n : ns) {
    System.out.println(n);
}
```

break label跳出多重循环(类似goto)

```java
for() for(){
    break out;
}
out:
```

## 数组

### Arrays(数组实用类)

引用类型排序，修改的是指针指向

```java
//数组排序并输出
import java.util.Arrays;
int[] a = { 2, 7, 3, 8, 6 };
Arrays.sort(a);
System.out.println(Arrays.toString(a));
```

降序

```java
import java.util.Collections;
Arrays.sort(a, Collections.reverseOrder());
```

其他

```java
int binarySearch(Object[] a，Object key);//返回索引，不在就返回-1。要求已升序排列
binarySearch(Object[] a, int l, int r, Object key)//在[l,r)区间内找
void fill(Object[] a, Object val)//全部填充
Object[] copyOf(Object[] original, int newLength)//复制。newLength较小就是前n个，较大就补0/false/null
boolean equals(Object[] a, Object[] a2)
```

### 二维数组

每行长度不一定相同

```java
import java.util.Arrays;
int[][] ns = {
    { 1, 2, 3 },
    { 4, 5 }
};
System.out.println(Arrays.deepToString(ns));
```

```java
int a[][]=new int[2][];
a[0]=new int[3];
a[1]=new int[2];
```

可以用二维数组的一行，初始化一维数组。

```java
int[] arr = ns[0];
```

## 字符串

```java
char[] s={'a','b'};//字符数组
//String: 引用类型对象，不可变
String s1 = new String(s);
String s2 = new String(s1);//两种初始化
String s3 = new String(s2,1,3);//从下标1开始，取3位
s3.charAt(1);//取单个字符
s3.substring(l,r)//取[l,r)子串
```

### 字符比较

判断引用类型的变量内容是否相等，使用s1.equals(s2)

switch匹配字符串时，比较内容相等

字符串对象一旦被配置，它的内容就是固定不变的。执行时会维护一个String Pool，有重复的就直接返回对象。没有被引用时，可能会被GC。

## 面向对象

### this变量

指向当前实例，命名不冲突时可以省略。但局部变量优先级更高

```java
class Person {
    private String name;
    public void setName(String name) {
        this.name = name; // this不可少，少了就变成局部变量name了
    }
}
```

### 可变参数

用`类型...`定义，相当于数组：

```java
class Group {
    private String[] names;
    public void setNames(String... names) {
        this.names = names;
    }
}
Group g = new Group();
g.setNames("Jia", "Yi");
g.setNames();
```

### 引用类型参数

调用方和接收方指向同一个对象。

### 构造方法

构造方法可调用其他构造方法，便于代码复用。语法是`this(…)`：

```java
class Person {
    private String name;
    private int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }
    public Person() {
        this("Unnamed"); // 调用另一个构造方法Person(String)
    }
}
```

### 嵌套类

Java支持嵌套类，嵌套类能访问`private`

### 继承

```java
class A{}
class B extends A{}
```

一个类有且仅有一个父类(不写就默认继承`Object`)

子类无法访问父类的`private`

子类构造方法，要先调用父类的构造方法`super()`。不写就默认无参。父类没有无参构造，就得在第一行语句手动调用父类构造方法。

#### sealed类(Java17)

```java
public sealed class Shape permits Rect, Circle {}
```

只有Rect, Circle才可以继承

#### 向上转型

把一个子类型安全地变为更加抽象的父类型

```java
class Student extends Person{}
Person p = new Student();
```

向下转型很可能会失败。失败的时候，Java虚拟机会报`ClassCastException`

### instanceof

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false
```

判断`instanceof`后，可以直接转型为指定变量，避免再次强制转型(Java14)

```java
Object obj = "hello";
if (obj instanceof String s) {
    // 直接使用变量s:
    System.out.println(s.toUpperCase());
}
```

### final

修饰方法不能被覆写

```java
public final String hello() {
    return "Hello";
}
```

修饰类不能被继承

修饰字段在初始化后不能被修改，但可在构造方法中初始化

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

修饰局部变量可以阻止被重新赋值

### 多态


```java
class Person {
    public void run()
    {System.out.println("Person.run");}
}
class Student extends Person {
    @Override
    public void run()
    {System.out.println("Student.run");}
}
```

在子类`Student`中，**覆写**了`run()`方法：

加上`@Override`可以让编译器帮助检查是否进行了正确的覆写，非必需。

#### 覆写Object

所有的`class`最终都继承自`Object`，`Object`定义了几个重要的方法：

- `toString()`：输出为`String`；
- `equals()`：是否逻辑相等；
- `hashCode()`：计算哈希值。

在必要的情况下，我们可以覆写`Object`的这几个方法。

#### super

可以通过`super`来调用父类被覆写的方法


#### 抽象类

父类的方法本身不实现功能，目的是让子类覆写，可以把父类的方法声明为抽象方法(必须申明为抽象类，无法实例化)

```java
abstract class Person {
    public abstract void run();
}
```

抽象类可以强迫子类实现其定义的抽象方法

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就必须覆写`run()`方法：

```java
Person s = new Student();
Person t = new Teacher();
s.run();
t.run();
```

好处在于，调用方法时不用关心`Person`类型变量的具体子类型

尽量引用高层类型，避免引用实际子类型，称为**面向抽象编程**

- 上层代码只定义规范（例如：`abstract class Person`）；
- 不需要子类就可以实现业务逻辑（正常编译）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心。

#### 接口

所有方法都是抽象方法，可以把抽象类改写为接口

```java
interface Person {
    void run();
    String getName();
}
```

所有方法默认都是`public abstract`。

一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字

```java
class Student implements Person {
    private String name;
    public Student(String name) {
        this.name = name;
    }
    @Override
    public void run() {
        System.out.println(this.name + " run");
    }
    @Override
    public String getName() {
        return this.name;
    }
}
```

一个类可以实现多个接口

`interface`继承`interface`，使用`extends`

`List`就是典型的接口

##### default方法

(Java8)

因为`interface`没有字段，`default`方法无法访问字段

```java
interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}
```

### 静态字段、方法

```java
class Person {
    public static int number;
    public static void setNumber(int value) {
        number = value;
    }
}
Person.number = 99;
System.out.println(Person.number);
```

`interface`可以有静态字段，必须为`public static final`(可省略不写)

### 包

JVM只看完整类名，包名不同，类就不同

```java
package ming;
```

包可以是多层结构，用`.`隔开。**没有父子关系**。`java.util`和`java.util.zip`是不同的包，没有继承关系。

 不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。`package`权限有助于测试。

 所有Java文件对应的目录层次要和包的层次一致。

```ascii
package_sample
├─ src
│   ├─ hong
│   │  └─ Person.java
│   └─ mr
│      └─ jun
│         └─ Arrays.java
└─ bin
   ├─ hong
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

#### 引用其他包

1. 写出完整类名

```java
package ming;
public class Person {
    public void run() {
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

2. 用`import`语句

```java
package ming;
import mr.jun.Arrays;
import mr.jun.*;//导入包的所有class
import static java.lang.System.*;// 导入所有静态字段、方法
public class Person {
    public void run() {
        Arrays arrays = new Arrays();
        out.println("Hello, world!");
    }
}
```

简单类名的查找顺序：

1. 当前`package`
2. `import`的包
3. `java.lang`

推荐使用倒置域名来确保唯一性

- org.apache
- org.apache.commons.log
- com.liaoxuefeng.sample

#### 编译

`-d`指定输出的`class`文件存放`bin`目录，`src/**/*.java`表示`src`目录下的所有`.java`文件

```
javac -d ./bin src/**/*.java
```

Windows不支持`**`这种搜索全部子目录的做法

```cmd
javac -d bin src\com\itranswarp\sample\Main.java src\com\itranswarp\world\Persion.java
```

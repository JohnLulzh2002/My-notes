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

---

判断引用类型的变量内容是否相等，使用s1.equals(s2)

---

switch匹配字符串时，是比较内容相等

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


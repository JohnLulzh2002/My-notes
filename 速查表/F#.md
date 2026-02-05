# F#基础

## REPL

```shell
dotnet fsi
dotnet fsi code.fsx #F# 脚本文件
```

输入 `#quit;;` 退出

## 新建、运行项目

```shell
dotnet new console -o HelloWorld -lang F#
dotnet run
```

## 变量

默认不可变，不可重赋值

```fsharp
let mutable a = 1	//强制可变
let s:float = 0.0	//指定类型
let first = "32"
let numberVersion = System.Int32.Parse first
let numberVersion = int first
```

判断等于用单个 `=`

## 输入输出

```fsharp
printfn $"a+b= {a+b}"	//无类型检查
printf "Hi %s" name
System.Console.WriteLine $"Hello {name}"
let str = System.Console.ReadLine()
```

| Specifier | Description |
|-|-|
| %s | strings and unescaped contents |
| %d  %i | decimal / integer |
| %b | Boolean |

`printf` 的返回值为 `unit` 类型，类似 `void` 或 `None`

## 流程控制

### 分支

```fsharp
if age > 65 
then printfn "Senior"
elif age > 18 
then printfn "Adult"
else printfn "Child"

let message = if age > 18 then "Adult" else "Child"
```

`then` 和 `else` 需要返回同种类型

### 循环

```fsharp
let list = [1; 2; 3; 4; 5]
for i in list do
     printf "%d " i
for i = 1 to 5 do
    printfn "%i " i	//1 2 3 4 5
for i = 5 downto 1 do
    printfn "%i " i	//5 4 3 2 1

let mutable quit = false
while not quit do
    printf "Guess a number: "
    let guess =int (System.Console.ReadLine())
    if guess = 11 then quit <- true
```

## 函数

### 定义

```fsharp
let add a b = a + b
let add a b =
    a+b
let convert (a: string) : int = int a
//传入传出 tuple
let f (t: int * int) =
    printfn $"{fst t} + {snd t}"
    (fst t + 1, snd t + 1)
f (1, 2)
```

### 组合与管道

```fsharp
let f a = f2 (f1 a)
let f = f1 >> f2
let f a = a |> f1 |> f2
```

## 集合（数组）

### List

不可变。实现方式是链表。

```fsharp
let primes = [2; 3; 5]
let cards = [
  "Ace"
  "King"
  "Queen"
]
let numbers = [ 1 .. 5 ] //1 2 3 4 5
let newList = 1 :: numbers//1 1 2 3 4 5
let concatList = listA @ listB
let primes1 = primes |> List.append [ 7; 11 ]
primes.Item 1   //0起点，按下标访问
```

属性：

- `Head`
- `List.Empty`
- `IsEmpty`
- `Length`
- `Tail` （除了首个元素的子序列）

函数式操作

```fsharp
List.iter (fun x -> printfn $"{x}") primes
List.map (fun person -> person.FirstName + person.LastName) people 
List.filter (fun i -> i % 2 = 0) nums

List.sort list
List.sortBy (fun (s: string) -> s.Length) fruits
let comparator a b=
    if a>b then 1
    elif a<b then -1
    else 0
List.sortWith comparator list

List.find (fun x -> x % 2 = 0) list //找第一个为真的。没有就抛 KeyNotFoundException
List.findIndex (fun x -> x % 2 = 0) list
let found = list |> List.tryFind (fun item -> item = 3)
match found with
| Some value -> printfn "%i" value
| None -> printfn "Not found"

List.sum [1 .. 5]
List.sumBy(fun item -> item.Cost) orderItems
List.average [1.0;2.0] //要求 float
```

### Array

定长 可变

### Sequence

使用时求值
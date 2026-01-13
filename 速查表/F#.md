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

```fsharp
if age > 65 
then printfn "Senior"
elif age > 18 
then printfn "Adult"
else printfn "Child"

let message = if age > 18 then "Adult" else "Child"
```

`then` 和 `else` 需要返回同种类型

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
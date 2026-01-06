# F#基础

## REPL

```shell
dotnet fsi
dotnet fsi code.fsx
```

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
```

## 输出

`printf`: stdout inline

`printfn`: stdout adds a newline character

`Console.WriteLine`: works in all .NET languages

```fsharp
printfn $"a+b= {a+b}"	//无类型检查
printf "Hi %s" name
```

| Specifier | Description |
|-|-|
| %s | strings and unescaped contents |
| %d  %i | decimal / integer |
| %b | Boolean |
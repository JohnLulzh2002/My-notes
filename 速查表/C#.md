新建.NET项目

```powershell
dotnet new console -o MyApp -f net6.0
```

小数类型：decimal

```csharp
decimal a = 7m 
```

字符串内插

```c#
string s = "world";
Console.WriteLine($"Hwllo, {s}!");
```

逐字文本

```c#
string projectName = "Project";
Console.WriteLine($@"C:\{projectName}\Data");
```

数组

```c#
int[] a=new int[10];
```


# C#

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

# LINQ

适用于 `IEnumerable<T>`（内存集合）和 `IQueryable<T>`（如 Entity Framework）  
所有方法均来自 `System.Linq` 命名空间

## 筛选（Filtering）

| 方法                      | 说明                             | 示例                              |
| ------------------------- | -------------------------------- | --------------------------------- |
| `Where(predicate)`        | 筛选满足条件的元素               | `people.Where(p => p.Age >= 18)`  |
| `OfType<T>()`             | 筛选出可转换为指定类型的元素     | `objects.OfType<string>()`        |
| `Distinct()`              | 去除重复项（使用默认相等比较器） | `numbers.Distinct()`              |
| `DistinctBy(keySelector)` | 按键去重（.NET 6+）              | `people.DistinctBy(p => p.Email)` |

## 投影与转换（Projection & Transformation）

| 方法                   | 说明                         | 示例                              |
| ---------------------- | ---------------------------- | --------------------------------- |
| `Select(selector)`     | 将每个元素映射为新形式       | `people.Select(p => p.Name)`      |
| `SelectMany(selector)` | 展平嵌套集合（类似 flatMap） | `orders.SelectMany(o => o.Items)` |
| `Cast<T>()`            | 将非泛型集合转为泛型         | `ArrayList.Cast<string>()`        |

## 排序（Ordering）

| 方法                     | 说明     | 示例                                        |
| ------------------------ | -------- | ------------------------------------------- |
| `OrderBy(keySelector)`   | 升序排序 | `people.OrderBy(p => p.Age)`                |
| `OrderByDescending(...)` | 降序排序 | `people.OrderByDescending(p => p.Salary)`   |
| `ThenBy(...)`            | 二级升序 | `.OrderBy(p => p.Dept).ThenBy(p => p.Name)` |
| `ThenByDescending(...)`  | 二级降序 | 同上，降序                                  |

链式排序必须以 `OrderBy` 开头。

## 分组（Grouping）

返回 `IGrouping<TKey, TElement>`

| 方法                           | 说明           | 示例                                       |
| ------------------------------ | -------------- | ------------------------------------------ |
| `GroupBy(keySelector)`         | 按键分组       | `people.GroupBy(p => p.Department)`        |
| `GroupBy(k => k, e => e.Name)` | 自定义元素投影 | `people.GroupBy(p => p.Dept, p => p.Name)` |

遍历分组示例：

```csharp
foreach (var group in people.GroupBy(p => p.Dept))
{
    Console.WriteLine($"部门: {group.Key}");
    foreach (var name in group) Console.WriteLine($"  - {name}");
}
```

## 连接（Joining）

| 方法                                              | 说明                 | 示例   |
| ------------------------------------------------- | -------------------- | ------ |
| `Join(inner, outerKey, innerKey, resultSelector)` | 内连接               | 见下方 |
| `GroupJoin(...)`                                  | 左外连接（分组形式） | 较少用 |

内连接示例：

```csharp
orders.Join(
    customers,
    order => order.CustomerId,
    cust => cust.Id,
    (order, cust) => new { order.Id, CustomerName = cust.Name }
)
```

EF Core 中通常用导航属性代替显式 Join。

## 分页与切片（Partitioning）

| 方法          | 说明                     | 示例                                      |
| ------------- | ------------------------ | ----------------------------------------- |
| `Take(n)`     | 取前 N 项                | `list.Take(10)`                           |
| `Skip(n)`     | 跳过前 N 项              | `list.Skip(10)`                           |
| `SkipLast(n)` | 跳过末尾 N 项（.NET 6+） | `list.SkipLast(2)`                        |
| `TakeLast(n)` | 取末尾 N 项（.NET 6+）   | `list.TakeLast(3)`                        |
| `Chunk(size)` | 分块（.NET 6+）          | `numbers.Chunk(5)` → `IEnumerable<int[]>` |

分页经典组合：

```csharp
var page = source.Skip(pageIndex * pageSize).Take(pageSize);
```

## 聚合与统计（Aggregation）

| 方法                | 说明                          | 示例                                 |
| ------------------- | ----------------------------- | ------------------------------------ |
| `Count()`           | 元素总数                      | `people.Count()`                     |
| `Count(predicate)`  | 满足条件的数量                | `people.Count(p => p.Age > 30)`      |
| `Sum(selector)`     | 求和                          | `orders.Sum(o => o.Amount)`          |
| `Min() / Max()`     | 最小/最大值                   | `numbers.Max()`                      |
| `MinBy(key)`        | 返回最小元素（按键，.NET 6+） | `people.MinBy(p => p.Age)`           |
| `MaxBy(key)`        | 返回最大元素（.NET 6+）       | `people.MaxBy(p => p.Salary)`        |
| `Average(selector)` | 平均值                        | `people.Average(p => p.Age)`         |
| `Aggregate(func)`   | 自定义累积                    | `numbers.Aggregate((a, b) => a + b)` |

## 判定与查找（Element & Existence）

| 方法                       | 说明                                 | 示例                                 |
| -------------------------- | ------------------------------------ | ------------------------------------ |
| `Any()`                    | 是否存在任意元素                     | `list.Any()`                         |
| `Any(predicate)`           | 是否存在满足条件的元素               | `people.Any(p => p.Name == "Alice")` |
| `All(predicate)`           | 是否所有元素都满足条件               | `numbers.All(n => n > 0)`            |
| `First()`                  | 获取第一个元素（无则抛异常）         | `list.First()`                       |
| `FirstOrDefault()`         | 获取第一个或默认值                   | `list.FirstOrDefault()`              |
| `Single()`                 | 获取唯一元素（多于1个或0个都抛异常） | 用于断言唯一性                       |
| `SingleOrDefault()`        | 获取唯一元素或默认值                 | 常用于主键查询                       |
| `Last() / LastOrDefault()` | 获取最后一个                         | `list.LastOrDefault()`               |
| `ElementAt(index)`         | 按索引获取                           | `list.ElementAt(2)`                  |

## 集合操作（Set Operations）

| 方法               | 说明                      | 示例             |
| ------------------ | ------------------------- | ---------------- |
| `Union(other)`     | 并集（自动去重）          | `a.Union(b)`     |
| `Concat(other)`    | 连接（不去重）            | `a.Concat(b)`    |
| `Intersect(other)` | 交集                      | `a.Intersect(b)` |
| `Except(other)`    | 差集（a 中有但 b 中没有） | `a.Except(b)`    |

## 其他实用方法

| 方法                         | 说明                                                 |
| ---------------------------- | ---------------------------------------------------- |
| `DefaultIfEmpty()`           | 若为空，返回含默认值的序列（用于左外连接）           |
| `Zip(other, resultSelector)` | 两序列按位置配对                                     |
| `Reverse()`                  | 反转顺序                                             |
| `AsEnumerable()`             | 将 `IQueryable` 转为 `IEnumerable`，切换到客户端执行 |
| `ToArray() / ToList()`       | 强制立即执行（结束延迟执行）                     |

## 关键注意事项

1. 延迟执行（Deferred Execution）  
   大多数 LINQ 方法返回 `IEnumerable<T>`，不会立即计算，直到调用 `ToList()`、`foreach` 或聚合方法。

2. 性能建议  
   - 把 `Where()` 放在链的前面，尽早过滤数据。  
   - 避免在链中多次遍历同一序列（如先 `Count()` 再 `ToList()`）。

3. Entity Framework 注意  
   - 不是所有方法都能转成 SQL（如 `DistinctBy()` 在旧版 EF 中不支持）。  
   - 使用 `AsEnumerable()` 可切换到内存计算（谨慎使用，可能拉取大量数据）。
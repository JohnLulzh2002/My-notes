# 基本结构

```fortran
program findv
    !calculate the velocity from the acceleration and time
    implicit none
    real::v,a=128.0,t=8.0
    v=a*t
    write (*,*) "V=", v
end program findv
```

一行一句，用`&`换行

## 编译

```shell
gfortran -o hello.exe hello.f90
```

# 类型

```fortran
integer::a,b
real::c=1.1
real,parameter::pi=3.14	!常量
integer*8::bignum	!8字节(默认4字节)
integer(kind=8)::bignum!同上
complex::d=(2.1e-1,-3)
character(9)::s="Hello"	!不够长则右侧补空格
character(len=9)::s
character(*),parameter::s="Hello"!常量可以不写长度
logical::t=.true.
```

## 派生类(结构体)

```fortran
type student
    integer::id
    real::score
end type student
type(student)::a,b
a%id=123
a%score=99.9
b=a
```

# 表达式

```fortran
r=-b+sqrt(b**2-4*a*c)
a/=b .and. a==1
"a""b"	!a"b，连用引号会转义
'Hello'//'World'!连接，'HelloWOrld'
'abcde'(2,4)	!子串，'bcd'
'abc'(2:2)		!取单个字符
"A"<"D"	!按ascii码，字典序比较
```

字符串用单双引号都行，`'"'`就是双引号字符

负号优先级最高

# 内置函数

|函数|备注|函数|备注|
|-|-|-|-|
|sin|弧度|achar|整数转字符(Ascii)|
|sind|角度|iachar|字符转整数|
|asin|反三角|len|字符串长度|
|int|向下取整|len_trim|字符串长度(忽略末尾空格)|
|nint|四舍五入|trim|去除末尾空格的子串|
|real|类型转换|adjustl|左侧空格挪到右侧(左对齐)|
|mod(a,b)|a模b|adjustr|右侧空格挪到左侧(右对齐)|

# I/O

```fortran
print *,"a=",a	!只能输出到终端
```

```fortran
read (*,*) a,b
write (*,*) "a=",a,"b=",b
```

## 参数

### 格式化

|Description|Specifier|Description|Specifier|
|-|-|-|-|
|integer|`rIw` or `rIw.m`|宽度|w|
|real|`rFw.d`|最小宽度|m|
|logical(输出T/F)|`rLw`|小数点后位数|d|
|character|`rA` or `rAw`|数量|n|
|连续n个空格|`nX`|列数|c|
|补空格到第c列|`Tc`|重复|r|
|空行|`n/`|

```fortran
write (*,'(i2,i4,i6.4,2x,a)') 42,123,42,'haha'
!42 123  0042  haha
!逗号可忽略
```

### advance

默认‘yes’换行

```fortran
write (*,'(a)',advance="no") "Enter n: "
read (*,*) n
```

### iostat

标志变量，若成功则置0，失败则>0

## 文件

```fortran
open(unit=10, file='out.txt', status="replace", &
    action="write", position="rewind", iostat=t)
if(t>0) stop "Cannot open file."
write(10, '(a/,i5/,f6.3)',iostat=s2) msg, a, pi
close(10)
```

|参数|解释|
|-|-|
|unit|序号，[10,99]范围内|
|file|文件名|
|status|“old”:已有, “new”:新建, “replace”:新建，若已有则替换|
|action|“read”, “write”, “readwrite”|
|position|“rewind”:beginning, “append”:end|
|iostat|标志变量，若成功则置0，失败则>0|

```fortran
rewind(<unit>)	!回到文件开头
backspace(<unit>)!回上一行
```

##  Iinternal read/write

Read and write directly from and to variables.

用于字符串与数字相互转换

字符串转数字：

```fortran
read(str,'(i5)') i
```

数字转字符串：

```fortran
write(str,'(i5)') i
```



# 分支循环

## if

```fortran
if(l==0)write (*,*) "Game Over."
```

```fortran
if(l==0)then
	print*,"Game Over"
else if(l==1)then
	print*,"a"
else
	print*,l
end if
```

## select case

```fortran
select case(h)
    case (0)
        print*."midnight"
    case (1:11)	!闭区间
        print*."am"
    case default
    	print*,"pm"
end select
```

## do

```fortran
do i=1,10,2	!闭区间
    print*,i
end do
```

```fortran
i=1.5
do while(i<=4.5)
    print*,"i=",i
    i=i+0.25
end do
```

```fortran
do
	print*,"haha"
end do
```

| Fortran | C        |
| ------- | -------- |
| cycle   | continue |
| exit    | break    |

# 数组

```fortran
integer,dimension(8)::a		![1,8]，8个
integer,dimension(-4:4)::b	![-4,4]，9个
integer,dimension(3)::c=1
integer,dimension(3)::c=(/2,1,3/)
integer,dimension(3)::c=(/(i,i=1,3)/)
a=1		!将所有值设为1
a(2)=2
```

编译时启用`-fcheck=bound`，溢出时会产生RE。不然可能导致内存泄漏。

```fortran
maxval(a)	!最大值
sum(a)		!求和
```

## 动态数组

可以先声明，运行时再申请空间。申请后长度就是确定的了

```fortran
integer,dimension(:),allocatable::d
integer::i
!input i
allocate(d(i),stat=t
```

##  Implied Do-Loop

可以嵌套

```fortran
print*,(a(i),b(i),i=1,3,2)
!同a(1),b(1),a(3),b(3)
```

## 多维数组

```fortran
integer,dimension(9,9)::a
integer,dimension(0:5,6)::b
```

```fortran
integer,dimension(:,:),allocatable::a
allocate(d(9,9),stat=t
```

# 其他语句、内置函数

## 报错

```fortran
stop 'Cannot open file'
```

## 随机数

[0,1)均匀分布

```fortran
real::x
call random_seed()
call random_number(x)
```

## 获取时间

至少包含一个参数

| 参数   | 类型                 | 解释                     |
| ------ | -------------------- | ------------------------ |
| date   | character(8)         | YYYYMMDD                 |
| time   | character(8)         | HHMMSS.SSS               |
| zone   | character(8)         | ±HHMM，本地时间与UTC差距 |
| values | integer,dimension(8) | 见下表                   |

| index | 解释                      |
| ----- | ------------------------- |
| 1     | 年                        |
| 2     | 月                        |
| 3     | 日                        |
| 4     | 本地时间与UTC差距（分钟） |
| 5     | 时                        |
| 6     | 分                        |
| 7     | 秒                        |
| 8     | 毫秒                      |

```fortran
call date_and_time(date=today)
```

## 命令行参数

```fortran
argc=command_argument_count()
call get_command_argument(number=1,value=str)
```

get_command_argument的参数：

| 参数   | 解释                             | intent |
| ------ | -------------------------------- | ------ |
| number | 第几个命令行参数                 | in     |
| value  | 字符串，长度大于实际参数长度即可 | out    |
| length | 实际长度                         | out    |
| status | 0:成功, -1:失败                  | out    |

# 子程序

```fortran
program name
    !声明
    !程序
contains
    !内部函数
end program name
    !外部函数
```

fortran传参时直接传地址，所以为防止修改主程序段的参数，要给出intent

| intent | 解释       |
| ------ | ---------- |
| in     | 只读       |
| out    | 必须赋新值 |
| inout  | 随意(默认) |

默认不能调用自己，递归函数/子程序需要在声明前加`recursive`

## 函数

形式

```fortran
<type> function <name>(<arguments>)<declarations>
!可换行声明参数
    <body of function>
    <name> = expression
    return
end function <name>
```

例如

```fortran
real function f2c(f)
real,intent(in)::f
    f2c=(f-32.0)/1.8
    return
end function f2c
```

递归

```fortran
<type> recursive function <name>(<arguments>)result(<variable>)
<declarations>
    <body of function>
    <variable> = expression
    return
end function <name>
```

## Subroutine

```fortran
subroutine sumAve(a,b,s)
real,intent(in)::a,b
real,intent(out)::s
	s=a+b
    return
end subroutine sumAve
```

递归

```fortran
recursive subroutine <name>(<arguments>)
<declarations>
    <body of subroutine>
    return
end subroutine <name>
```

# 模块

模块实现：

```fortran
module <module name>
    <global variable declarations>
contains
    <function definitions>
end module <module name>
```

主程序：

```fortran
program <name>
use <module name>
implicit none
!...
end program <name>
```

编译模块

```shell
gfortran -c module.f90
```

产生`源文件名.o`和`模块名.mod`

```sh
gfortran -o out.exe main.f90 module.o
```

只需列出`.o`文件，`gfortran`会自动寻找`.mod`文件
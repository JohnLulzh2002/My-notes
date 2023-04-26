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

编译

```shell
gfortran -o hello hello.f95
```

# 基础语法

一行一句，用`&`换行

## 变量声明

```fortran
integer::a,b
real::c=1.1
real,parameter::pi=3.14	!常量
integer*8::bignum	!8字节(默认4字节)
integer(kind=8)::bignum!同上
complex::d=(2.1e-1,-3)
character(9)::s="Hello"	!不够长则右侧补空格
character(len=9)::s
logical::t=.true.
```

## 表达式

```fortran
r=-b+sqrt(b**2-4*a*c)
a/=b .and. a==1
"a""b"	!a"b，连用引号会转义
'Hello'//'World'	!'HelloWOrld'
'abcde'(2,4)	!'bcd'
"A"<"D"	!按ascii码，字典序比较
```

字符串用单双引号都行，`'"'`就是双引号字符

负号优先级最高

## 内置函数

| 函数     | 备注     |
| -------- | -------- |
| cos      | 弧度     |
| int      | 向下取整 |
| nint     | 四舍五入 |
| real     | 类型转换 |
| mod(a,b) | a模b     |

# I/O

```fortran
print *,"a=",a	!只能输出到终端
```

```fortran
read (*,*) a,b
write (*,*) "a=",a,"b=",b
```

| Description      | Specifier        |
| ---------------- | ---------------- |
| 宽度             | w                |
| 最小宽度         | m                |
| 小数点后位数     | d                |
| 数量             | n                |
| 列数             | c                |
| 重复             | r                |
| integer          | `rIw` or `rIw.m` |
| real             | `rFw.d`          |
| logical(输出T/F) | `rLw`            |
| character        | `rA` or `rAw`    |
| 连续n个空格      | `nX`             |
| 补空格到第c列    | `Tc`             |
| Vertical Spacing | `n/`             |

```fortran
write (*,'(i2,i4,i6.4,2x,a)') 42,123,42,'haha'
!42 123  0042  haha
```

advance clause (默认‘yes’换行)

```fortran
write (*,'(a)', advance="no") "Enter n: "
read (*,*) n
```



# 分支循环

## if

```fortran
if(l==0)write (*,*) "Game Over."
```

```fortran
if(l==0)then
	print*,"Game Over"
else if(l==1)
	print*,"a"
else
	print*,l
end if
```

## select case

```fortran
select case (h)
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
    if(i==5) exit
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
	print*,"hahaha"
end do
```

# 文件

```fortran
open(unit=10, file=myfilename, status="replace", &
    action="write", position="rewind", iostat=s1)
if (s1>0) stop "Cannot open file."
write(10, '(a/, i5/, f6.3)', iostat=s2) msg, a, pi
close(10)
```

||Explanation|
|-|-|
|unit|序号，[10,99]范围内|
|file|文件名|
|status|“old”:已有文件, “new”:新建文件, “replace”:新建，若已有则替换|
|action|“read”, “write”, “readwrite”|
|position|“rewind”:beginning:, “append”:end|
|iostat|标志变量，若成功则置0，失败则>0|

```fortran
rewind(<unit number>)	!回到文件开头
backspace(<unit number>)!回上一行
```



# 特殊语句

```fortran
stop 'Cannot open file'	!报错
```


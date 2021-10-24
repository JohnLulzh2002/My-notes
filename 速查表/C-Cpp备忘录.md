# C语言

## 文件操作
```c
//单个文件：
freopen("in.txt","r",stdin);
freopen("out.txt","w",stdout);
//多个文件：
FILE* out=fopen("out.txt","w");//返回指针，失败NULL
fflush(FILE*);//清空缓存区
fclose(FILE*);//成功返0，失败返EOF。会先fflush
feof(FILE*);//结尾返非0，还有就返0
fgetc(),fputc(),fgets(),fputs(),fscanf(),fprintf();//第一个参数为FILE*即可
//块存储：
fread(void*,size_t size,size_t num,FILE*);//返回读入的元素个数
fwrite(void*,size_t size,size_t num,FILE*);
//文件随机访问：
fseek(FILE*,long step,int origin);//移动FILE*指向
rewind(FILE*);//移至开头
ftell(FILE*);//当前位置
//出错检测
ferror(FILE*);//正常返非0，出错返0
clearerr(FILE*);//错误标志归零（rewind、输入输出也可以）
```
| 常量     | 值   | 含义       |
| -------- | ---- | ---------- |
| SEEK_SET | 0    | 从前往后   |
| SEEK_CUR | 1    | 从当前往后 |
| SEEK_END | 2    | 从后往前   |

## main的参数

```c
main(int argc, char* argv[])
//argv[]={"str1","str2"}
```
## typedef数组

```c
typedef char Name[20];
Name a,b;
//same as
char a[20],b[20];
```

## 位域

```c
struct Bit{
	unsigned a:2;
	unsigned  :2;//占位，跳过2位
	unsigned  :0;//占位，填满当前字节
};
```

## 可变参数 (stdarg.h)

声明函数时，末尾用省略号`...`表示可变数量的参数

```c
int printf(const char* format, ...);
```

头文件`stdarg.h`定义了一些宏，可以操作可变参数。

1. `va_list`：数据类型，定义一个可变参数对象。它必须在操作可变参数时，首先使用。
2. `va_start`：函数，初始化可变参数对象。接受两个参数，第一个是可变参数对象，第二个是原始函数里面，可变参数之前的那个参数，用来为可变参数定位。
3. `va_arg`：函数，取出当前那个可变参数，每次调用后，内部指针就会指向下一个可变参数。接受两个参数，第一个是可变参数对象，第二个是当前可变参数的类型。
4. `va_end`：函数，清理可变参数对象。

```c
double average(int i, ...) {
  double total = 0;
  va_list ap;
  va_start(ap, i);
  for (int j = 1; j <= i; ++j) {
    total += va_arg(ap, double);
  }
  va_end(ap);
  return total / i;
}
```

示例中，`va_list ap`定义`ap`为可变参数对象，`va_start(ap, i)`将参数`i`后面的参数统一放入`ap`，`va_arg(ap, double)`用来从`ap`依次取出一个参数，并且指定该参数为 double 类型，`va_end(ap)`用来清理可变参数对象。

# C++

## 比C语言新增的零碎用法

### 引用

```cpp
int&a=b;
```

### 重载函数

同名不同参数

### 缺省参数

注意重载和缺省同时用时，可能产生二义性，编译出错

### 类型推导

```cpp
auto j=i;
decltype(i) j;
```




## 动态内存分配

```cpp
int*a=new int;
int*b=new int[10];
delete a;
delete[]b;
```

## 类的定义

如果先声明后定义，格式如下

```cpp
class A{
public:
    int b();
    int c(int);
};
int A::b()
{return 0;}
int A::c(int n)
{return n;}
```

成员有3种访问范围：

1. public
2. private
3. protected

不加关键字，就默认private

可以访问同类其他对象的private成员

```cpp
class A{
private:
    int n;
public:
    void setn(int t){n=t;}
    void avg(A&b,A&c){n=(b.n+c.n)/2;}
    void out(){cout<<n;}
};
A d,e,f;
d.setn(1);e.setn(3);
f.avg(d,e);
f.out();
```

### 构造函数(初始化)

同名函数。如：

```cpp
class A{
    int t;
public:
    A();
};
A::A()
{t=n;}
```

可以直接用初始化列表：

```cpp
class A{
    int t;
public:
    A(int n):t(n){};//对t用构造函数
};
```

如果构造函数有参数，新建对象时就必须带参数

```cpp
A a(2);
A b[2]={2,3};
A* p=new A(4);
```

构造函数可以重载

数组可以这样初始化：

```cpp
class A{
public:
    A(int n){}
    A(int n,int m){}
}
A arr[2]={1,A(2,3)};
```

### 复制构造函数

默认就是简单复制，可以自定义覆盖

建议(不一定)用常量，必须是引用

```cpp
A::A(const A&a);
```

调用：

```cpp
A a;
A b(a);//初始化
A c=a;//初始化
void fun(A n){}
fun(a);//作函数参数
void betterFun(const A& n){}//为避免复制开销，可以引用
A fun2(){
    A d;
    return d;//作返回值
}
A f;
f=a;//不调用复制构造函数，就是简单复制
```

g++会优化程序，可能会跳过复制构造函数

### 类型转换构造函数

一个参数的构造函数.赋值时会新建一个临时对象,再复制.

```cpp
A::A(int n);
A b=1;	//调用构造函数
b=2;	//调用类型转换构造函数
```

### 析构函数

销毁对象.主要是释放动态内存分配的空间.

```cpp
class A{
    int *p;
public:
    A() {p=new int[10];}
    ~A(){delete[]p;}
}
```

### 返回值为对象的函数

return时，先调用复制构造函数，生成临时对象。

函数较简单时gcc会优化，不复制

### 静态变量、函数

本质是全局变量/函数。所有同类对象共用.定义时便实例化。

必须在所有函数外初始化

占用空间不算在sizeof里

静态函数不实例化便可用(空指针、类名::函数名、对象名.函数名).不能调用非静态函数、变量、this指针.

### 封闭类

定义：有成员对象的类

先执行成员变量的构造函数。执行次序取决于声明次序（而不是初始化列表次序）

### 常量

#### 常量对象

对象的值不能改变

#### 常量成员函数

在函数声明的**后面**加const

```cpp
void Class::f() const{}
```

不能修改所作用的对象

不能调用非常量的成员函数

可以与非常量成员函数重载。调用时取决于是否为常量对象

### 友元(函数/类)

不是成员函数、成员类，但可以访问该类型私有成员

```cpp
class C{
private:
    int t;
	friend void f(C obj);
}
void f(C &obj){obj.t++;}
```

友元关系不传递，不继承

# STL

## sort (algorithm)

```cpp
sort(a+n1,a+n2,cmp());//from a[n1] to a[n2-1]
```

现成的比较函数

| 名称          | 功能描述 |
| ------------- | -------- |
| equal_to      | 相等     |
| not_equal_to  | 不相等   |
| less          | 小于     |
| greater       | 大于     |
| less_equal    | 小于等于 |
| greater_equal | 大于等于 |

用法

```cpp
greater<int>()
```

也可自定义比较函数

```cpp
struct cmp{
    bool operator()(const T & a, const T & b)const{
        //a必须在b前面，则ture
        //相等属于false的情况
    }
};
```

## 全排列 (algorithm)

```cpp
next_permutation(begin,end,cmp);
prev_permutation(begin,end,cmp);
```

返回值为bool类型，失败为false

### int

```cpp
int a[]={1,2,3,4,5};
next_permutation(a,a+5);
```

### string

```cpp
string s="abcde";
next_permutation(s.begin(),s.end());
```

## 二分查找 (algorithm)

```cpp
binary_search(a+n1,a+n2,value,cmp());//ture(找到)
//"找到"是指a<value||a>value ==false
```
```cpp
T* lower_bound(a+n1,a+n2,value,cmp());//返回值指向第一个>=value的元素，找不到就指向a[n2]
// 1    3    3    3    5
//a[0] a[1] a[2] a[3] a[4]
//假如搜索3,返回值指向a[1]
//假如搜索4,返回值指向a[4]
```

```cpp
T* upper_bound(a+n1,a+n2,value,cmp());//返回值指向第一个>value的元素，找不到就指向a[n2]
//在上一个案例中，假如搜索3,返回值指向a[4]
```

## 平衡二叉树 (set)

### multiset (可以重复)

```cpp
multiset<int> a;
multiset<int,cmp> a;//自定义排序规则
multiset<int>::iterator i;//迭代器
```
增
```cpp
a.insert(in);
```
顺序读取
```cpp
for(i=a.begin();i!=a.end();i++)
//不可比较大小、加减(只可++/--).end()指向最后元素的后面
    cout<<*i<<",";	//类似指针
```
查
```cpp
i=a.find(value);//返回迭代器，找不到就返回a.end()
i=a.lower_bound(value);//i前面的元素都在value前面
i=a.upper_bound(value);//i及其后面的元素都在value后面
```
删
```cpp
a.erase(i);
```

### set (集合,不重复)

```cpp
set<int> a;
pair<set<int>::iterator, bool> result=a.insert(in);
//result.second表示插入是否成功
a.size();//个数
```

## pair模板

```cpp
pair<T1,T2>;
//same as
struct{
    T1 first;
    T2 second;
};
```

```cpp
p=make_pair(a,b);
//same as
struct{
    T1 first;
    T2 second;
}p;
p.first=a;
p.second=b;
```

## 映射 (map)

### multimap

元素类型为pair<T1,T2>,T1 first为键,T2 second为值.按first排序

```cpp
multimap<T1,T2> a;
```

与multiset一样,有begin、end、insert、find、lower_bound、upper_bound函数，有迭代器

### map (不能重复)

```cpp
map<string,int> m;
m.insert(make_pair("alice",89));
cout<<m["Alice"];//可以直接把first当下标,结果是second
m["Bob"]=98;//可以读也可以写
```

类似set,insert()有返回值,其.second表明是否成功

## 变长数组 (vector)

作函数参数时，必须用引用

```cpp
vector<int> v;
vector<int> vv(maxSize,defaultValue);
vector<int>::iterator it;

int a[]={1,2,3,4,5};
vector<int> va(i+2,i+4);
```

增


```cpp
v.push_back(a);
v.insert(v.begin()+2);
```

删

```cpp
v.erase(v.begin()+2);
v.erase(v.begin()+2,v.begin()+4);
v.pop_back()
v.clear();
```

查

```cpp
v.size();//个数
cout<< v[1] <<endl;
for(it=v.begin();it!=v.end();it++)
    cout<<*it<<endl;
```

改 (algorithm)

```cpp
#include<algorithm>
reverse(v.begin(),v.end());
sort(v.begin(),v.end());
```
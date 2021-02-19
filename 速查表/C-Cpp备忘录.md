# C语言

### 文件操作
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

### main的参数

```c
main(int argc, char* argv[])
//argv[]={"str1","str2"}
```
### typedef数组

```c
typedef char Name[20];
Name a,b;
//same as
char a[20],b[20];
```

### 位域

```c
struct Bit{
	unsigned a:2;
	unsigned  :2;//占位，跳过2位
	unsigned  :0;//占位，填满当前字节
};
```

# C++

### 比C语言新增的零碎用法

##### 引用

```cpp
int&a=b;
```

##### 重载函数

##### 缺省参数

注意重载和缺省同时用时，可能产生二义性，编译出错

### 动态内存分配

```cpp
int*a=new int;
int*b=new int[10];
delete a;
delete[]b;
```

### 类的定义

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



# STL

### sort (algorithm)

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



### 二分查找 (algorithm)

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

### 平衡二叉树 (set)

##### multiset (可以重复)

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

##### set (集合,不重复)

```cpp
set<int> a;
pair<set<int>::iterator, bool> result=a.insert(in);
//result.second表示插入是否成功
a.size();//个数
```

pair模板

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

### 映射 (map)

##### multimap

元素类型为pair<T1,T2>,T1 first为键,T2 second为值.按first排序

```cpp
multimap<T1,T2> a;
```

与multiset一样,有begin、end、insert、find、lower_bound、upper_bound函数，有迭代器

##### map (不能重复)

```cpp
map<string,int> m;
m.insert(make_pair("alice",89));
cout<<m["Alice"];//可以直接把first当下标,结果是second
m["Bob"]=98;//可以读也可以写
```

类似set,insert()有返回值,其.second表明是否成功
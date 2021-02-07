## 常用数据

### 时间复杂度

| 复杂度           | 数量级      | 最大规模  | 复杂度   | 数量级 | 最大规模 |
| ---------------- | ----------- | --------- | -------- | ------ | -------- |
| $O(\log N)$      | >>$10^{20}$ | 很大      | $O(N^3)$ | 100    | 500      |
| $O(N^{\frac12})$ | $10^{12}$   | $10^{14}$ | $O(N^4)$ | 50     | 50       |
| $O(N)$           | $10^6$      | $10^7$    | $O(2^N)$ | 20     | 20       |
| $O(N \log N)$    | $10^5$      | $10^6$    | $O(N!)$  | 9      | 10       |
| $O(N^2)$         | $1000$      | $2500$    |          |        |          |

### 数组大小

|情形|数组大小|
|-|-|
|本地|5*10^8|
|128MB|33554432(3*10^7)|

### 各类型的最大最小值

|类型|min|max|
|-|-|-|
|unsigned short|0|65535|
|short|-32768|32767|
|unsigned int|0|4294967295|
|int|-2147483648|2147483647 (2e9)|
|unsigned long|0|4294967295|
|long|-2147483648|2147483647|
|long long|-9223372036854775808|9223372036854775807|
|unsigned long long|0|1844674407370955161|
|int64|-9223372036854775808|9223372036854775807|
|unsigned int64|0|18446744073709551615|

### 各类型的字节数

|类型|字节数|
|-|-|
|char|1|
|short int|2|
|int|4|
|unsigned int|4|
|float|4|
|double|8|
|long|8|
|long long|8|
|unsigned long|8|
|void*(指针)|8|

### 运算符优先级

|级别|运算符|
|-|-|
|1|[]数组下标、()函数调用、->成员访问、.成员访问、i++、i--|
|2|++i、--i、&取地址、*提领、+正号、-负号、~位反、!逻辑否、sizeof ()|
|3|强制类型转换|
|4|*乘法|
|5|+加法|
|6|<< 左移、>> 右移|
|7|<、<=、>、>=|
|8|==等于、!=不等于|
|9|&按位与|
|10|∧按位异或|
|11|\|按位或|
|12|&&逻辑与|
|13|\|\|逻辑或|
|14|? :三目运算符|
|15|=赋值|
|16|,逗号运算符|

---

## 语句、函数备忘录

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


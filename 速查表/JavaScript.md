# 语法

## 基本结构

```javascript
console.log('haha');
```

## 变量

弱类型

|变量|解释|
|-|-|
|String|'和"都行|
|Number|double|
|Boolean|true/false|
|Array|[1,'李']|
|Object|一切皆对象,都可储存进变量|

- undefined+number = NaN (Not a Number)
- undefined+string = 'undefined'+string

```javascript
var a,s='haha';	//'和"都行
let b;			//避免重复定义(ES6)
const pi=3.14;	//常量(ES6)
s.length	//4
s[1]		//'a'
```

数组

```javascript
let arr=['ha',3];//无需同类型
let arr2=[
  [1, 2],[3,],	//结尾可以有多余,
  [[4,5],6]
];
arr2[2][0][1];//5
arr.push(4)	//['ha',3,4]
arr.pop()	//删除末尾并返回
arr.shift()	//删除开头并返回
arr.unshift(5)//[5,3]
```

## 函数

```javascript
function f(s) {
  console.log("hi "+s);
}
```

lambda

```javascript
let f=()=>{
	console.log("ha");
};
let g=()=>5;
let h=x=>x*x;
```
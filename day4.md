# Today's schedule

今天的任务安排:

熟悉掌握应用javascript语法

将todomvc加载到数据集中  

熟悉云函数

# Yesterday's unfinished work

## 关于数据在localstorage上的存储

### 思路

* 先获取用户输入的信息
* 将获取到的信息添加到数组items中
* 用JSON.stringifly( )方法将items转换成字符串
* 用localstorage.SetItem(' key',value)键值形式存放到local中
* 数据展示:用localstorage.GetItem('key')通过获取键值拿出数据
* 将拿出的数据通过JSON.parse()方法转换成数组
* 将转换好的数组在created(){}中赋值给items

### 注意:

如果这里在created()中不设置不为空在执行赋值的话,可能会导致数组添加元素时报错

# JS学习

## querySelector()

通过querySelector()函数获取标题的引用

```javascript
let mytitle = document.querySelector('h1');
mytitle.textcontent = 'hello world !'
```

## 变量类型

```javascript
String:  let my = '李磊';
Number:  let my = 10;
Boolean: let my = true;
Array:   let my = [1,'雷','雨']
元素引用方法: my[0],my[1]
object   let my = document.querySelector('h1')

```

## 运算符

```javascript
赋值运算符:  '='
等于 		'==='
不等于		'!='
取非		!(my ===3);//false
```

## 条件语句

```javascript
if .... else
let ice = '巧克力'
if (ice ==='巧克力'){
    alert('我最喜欢吃巧克力')
}else{
    alert('巧克力才是我最喜欢的')
};
如果条件正确则执行第一个代码块如果错误则执行第二个代码块

```

## 函数

```javascript
示例: 两个参数进行乘法运算的函数
 function multiply(num1,num2){
     var res = num1*num2
     return res
 }
```

## 事件

```javascript
点击事件
document.querySelector('html').onclick = function(){
    alert('你好!')
}
这里采用的示匿名函数 等价与
let myhtml = document.querySelector('html')
myhtml.onclick = function(){
    alert('hello world!')
}
```

## 图像的切换器

```javascript
let myimage = document.querySelector('img')
	myimage.onclick = function(){
        let mysrc = myimage.getAttribute('src');
        if(mysrc ==='images/filed1'){
            myimage.setAttribute('src','images/field2')
        }else{
            myimage.setAttribute('src','images/field1')
        }
    }

prompt() 函数与alert()类似弹出一个对话框,但是这里需要用户输入数据
```

## addEventListener()

addEventListener('方式',function\)

方式例如点击 click

## foreach() 

```javascript
array.foreach()
对数组的每个元素执行一次提供的函数
const array1 = ['a','b','c']
array1.forEach(element => console.log(element));
(element可以为任意字符)
```

## indexOf()

```javascript
array.indexOf()
返回在数组中可以找到一个给定元素的第一个索引,如果不存在,则返回-1
const b = ['a','b','c']
console.log(b.indexOf('c'))//2
```

## every()

```javascript
array.every(测试方法)
测试一个数组内大所有元素是否都能通过某个指定函数的测试,它返回一个布尔值
注意:若收到一个空数组,此方法在一切请看下都会返回True
//先定义一个测试函数
const c = (v) => v<100;
const array = [1,20,50,99];
console.log(array.every(c))
```

## filter()

```javascript
创建一个新数组,其包含通过所提供函数的测试的所有元素
array .filter(a =>a.length>6)
const a = ['write','run','dump','jump']
const res = a.filter(b =>b.length>4)
console.log(res)//['write']
```

## map()

```javascript
创建一个新数组,其结果是数组中的每个元素都调用一个提供的函数后返回的结果
const a = [1,4,9,16]
const map1 = a.map(x=>x**0.5)
console.log(map1)
```

## some()

```javascript
测试数组中是不是至少有一个元素通过了被提供的函数测试,它返回的是一个boolean类型的值
const a = [1,2,3,4,5]
const even = (e) => e% 2 ===0;
console.log(a.some(e))// true
```

## 数据类型的转换

 

```javascript
var answer = 42;
answer = " Thanks for all the fish..."

x = "The answer is "+42 //"The answer is 42"
y = 42+ "is the answer" //"42 is the answer"

"37" -7 //30 数学运算
"37" + 7 //"377"  字符串运算

```

## 字符串转换为数字

* parseInt()  和 parseFloat()
* 将字符串转换为数字的另一种方法是使用一元加法运算符

```javascript
"1.1"+"1.1" = "1.11.1"
(+"1.1")+(+"1.1") = 2.2
+"1.1" + +"1.1" = 2.2
```

## 字面量

* 字面量是由语法表达式定义的常量
* 数字字面量
* 布尔字面量
* 浮点数字面量
* 整数
* 对象字面量
* RegExp literals
* 字符串字面量

### 数组字面量

```
var coffees = ["French Roast","Colombian","Kona"];
var a =[3]
console.log(a.length)//1
console.log(a[0])//3
```

### 数组字面值中的多余逗号

若你在同一行中连写两个逗号(,),数组中就会产生一个没有被指定的元素,其初始值是underfined.以下示例创建了一个名为fish的数组

```
var fish =["Lion","Angel"]
```


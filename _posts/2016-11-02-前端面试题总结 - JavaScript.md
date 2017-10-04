---
layout: post
title: "前端面试题总结 - JavaScript"
date: 2016-11-02
tags: 博客
---



[TOC]

#### １．javascript的typeof返回哪些数据类型

```javascript
alert(typeof [1, 2]); //object
alert(typeof 'leipeng'); //string
var i = true; 
alert(typeof i); //boolean
alert(typeof 1); //number
var a; 
alert(typeof a); //undefined
function a(){;};
alert(typeof a) //function
```



#### ２．例举3种强制类型转换和2种隐式类型转换?

强制（parseInt(),parseFloat(),Number()）

隐式（== ,!!）



#### ３．split()、join() 的区别

前者是切割成数组的形式，后者是将数组转换成字符串



#### ４．数组方法pop()　push()　unshift()　shift()

push()尾部添加 

pop()尾部删除

unshift()头部添加 

shift()头部删除



#### ５．事件绑定和普通事件有什么区别

普通添加事件的方法：

```javascript
var btn = document.getElementById("hello");
btn.onclick = function(){
	alert(1);
}
btn.onclick = function(){
	alert(2);
}
```

执行上面的代码只会alert 2 



事件绑定方式添加事件：

```javascript
var btn = document.getElementById("hello");
btn.addEventListener("click",function(){
	alert(1);
},false);
btn.addEventListener("click",function(){
	alert(2);
},false);
```

执行上面的代码会先alert 1 再 alert 2



##### 区别:

普通添加事件的方法不支持添加多个事件，最下面的事件会覆盖上面的，而事件绑定（addEventListener）方式添加事件可以添加多个。

addEventListener不兼容低版本IE

普通事件无法取消

addEventLisntener还支持事件冒泡+事件捕获




#### ６．IE和DOM事件流的区别

IE采用冒泡型事件 Netscape使用[捕获型事件](https://www.baidu.com/s?wd=%E6%8D%95%E8%8E%B7%E5%9E%8B%E4%BA%8B%E4%BB%B6&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YvnW01uHmYuWRzmyn3nWm40ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3En1TkPjb1rHm1rjfsn1TdPHmY) DOM使用先捕获后冒泡型事件 

1.执行顺序不一样

2.参数不一样

3.事件加不加on

4.this指向问题



#### ７．IE和标准下有哪些兼容性的写法

```javascript
var ev = ev || window.event
document.documentElement.clientWidth || document.body.clientWidth
var target = ev.srcElement||ev.target
```



#### ８．call和apply的区别

##### call方法

语法：call(thisObj，Object1,Object2...)

定义：调用一个对象的一个方法，以另一个对象替换当前对象。

说明：

call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 

如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 



##### apply方法

语法：apply(thisObj，[argArray])

定义：应用某一对象的一个方法，用另一个对象替换当前对象。 

说明： 

如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 

如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。



eg:

```javascript
function sum(a, b) {
  return a + b;
}
//带参数call的使用
var result = sum.call(window, 5, 10);
console.log(result);
//apply和call功能一样，只不过传参数的时候是一个数组
var result2 = sum.apply(window, [10, 20]);
console.log(result2);
```



#### ９．b继承a的方法

```javascript
function A( age, name ){ 
  this.age = age; 
  this.name = name; 
} 

A.prototype.show = function(){ 
  alert('父级方法'); 
} 

function B(age,name,job){ 
  A.apply( this, arguments ); 
  this.job = job; 
} 

B.prototype = new A();
var b = new A(14,'侠客行'); 
var a = new B(15,'狼侠','侠客'); 
```



#### １０．如何阻止事件冒泡和默认事件

canceBubble()　只支持IE

return false

stopPropagation()



#### １１．添加 删除 替换 插入到某个接点的方法

obj.appendChid()

obj.insertBefore()

obj.replaceChild()

obj.removeChild()



#### １２．javascript的本地对象，内置对象和宿主对象

本地对象为array obj regexp等可以new实例化

内置对象为gload Math 等不可以实例化的

宿主为浏览器自带的document,window 等



#### １３．window.onload 和document ready的区别

window.onload 是在dom文档树加载完和所有文件加载完之后执行一个函数Document.ready原生种没有这个方法，jquery中有 $().ready(function),在dom文档树加载完之后执行一个函数（注意，这里面的文档树加载完不代表全部文件加载完）。

$(document).ready要比window.onload先执行

window.onload　只能出来一次，

$(document).ready　可以出现多次

$(document).ready(function(){})　可以简写为　\$(function(){})



#### １４．”\==”和“===”的不同

前者会自动转换类型，后者不会



#### １５．javascript的同源策略

一段脚本只能读取来自于同一来源的窗口和文档的属性，这里的同一来源指的是主机名、议和端口号的组合



#### １６．JavaScript是一门什么样的语言，它有哪些特点？

javaScript一种[直译](http://baike.baidu.com/view/295412.htm)式[脚本语言](http://baike.baidu.com/view/76320.htm)，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的[解释器](http://baike.baidu.com/view/592974.htm)被称为JavaScript引擎，为[浏览器](http://baike.baidu.com/view/7718.htm)的一部分，广泛用于[客户端](http://baike.baidu.com/view/930.htm)的脚本语言，最早是在[HTML](http://baike.baidu.com/view/692.htm)网页上使用，用来给[HTML](http://baike.baidu.com/view/692.htm)网页增加动态功能。JavaScript[兼容](http://baike.baidu.com/subview/348591/5144387.htm)于ECMA标准，因此也称为[ECMAScript](http://baike.baidu.com/view/810176.htm)。



##### 基本特点

1．是一种解释性脚本语言（代码不进行[预编译](http://baike.baidu.com/view/176610.htm)）。

2．主要用来向[HTML](http://baike.baidu.com/view/692.htm)（[标准通用标记语言](http://baike.baidu.com/view/5286041.htm)下的一个应用）页面添加交互行为。

3．可以直接嵌入HTML页面，但写成单独的[js](http://baike.baidu.com/subview/9866/6241710.htm)文件有利于结构和行为的[分离](http://baike.baidu.com/view/351036.htm)。

4．跨平台特性，在绝大多数浏览器的支持下，可以在多种平台下运行（如[Windows](http://baike.baidu.com/view/4821.htm)、[Linux](http://baike.baidu.com/view/1634.htm)、Mac、Android、iOS等）。



#### １７．JavaScript的数据类型都有什么？

基本数据类型：String,boolean,Number,Undefined, Null

引用数据类型：Object(Array,Date,RegExp,Function)

那么问题来了，如何判断某变量是否为数组数据类型？

方法一.判断其是否具有“数组性质”，如slice()方法。可自己给该变量定义slice方法，故有时会失效

方法二.obj instanceof Array 在某些IE版本中不正确

方法三.方法一二皆有漏洞，在ECMA Script5中定义了新方法Array.isArray(), 保证其兼容性，最好的方法如下：

```javascript
if(typeof Array.isArray==="undefined")
{
  Array.isArray = function(arg){
        return Object.prototype.toString.call(arg)==="[object Array]"
  };  
}
```



#### １８．已知ID的Input输入框，希望获取这个输入框的输入值，怎么做？(不使用第三方框架)

```javascript
document.getElementById('ID').value
```



#### １９．希望获取到页面中所有的checkbox怎么做？(不使用第三方框架)

```javascript
var domList = document.getElementsByTagName('input')
var checkBoxList = [];
var len = domList.length;　　//缓存到局部变量
while (len--) {　　//使用while的效率会比for循环更高
　　if (domList[len].type == ‘checkbox’) {
    　　checkBoxList.push(domList[len]);
　　}
}
```



#### ２０．设置一个已知ID的DIV的html内容为xxxx，字体颜色设置为黑色(不使用第三方框架)

```javascript
var dom = document.getElementById('ID');
dom.innerHTML = 'xxxx'
dom.style.color = '#000'
```



#### ２１．当一个DOM节点被点击时候，我们希望能够执行一个函数，应该怎么做？

直接在DOM里绑定事件：\<div onclick=”test()”>\</div>

在JS里通过onclick绑定：xxx.onclick = test

通过事件添加进行绑定：addEventListener(xxx, ‘click’, test)

那么问题来了，Javascript的事件流模型都有什么？

“事件冒泡”：事件开始由最具体的元素接受，然后逐级向上传播

“事件捕捉”：事件由最不具体的节点先接收，然后逐级向下，一直到最具体的

“DOM事件流”：三个阶段：事件捕捉，目标阶段，事件冒泡



#### ２２．看下列代码输出为何？解释原因。

```javascript
var a;
alert(typeof a); // undefined
alert(b); // 报错
```

解释：Undefined是一个只有一个值的数据类型，这个值就是“undefined”，在使用var声明变量但并未对其赋值进行初始化时，这个变量的值就是undefined。而b由于未声明将报错。注意未申明的变量和声明了未赋值的是不一样的。



#### ２３．看下列代码,输出什么？解释原因。

```javascript
var a = null;
alert(typeof a); //object
```

解释：null是一个只有一个值的数据类型，这个值就是null。表示一个空指针对象，所以用typeof检测会返回”object”。



####  ２４．看下列代码，输出什么？解释原因。

```javascript
var undefined;
undefined == null; // true
1 == true;   // true
2 == true;   // false
0 == false;  // true
0 == '';     // true
NaN == NaN;  // false
[] == false; // true
[] == ![];   // true
```

undefined与null相等，但不恒等（===）

一个是number一个是string时，会尝试将string转换为number

尝试将boolean转换为number，0或1

尝试将Object转换成number或string，取决于另外一个对比量的类型

所以，对于0、空字符串的判断，建议使用 “\=\==” 。“===”会先判断两边的值类型，类型不匹配时为false。

那么问题来了，看下面的代码，输出什么，foo的值为什么？

```javascript
var foo = "11"+2-"1";
console.log(foo);
console.log(typeof foo);
```

执行完后foo的值为111，foo的类型为Number。



#### ２５．看代码给答案。

```javascript
var a = new Object();
a.value = 1;
b = a;
b.value = 2;
alert(a.value);
```

答案：2（考察引用数据类型细节）



#### ２６．已知数组var stringArray = [“This”, “is”, “Baidu”, “Campus”\]，Alert出”This is Baidu Campus”。

答案：alert(stringArray.join(“　”))



#### ２７．已知有字符串foo=”get-element-by-id”，写一个function将其转化成驼峰表示法”getElementById”

```javascript
function combo(msg){
   var arr=msg.split("-");
   for(var i=1;i<arr.length;i++){
       arr[i]=arr[i].charAt(0).toUpperCase()+arr[i].substr(1,arr[i].length-1);
   }
   msg=arr.join("");
   return msg;
}
```



#### ２８．var numberArray = [3,6,2,4,1,5\];（考察基础API）

1) 实现对该数组的倒排，输出[5,1,4,2,6,3]

numberArray.reverse()

2) 实现对该数组的降序排列，输出[6,5,4,3,2,1]

numberArray.sort(function(a,b){return　b-a})



#### ２９．输出今天的日期，以YYYY-MM-DD的方式，比如今天是2014年9月26日，则输出2014-09-26

var d = new Date();

// 获取年，getFullYear()返回4位的数字

var year = d.getFullYear();

// 获取月并变成两位，月份比较特殊，0是1月，11是12月

var month = d.getMonth() + 1;

month = month < 10 ? '0' +month : month;

// 获取日并变成两位

var day = d.getDate();

day = day < 10 ? '0' + day :day;

alert(year + '-' + month + '-' + day);



#### ３０．将字符串”\<tr>\<td>{\$id}\</td>\<td>{\$name}\</td>\</tr>”中的\{\$id}替换成10，\{\$name}替换成Tony （使用正则表达式）

> "\<tr>\<td>{\$id}\</td>\<td>{\$id}_{$name}\</td>\</tr>".replace(/{\\\$id}/g, '10').replace(/{\\\$name}/g, 'Tony');



#### ３１．为了保证页面输出安全，我们经常需要对一些特殊的字符进行转义，请写一个函数escapeHtml，将<, >, &, “进行转义

```javascript
function escapeHtml(str) {
return str.replace(/[<>”&]/g, function(match) {
    switch (match) {
      case “<”:
        return “&lt;”;
      case “>”:
        return “&gt;”;
      case “&”:
        return “&amp;”;
      case “\””:
        return “&quot;”;
      }
  });
}
```



#### ３２．foo = foo||bar ，这行代码是什么意思？为什么要这样写？

if(!foo) foo = bar; //如果foo存在，值不变，否则把bar的值赋给foo。

短路表达式：作为”&&”和”||”操作符的操作数表达式，这些表达式在进行求值时，只要最终的结果已经可以确定是真或假，求值过程便告终止，这称之为短路求值。










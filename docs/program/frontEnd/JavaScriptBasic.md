# JavaScript

## 数据类型

JS是一种弱类型的语言，它的数据类型只有在程序运行过程中，根据等号右边的值确定，并且数据类型是可变的。

Js数据类型可以简单地分为两大类：

值类型(基本类型)：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol。

引用数据类型：对象(Object)、数组(Array)、函数(Function)。

| 数据类型  | 说明                                          | 默认值    |
| --------- | --------------------------------------------- | --------- |
| String    | 字符串类型，如“Hello”                         | ""        |
| Number    | 数字型，包含整型和浮点型，如21、0.21          | 0         |
| Boolean   | 布尔值，如true/false，等价于1和0              | false     |
| Undefined | var a;声明了变量却没有给值，此时a = undefined | undefined |
| Null      | var a = null;此时变量a为空值                  | null      |

数字型：在JS中八进制前面加0，十六进制前面加0x；Infinity，表示无穷大，大于任何数值。-Infinity，表示无穷小，小于任何数值。NaN，表示一个非数值。

**数据类型转换**

可以使用`typeof`操作符来查看 JavaScript 变量的数据类型。

```javascript
typeof []	//object
typeof NaN	//number
```

Number转换为字符串：var num = 1;alert(num.toString())或者String()强制类型转换。

字符串转换为数字型：parseInt(string)函数、parseFloat(string)函数，将string类型转换为整数和浮点类型。

| 原始值    | 转换为数字 | 转换为字符串 | 转换为布尔值 |
| --------- | ---------- | ------------ | ------------ |
| “”        | 0          | ""           | false        |
| []        | 0          | ""           | true         |
| undefined | NaN        | "undefined"  | false        |
| 0         | 0          | "0"          | false        |

## JS数组

**数组的定义**

```javascript
var cars=new Array("Saab","Volvo","BMW");
var cars=["Saab","Volvo","BMW"];
```

**数组方法**

| 方法          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| join()        | 把数组转换成字符串                                           |
| push()        | 把里面的内容添加到数组末尾，并返回修改后的长度               |
| pop()         | 移除数组最后一项，返回移除的那个值，减少数组的length         |
| shift()       | 删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined |
| unshift       | 将参数添加到原数组开头，并返回数组的长度                     |
| sort()        | 将数组里的项从小到大排序                                     |
| reverse()     | 反转数组项的顺序                                             |
| concat()      | 将参数添加到原数组中。这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给 concat()方法传递参数的情况下，它只是复制当前数组并返回副本。 |
| slice()       | 返回从原数组中指定开始下标到结束下标之间的项组成的新数组。在只有一个参数的情况下， slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。 |
| splice()      | 删除、插入和替换。                                           |
| indexOf()     | 接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的开头（位置 0）开始向后查找 |
| lastIndexOf() | 接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的开头（位置 0）开始向后查找 |
| forEach()     | 对数组进行遍历循环，对数组中的每一项运行给定函数。这个方法没有返回值。参数都是function类型，默认有传参，参数分别为：遍历的数组内容；第对应的数组索引，数组本身 |
| map()         | 指“映射”，对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。 |
| filter()      | 过滤功能，数组中的每一项运行给定函数，返回满足过滤条件组成的数组 |
| every()       | 判断数组中每一项都是否满足条件，只有所有项都满足条件，才会返回true |
| some()        | 判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回true |

## 局部变量和全局变量

在函数外声明的变量作用域是全局的

在函数内声明的变量作用域是局部的 

使用 var 关键字声明的全局作用域变量属于 window 对象:

函数内使用 var 声明的变量只能在函数内容访问，如果不使用 var 则是全局变量。

使用var关键字声明的全局作用域变量属于 window 对象

在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。  

**获取全局对象**：

在web中，可以通过`window`,`self`或者`frames`取到全局对象，但是在web workers中，只有`self`可以。在Node.js中，它们都无法获取，必须使用`global`。

`globalThis`提供了一个标准的方式来获取不同环境下的全局`this`对象，可以不必担心它的运行环境。全局作用域中的this就是globalThis。

##  JavaScript 变量提升

JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。 

```javascript
console.log(a);  //undefined
var a = 123; 
```

因为变量a的声明被提到了作用域顶端。上面代码编译后应该是下面这个样子 

```javascript
var a;
console.log(a)
a = 123			//所以输出内容为 undeifend
```

具名函数的声明有两种方式：1. 函数声明式 2. 函数字面量式 

```javascript
//函数声明式
function bar () {}
//变量形式声明； 
var foo = function () {}
```

函数变量形式声明和普通变量一样提升的只是一个没有值的变量。 

函数声明式的提升现象和变量提升略有不同，函数声明式会提升到作用域最前边，并且将声明内容一起提升到最上边。

```javascript
bar()
var bar = function() {
  console.log(1);
}
// 报错：TypeError: bar is not a function
```

 ```javascript
bar()
function bar() {
  console.log(1);
}
//输出结果1
 ```

## JavaScript函数

### 函数声明

```javascript
function fn(x, y){
	return x + y;
}
```

函数表达式

在函数表达式存储在变量后，变量也可作为一个函数使用： 

```javascript
var f = function (x, y){
    return x + y;
}
var z = f(4, 3)
```

自调用函数

```javascript
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
```

函数可作为一个值使用

```javascript
function myFunction(a, b) {
    return a * b;
}
var x = myFunction(4, 3);
```

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}
```

**函数是对象，函数名是指针** 

### 函数内this的指向

一般来说，谁调用就指向谁

可以改变函数内部this的指向，常用的有bind()、call()、apply()方法

call的主要作用是实现继承

```javascript
function Father(name, age){
	this.name = name;
    this.age = age;
}
function Son(name,age){
	Father.call(this,name,age);
}
var son = new Son("Hello",15);
console.log(son);
```

apply(thisArg,[]),第二个参数必须是一个数组

```javascript
var o = {
	name : "zhangsan"
}
function fn(a ,b){
    console.log(this);
    console.log(a + b);
};
fn.apply(o,[12,45]);
```

bind()返回的是原函数改变this之后的新函数，只会生效一次。

```javascript
var o = {
	name : "zhangsan"
}
function fn(){
    console.log(this);
};
var f = fn.bind(o);
f();
```

### 箭头函数

ES6 允许使用箭头定义函数

```javascript
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。 

在箭头函数中，如果函数体中只有一句代码，并且代码的执行结果就是函数的返回值，函数体大括号可以省略。

```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

箭头函数有几个使用注意点。

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

上面四点中，第一点尤其值得注意。`this`对象的指向是可变的，但是在箭头函数中，它是固定的 

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });// id: 42
```

上面代码中，`setTimeout()`的参数是一个箭头函数，这个箭头函数的定义生效是在`foo`函数生成时，而它的真正执行要等到 100 毫秒后。如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致`this`总是指向函数定义生效时所在的对象（本例是`{id: 42}`），所以打印出来的是`42` 

箭头函数可以让`setTimeout`里面的`this`，绑定定义时所在的作用域，而不是指向运行时所在的作用域。 

### 立即执行函数

 ```javascript
(function(){
	console.log("立即执行函数");
})()
 ```

###  回调函数

有我们声明，但是不由我们调用的函数

```javascript
var arr = ["a","b","c"]
arr.forEach(x =>{console.log(x)})  
```

## 绑定事件的方式

> 在DOM元素中直接绑定
>
> 在JavaScript代码中绑定
>
> 绑定事件监听函数。

```javascript
<body>
		<button id="btn1">按钮1</button>
		<button id="btn2">按钮2</button>
		<button onclick="demo()">按钮3</button>

		<script type="text/javascript" >
			const btn1 = document.getElementById('btn1')
			btn1.addEventListener('click',()=>{
				alert('按钮1被点击了')
			})

			const btn2 = document.getElementById('btn2')
			btn2.onclick = ()=>{
				alert('按钮2被点击了')
			}

			function demo(){
				alert('按钮3被点击了')
			}
		</script>
</body>
```

## 对象

对象也是一个变量，但对象可以包含多个值（多个变量），每个值以 **name:value** 对呈现。 

**创建对象的方式**

* 字面量的方式

```javascript
var person = {name:"zhangsan", age:18, sayHello(){console.log("Hello, World")}}
person.sayHello()
```

*  Object构造函数创建 

```javascript
var Person = new Object();
Person.name = 'Nike';
Person.age = 29;
```

* 使用构造函数创建

**对象的分类**

自定义对象、内置对象（Math、Date、Array、String）、浏览器对象

**理解对象**

* **数据属性**

  描述符： 

  * configurable：表示能否通过 delete 删除属性从而重新定义属性。

  * enumerable：表示能否通过 for-in 循环返回属性。 

  * writable：表示能否修改属性的值。 

  * value:  包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined。

  ````javascript
  var person = {};
  Object.defineProperty(person, "name", {
   writable: false,
   value: "Nicholas"
  }); 
  ````

* **访问器属性**

  访问器属性不包含数据值；它们包含一对儿 getter 和 setter 函数（不过，这两个函数都不是必需的）。在读取访问器属性时，会调用 getter 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用setter 函数并传入新值，这个函数负责决定如何处理数据。

  描述符：
  
  * Get：在读取属性时调用的函数。默认值为 undefined。 
  * Set：在写入属性时调用的函数。默认值为 undefined。

## JavaScript闭包

闭包是一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。 避免变量的全局作用域。

指有权访问另一个函数作用域中变量的函数。简单理解，一个作用域可以访问另一个函数内部的局部变量。

JavaScript 支持嵌套函数。嵌套函数可以访问上一层的函数变量。 

```javascript
function fn(){
        var num = 10;
        function fb(){
            console.log(num);
        }
        fb();
    }
fn();
```

## 作用域与作用域链

### 作用域

作用域就是一个独立的地盘，让变量不会外泄、暴露出去。也就是说作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

```javascript
function outFun2() {
    var inVariable = "内层变量2";
}
outFun2();//要先执行这个函数，否则根本不知道里面是啥
console.log(inVariable); // Uncaught ReferenceError: inVariable is not defined
```

**全局作用域和函数作用域**

函数作用域只在函数内部起作用

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。 

第一种场景，内层变量可能会覆盖外层变量

```javascript
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

原因在于变量提升，导致内层的`tmp`变量覆盖了外层的`tmp`变量。 

第二种场景，用来计数的循环变量泄露为全局变量。 

```javascript
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
```

上面代码中，变量`i`只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

ES 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。 

### 作用域链

内部函数访问外部函数变量时，采用的是链式查找的方式

##  let 和 const 命令

let 声明的变量只在 let 命令所在的代码块内有效。

```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

在 ES6 之前，是没有块级作用域的概念的。ES6 可以使用 let 关键字来实现块级作用域。 

如上所示： let 声明的变量只在 let 命令所在的代码块 **{}** 内有效，在 **{}** 之外不能访问。

let变量不存在变量提升。

let不允许在相同作用域内，重复声明同一个变量。 

```javascript
// 报错
function func() {
  let a = 10;
  var a = 1;
}
```

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

## 变量的结构赋值

数组的解构赋值

事实上，只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值。 

```javascript
let [a, b, c] = [1, 2, 3];
```

解构赋值允许指定默认值。 

```javascript
let [x, y = 'b'] = ['a']; // x='a', y='b'
```

注意，ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值。所以，只有当一个数组成员严格等于`undefined`，默认值才会生效。 

```javascript
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

对象的解构赋值

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
```

字符串的解构赋值

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

函数参数的解构赋值

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

提取JSON中的数据

```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;
```

## Class的基本语法

JavaScript 语言中，生成实例对象的传统方法是通过构造函数。

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。 

```javascript 
class Test{
	constructor(name,age){
    	this.name = name;
        this.age = age;
     }
    say(){
        console.log(this.name,this.age);
     }
}
var person = new Test("Hello",15);
person.say();
```

 ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。 

继承父类

```javascript
class Son extends Test{
     constructor(name, age){
         super();
         this.name = name;
         this.age = age;
      }
      saySon(){
         console.log("??" + this.name + ",??" + this.age +"??");
      }
}
var son = new Son("zhangsan",18);
son.saySon();
```

ES6 的类，完全可以看作构造函数的另一种写法。 

```javascript
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

 与 ES5 一样，实例的属性除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上） 

**静态方法**

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'
```

 父类的静态方法，可以被子类继承。 

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```

由于`super`指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过`super`调用的。 

```javascript
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m // undefined
```

上面代码中，`p`是父类`A`实例的属性，`super.p`就引用不到它。如果属性定义在父类的原型对象上，`super`就可以取到。

## Set 和 Map 数据结构

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。`Set`本身是一个构造函数，用来生成 Set 数据结构。

```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
```

`Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```javascript
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5
```

Map的使用

```javascript
const map = new Map();
map.set("Hello", 1);
map.get("Hello");
map.has("Hello");
```

## Promise 对象

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大 

## 原型链

JavaScript规定，每一个构造函数都有一个prototype属性，指向另一个对象。

原型：构造函数在创建的过程中，系统自动创建出来与构造函数相关联的一个空的对象。可以由构造函数.prototype来访问到。

举例：在实例化对象p的过程中，系统就自动创建出了构造函数的原型，即Person.prototype.

注意：每个对象的`__proto__`属性指向自身构造函数的prototype；

constructor属性是原型对象的属性，指向这个原型对象所对应的构造函数。

构造函数、原型对象、实例化对象三者的关系： 

 ![img](https://img-blog.csdn.net/20170503151554392?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3BpY3lCb2lsZWRGaXNo/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 

原型链：每一个对象都有自己的原型对象，原型对象本身也是对象，原型对象也有自己的原型对象，这样就形成了一个链式结构，叫做原型链。

举例：

在上面这个例子中的p对象的原型链结构图如下：

p对象----->Person.prototype------->Object.prototype--------->null

对这个实例化对象而言，访问对象的属性，是首先在对象本身去找，如果没有，就会去他的原型对象中找，一直找到原型链的终点；如果是修改对象的属性，如果这个实例化对象中有这个属性，就修改，没有这个属性就添加这个属性。

##  Model语法

## DOM

### DOM树的增删改查

###  事件

事件的冒泡：所谓的冒泡就是事件的向上传导，当后代元素的事件被触发时，其祖先元素的相同事件也会被触发。可以将事件对象中的event.cancelBubble设置为true,即可取消冒泡 

事件的委派：将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件被触发时，会一直冒泡到祖先元素

事件的绑定：使用addEventLister可以为元素绑定多个函数

事件的传播

## BOM

浏览器对象模型（Browser Object Model (BOM)）

## TypeScript

* 数据类型

  ```javascript
  // 1. 可以对变量指定类型
  var name : string = "HelloWorld"
  // 2. 可以对函数指定类型
  function sum(num1:number, num2:number) :number{
      return num1 + num2;
  }
  // 3. 指定对象中包含哪些属性,在属性名后面加上?,表示属性是可选的
  let obj : {name:String, age?:number};
  obj = {name: '张三', age: 18}
             
  let obj : {name:string, [proName:string]:any}
  obj = {name:"张三", age:"18", gender:'男'} 
  // 4. 设置函数结构的类型声明           
  let fn : (a:number, b:number) => number;
  fn = function(a, b){
      return a+b;
  }
  // 5. 数组
  let arr:number[];
  arr = [1,2,3];
  let arr:Array<number>;
  arr = [1,2,3]
  // 6. 元组
  let h : [string,string];
  h = ["Hello", "World"]
  // 7. 枚举
  enum Color {Red, Green, Blue};
  let c: Color = Color.Blue; 
  ```






























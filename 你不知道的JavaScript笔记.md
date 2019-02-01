## 第1章 类型

+ null是唯一一个用typeof检测返回"object"的基本类型

+ typeof  function a() {} === "function"    //true

+ typeof [1, 2, 3] === "object"    //true

+ typeof处理undefined和undeclared变量时都会返回"undefined"

+ **typeof undeclared防止错误抛出**，例如a是一个全局变量：

  if (a) {console.log(a)}    //会抛出错误

  if (typeof a !== "undefined") {console.log(a)}    //这样是安全的





## 第2章 值

#### 数组

+ 数组通过数字索引，因其也是对象，可以包含字符串键值和属性（但不计算在数组长度内）
+ 如果字符串键值可以强制转换为十进制数字，就会被当做数字索引来处理

#### 字符串

+ 字符串不可变：字符串的成员函数不会改变其原始值，而是创建并返回一个新的字符串，而数组成员函数都是在其原始值上进行操作

#### 数字

+ 小数点前和小数点后小数部分后的0都可省略，比如0.42可以写成.42，42.0可以写成42.

+ 由上可知，42.toFixed(3)存在语法错误，因为42.优先识别为数字字面量

+ 整数检测：ES6中有Number.isInteger()，自己实现：

  if (!Number.isInteger) {

  ​    Number.isInteger = function (num) {

  ​        return typeof num == "number"  && num % 1 == 0

  ​    }

  }    //遵循这种函数定义规范，防止变量污染或者抛出错误

+ Number.isNaN(a)返回布尔值判断一个值是否为NaN

#### 值和引用

+ js中引用指向的是值本身而不是变量，所以一个引用无法更改为另一个引用的指向

+ 向函数传参如果是为参数赋值一个变量或者数组或者等副本，在函数内修改的只是副本，不会影响原始值；

  如果修改参数指向的数组的而不是进行赋值操作，就会改变原始值。P30可以多看几遍理解一下





## 第3章 原生函数

+ 原生函数可以当做构造函数来使用，但其构造出来的是封装了基本类型值的封装对象

+ 如果想要自行封装基本类型值，可以使用object(...)函数（不带new关键字）

+ 对象拆封，得到封装对象中的基本类型值，使用valueOf()函数：

  var a = new String("abc")

  a.valueOf();    //"abc"

+ 构造函数Array(..)/Error(..)不要求必须带new，可以自动补上

+ 不要创建和使用空单元数组

+ 尽量不要使用原生函数Array()/Object()/Function()/RegExp()来当构造函数使用

+ Symbol()当构造函数不能带new

+ 所有的函数都可以调用Function.prototype中的apply(), call(),和bind()

+ Function.prototype是一个空函数

  RegExp.prototype是一个“空”的正则表达式

  Array.prototype是一个空数组    //这些原型很适合用作默认值

+ 对于简单变量基本类型值，比如“abc”，如果要访问length属性等，js引擎会自动将其封装





## 第4章 强制类型转换


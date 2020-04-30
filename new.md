## JavaScript类型：关于类型，有哪些你不知道的细节

### 1. 为什么有的编程规范要求用void 0代替undefined?
因为JavaScript的代码undefined是一个变量，不是关键字，所以避免无意中被篡改，所以建议使用`void 0`来获取undefined值

### 2. JavaScript最新的数据类型
+ 简单类型
  - Null
  - Undefined
  - Boolean
  - String
  - Number
  - Symbol(独一无二的值)
  - BigInt(大整数 提案)
+ 复杂类型
  - Object

### 3. Undefined跟Null的区别
+ Undefined：表示未定义。
+ Null：表示定义了但是为空
+ Null 是关键字
+ 原始类型使用typeof除了null都能返回正确的类型。

### 4. 简介BigInt数据类型
JavaScript·所有的数字都保存成64位浮点数。64位浮点数的两大限制。  
(1). 数值的精度只能到53个二进制（相等于16个十进制），大于这个范围的整数，是无法精确的。  
(2). 大于或者等于2的1024次方的数值，JavaScript无法表示，返回`Infinity`  
BigInt就是为了解决这些问题。

### 5. BigInt和Number的区别
+ BigInt 类型的数据必须添加后缀n，各种进制后面也加n
+ BigInt 与普通整数是两种值，它们并不能相等
+ BigInt `typeof` 返回 `bigint`
+ BigInt 不能跟普通数值进行混合运算
+ BigInt 除法运算会舍去小数部分，返回一个整数

### 6. 0.1 + 0.2不是等于 0.3么？为什么JavaScript里不是这样的？
浮点数运算的精度丢失，当两个十进制数，转化为二进制运算后再转化回来，在这个转化过程中自然会有损失。但一般的损失往往在乘除运算中比较多，而JS在简单的加减法里也会出现这类问题。

其正确的做法为：

```js
console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON);
```

Number.EPSILON:极小的常量,它表示 1 与大于 1 的最小浮点数之间的差。

```js
//封装个方法
function withinErrorMargin (left, right) {
  return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
}

0.1 + 0.2 === 0.3 // false
withinErrorMargin(0.1 + 0.2, 0.3) // true

1.1 + 1.3 === 2.4 // false
withinErrorMargin(1.1 + 1.3, 2.4) // true
```

### 7. Symbol的用处
Symbol函数会生成一个独一无二的值，`typeof` 运算结果也是 `symbol`。

作为属性名的Symbol:
```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

Symbol 类型还可以用于定义一组常量，保证这组常量的值都是不相等的。
```js
const log = {};

log.levels = {
  DEBUG: Symbol('debug'),
  INFO: Symbol('info'),
  WARN: Symbol('warn')
};
console.log(log.levels.DEBUG, 'debug message');
console.log(log.levels.INFO, 'info message');


const COLOR_RED    = Symbol();
const COLOR_GREEN  = Symbol();

function getComplement(color) {
  switch (color) {
    case COLOR_RED:
      return COLOR_GREEN;
    case COLOR_GREEN:
      return COLOR_RED;
    default:
      throw new Error('Undefined color');
    }
}
```

消除魔术字符串

```js
//以前
function getArea(shape, options) {
  let area = 0;

  switch (shape) {
    case 'Triangle': // 魔术字符串
      area = .5 * options.width * options.height;
      break;
    /* ... more code ... */
  }

  return area;
}

getArea('Triangle', { width: 100, height: 100 }); // 魔术字符串

//symbol
const shapeType = {
  triangle: Symbol()
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });

```

## JavaScript对象：面向对象还是基于对象？

### 1. 什么是对象？
+ 一个可以触摸或者可以看见的东西
+ 人的智力可以理解的东西
+ 可以指导思考或者行动（进行想象或施加动作）的东西
  
### 2. JavaScript 对象的特征
+ 对象具有唯一标识性：即使完全相同的两个对象，也并非同一个对象。（唯一的内存地址）
+ 对象有状态：对象具有状态，同一对象可能处于不同状态之下。（属性）
+ 对象具有行为：即对象的状态，可能因为它的行为产生变迁。（方法）

js中对象独有的特色是：对象具有高度的动态性

### 3. js中的对象分类

+ 宿主对象：由js宿主(浏览器、Node)环境提供的对象，它们的行为完全由宿主环境决定的
+ 内置对象：由jk语言提供的对象
  - 固有对象：由标准规定，随着js运行时创建而自动创建的对象实例
  - 原生对象：可以由用户通过Array、RegExp等内置构造器或者特殊语法创建的对象
  - 普通对象：由{}语法、Object构造器或者class关键字定义类创建的对象，它能够被原型继承

## 闭包

一个函数包裹另外一个函数就行成了闭包。

### 闭包的作用

内部函数总是可以访问其外部函数中声明的参数和变量，即使在其外部函数被返回（退出堆栈结束生命周期）。

基于这个特性，js可以实现私有变量、特权变量、存储变量等

### 闭包如何工作的

当外部函数执行时，内部函数被作为外部函数的返回值。并且赋值给一个变量时，外部函数在完成生命周期被推出堆栈之前，会生成一个闭包，里面保存着外部函数的作用域中所有的变量，随着内部函数一起被返回。
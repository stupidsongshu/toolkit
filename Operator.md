> https://segmentfault.com/a/1190000011557368

## 浮点数取整
``` js
const x = 123.45;
x >> 0; // 123
~~x; // 123
x | 0; // 123
Math.floor(x); // 123

// 注意：前三种方法只适用于32个位整数，对于负数的处理上和 Math.floor是不同的
const y = -123.45;
y >> 0; // -123
~~y; // -123
y | 0; // -123
Math.floor(y); // -124
Math.ceil(y); // -123
```

## 两个整数交换数值
### 方法1
``` js
var a = 20, b = 30;

a ^= b;
b ^= a;
a ^= b;

a; // 30
b; // 20
```
### 方法2
``` js
var a = 1, b = 2;
a = [b, b = a][0];
```
### 方法3
``` js
var a = 1, b = 2;
a = [b][b = a, 0];
```

## 使用`~x.indexOf('y')`来简化`x.indexOf('y') > -1`
``` js
var str = 'hello world';
if (str.indexOf('lo') > -1) {
  // ...
}

if (~str.indexOf('lo')) {
  // ...
}
```

## `+` 运算符，`+x` or `String(x)`
+运算符可用于数字加法，同时也可以用于字符串拼接。如果+的其中一个操作符是字符串(或者通过 隐式强制转换可以得到字符串)，则执行字符串拼接；否者执行数字加法。

需要注意的时对于数组而言，不能通过valueOf()方法得到简单基本类型值，于是转而调用toString()方法。
``` js
[1,2] + [3, 4]; // "1,23,4"
```
对于对象同样会先调用`valueOf()`方法，然后通过`toString()`方法返回对象的字符串表示。
``` js
var a = {};
a + 123; // "[object Object]123"
```
对于`a + ""`隐式转换和`String(a)`显示转换有一个细微的差别：`a + ''`会对`a`调用`valueOf()`方法，而`String()`直接调用`toString()`方法。大多数情况下我们不会考虑这个问题，除非真遇到。
``` js
var a  = {
  valueOf: function() { return 42; },
  toString: function() { return 4; }
}

a + ''; // 42
String(a); // 4
```
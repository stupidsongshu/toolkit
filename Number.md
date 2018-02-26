## `parseInt()` or `Number()`
两者的差别之处在于解析和转换两者之间的理解。

解析允许字符串中含有非数字字符，解析按从左到右的顺序，如果遇到非数字字符就停止。而转换不允许出现非数字字符，否者会失败并返回NaN。
``` js
var a = '520';
var b = '520px';

Number(a); // 520
parseInt(a); // 520

Number(b); // NaN
parseInt(b); // 520
```
`parseInt`方法第二个参数用于指定转换的基数，ES5默认为10进制。
``` js
parseInt('10', 2); // 2
parseInt('10', 8); // 8
parseInt('10', 10); // 10
parseInt('10', 16); // 16
```
对于网上`parseInt(0.0000008)`的结果为什么为8，原因在于0.0000008转换成字符为"8e-7"，然后根据`parseInt`的解析规则自然得到"8"这个结果。

## 数字四舍五入
``` js
// v: 值，p: 精度
function round(v, p) {
  p = Math.pow(10, p >>> 31 ? 0 : p | 0);
  v *= p;
  return (v + 0.5 + (v >> 31) | 0) / p;
}

round(123.45353, 2); // 123.45
```

## 测试质数
### 方法1
``` js
function isPrime(n) {
  return !(/^.?$|^(..+?)\1+$/).test('1'.repeat(n))
}
```
## 方法二
``` js
function isPrime(n){
  if (n <= 3) {
    return true;
  } else {
    for (var i = 2; i < Math.sqrt(n); i++) {
      if (n % i == 0) {
        return false;
      }
    }
    return true;
  }
}
```

## 求正整数n以内的质数
``` js
function getPrimeRange(n) {
    var arr = []
    while (n > 0) {
        if (isPrime(n)) arr.push(n)
        n --
    }
    return arr.reverse()
}
```
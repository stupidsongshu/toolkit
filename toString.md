## 让数字的字面值看起来像对象
``` js
2.toString(); // Uncaught SyntaxError: Invalid or unexpected token

2..toString(); // 第二个点号可以正常解析
2 .toString(); // 注意点号前面的空格
(2).toString(); // 2先被计算
```
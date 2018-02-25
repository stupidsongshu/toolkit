## 随机返回一个范围的数字
``` js
//randomNumber(5,10)
//返回5-10的随机整数，包括5，10
//randomNumber(10)
//返回0-10的随机整数，包括0，10
//randomNumber()
//返回0-255的随机整数，包括0，255
randomNumber: function (n1, n2) {
    if (arguments.length === 2) {
        return Math.round(n1 + Math.random() * (n2 - n1));
    } else if (arguments.length === 1) {
        return Math.round(Math.random() * n1);
    } else {
        return Math.round(Math.random() * 255);
    }
}
```

## 随机产生颜色
``` js
randomColor: function () {
    //randomNumber是上面定义的函数
    //写法1
    //return 'rgb(' + this.randomNumber(255) + ',' + this.randomNumber(255) + ',' + this.randomNumber(255) + ')';
    //写法2
    return '#' + Math.random().toString(16).substring(2).substr(0, 6);
    //写法3
    //var color='#',_index=this.randomNumber(15);
    //for(var i=0;i<6;i++){
    //color+='0123456789abcdef'[_index];
    //}
    //return color;
}
//这种写法，偶尔会有问题。大家得注意哦
//Math.floor(Math.random()*0xffffff).toString(16);
```

## 随机生成16进制颜色代码
``` js
'#' + ('00000' +(Math.random() * 0x1000000 << 0).toString(16)).slice(-6)
```

## 设置url参数
``` js
//设置url参数
//setUrlPrmt({'a':1,'b':2})
//result：a=1&b=2
setUrlPrmt: function (obj) {
    var _rs = [];
    for (var p in obj) {
        if (obj[p] != null && obj[p] != '') {
            _rs.push(p + '=' + obj[p])
        }
    }
    return _rs.join('&');
}
```
## 获取日期时间戳
``` js
// 获取指定时间的时间缀
new Date().getTime();
(new Date()).getTime();
(new Date).getTime();
// 获取当前的时间缀
Date.now();
// 日期显示转换为数字
+new Date();
```

## 到某一个时间的倒计时
``` js
//getEndTime('2017/7/22 16:0:0')
//result："剩余时间6天 2小时 28 分钟20 秒"
getEndTime: function (endTime) {
    var startDate = new Date(); //开始时间，当前时间
    var endDate = new Date(endTime); //结束时间，需传入时间参数
    var t = endDate.getTime() - startDate.getTime(); //时间差的毫秒数
    var d = 0,
        h = 0,
        m = 0,
        s = 0;
    if (t >= 0) {
        d = Math.floor(t / 1000 / 3600 / 24);
        h = Math.floor(t / 1000 / 60 / 60 % 24);
        m = Math.floor(t / 1000 / 60 % 60);
        s = Math.floor(t / 1000 % 60);
    }
    return "剩余时间" + d + "天 " + h + "小时 " + m + " 分钟" + s + " 秒";
}
```

## 日期格式化
``` js
// format(new Date(), 'yyyy-MM-dd hh:mm:ss')
// result: 2018-02-24 17:45:24
function format(x, y) {
    var z = {
        y: x.getFullYear(),
        M: x.getMonth() + 1,
        d: x.getDate(),
        h: x.getHours(),
        m: x.getMinutes(),
        s: x.getSeconds()
    };

    return y.replace(/(y+|M+|d+|h+|m+|s+)/g, function(v) {
        return ((v.length > 1 ? "0" : "") + eval('z.' + v.slice(-1))).slice(-(v.length > 2 ? v.length : 2));
    });
}
```

``` js
// new Date().format('yyyy-MM-d h:m:ss:S')
// result: 2018-02-24 17:58:20:767
Date.prototype.format = function(fmt) {
    var o = {
        "M+": this.getMonth() + 1, //月份
        "d+": this.getDate(), //日
        "h+": this.getHours(), //小时
        "m+": this.getMinutes(), //分
        "s+": this.getSeconds(), //秒
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度
        "S": this.getMilliseconds() //毫秒
    };

    if (/(y+)/.test(fmt)){
        fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    }

    for (var k in o) {
        if (new RegExp("(" + k + ")").test(fmt)){
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        }
    }

    return fmt;
}
```

## 快速得到标准的时间格式
我们经常有这样的需求，把当前的时间转化为2018-02-25 22:41:08的形式，通常的实现方式是
``` js
var formatNum = function(n) {
    return n > 10 ? n : '0' + n;
};

var getCurrDateTime = function() {
    var d = new Date(),
        year = d.getFullYear(),
        month = d.getMonth() + 1,
        day = d.getDate(),
        hour = d.getHours(),
        min = d.getMinutes(),
        sec = d.getSeconds();
    return year +
        '-' +
        formatNum(month) +
        '-' +
        formatNum(day) +
        ' ' +
        formatNum(hour) +
        ':' +
        formatNum(min) +
        ':' +
        formatNum(sec);
};
```
我们可以使用正则简化一下
``` js
var getCurrDateTime = function() {
    var d = new Date();
    var dt = d.getFullYear() +
        '-' +
        (d.getMonth() + 1) +
        '-' +
        d.getDate() +
        ' ' +
        d.getHours() +
        ':' +
        d.getMinutes() +
        ':' +
        d.getSeconds();
    return dt.replace(/([-:\s])(\d{1})(?!\d)/g, '$10$2');
};
```
`/([-:\s])(\d{1})(?!\d)/`这段正则的意思是，匹配前面是`-`或者`:`或者空格的并且后面不是数字的单个数字，像2018-2-25 22:41:8这里面的数字2和8会被匹配。然后`$1`是指匹配到的`-`或者`:`或者空格，`$2`是指匹配到的单个数字，`$10$2`即实现了单个数字前面补0的效果。

## 数字0-6到“一二三四五六日”的对应
Javascript中的日期对象得到星期时是使用getDay()方法，得到的是0-6的数字
### 方法1
``` js
// var chineseWeek = getChineseWeek(3); // chineseWeek为三
var getChineseWeek = function(n) {
    var w = '';
    switch(n) {
        case 0:
            w = '日';
            break;
        case 1:
            w = '一';
            break;
        case 2:
            w = '二';
            break;
        case 3:
            w = '三';
            break;
        case 4:
            w = '四';
            break;
        case 5:
            w = '五';
            break;
        case 6:
            w = '六';
            break;
    }
    return w;
};
```
### 方法2
``` js
var getChineseWeek = function(n) {
    return ['日', '一', '二', '三', '四', '五', '六'][n];
};
```
### 方法3
``` js
var getChineseWeek = function(n) {
    return '日一二三四五六'.charAt(n);
};
```
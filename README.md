> https://github.com/chenhuiYj/ec-do

# 1.字符串操作
## 1.1 去除字符串空格
``` javascript
//去除空格  type 1-所有空格  2-前后空格  3-前空格 4-后空格
//trim('  1235asd',1)
//result：1235asd
//这个方法有原生的方案代替，但是考虑到有时候开发PC站需要兼容IE8，所以就还是继续保留
trim: function (str, type) {
    switch (type) {
        case 1:
            return str.replace(/\s+/g, "");
        case 2:
            return str.replace(/(^\s*)|(\s*$)/g, "");
        case 3:
            return str.replace(/(^\s*)/g, "");
        case 4:
            return str.replace(/(\s*$)/g, "");
        default:
            return str;
    }
}
```
## 1.2 字母大小写转换
``` js
/*type
1:首字母大写
2：首页母小写
3：大小写转换
4：全部大写
5：全部小写
* */
//changeCase('asdasd',1)
//result：Asdasd
changeCase: function (str, type) {
    function ToggleCase(str) {
        var itemText = ""
        str.split("").forEach(
            function (item) {
                if (/^([a-z]+)/.test(item)) {
                    itemText += item.toUpperCase();
                } else if (/^([A-Z]+)/.test(item)) {
                    itemText += item.toLowerCase();
                } else {
                    itemText += item;
                }
            });
        return itemText;
    }
    switch (type) {
        case 1:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toUpperCase() + word.substring(1).toLowerCase();
            });
        case 2:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toLowerCase() + word.substring(1).toUpperCase();
            });
        case 3:
            return ToggleCase(str);
        case 4:
            return str.toUpperCase();
        case 5:
            return str.toLowerCase();
        default:
            return str;
    }
}
```
## 1.3 字符串循环复制
``` js
//repeatStr(str->字符串, count->次数)
//repeatStr('123',3)
//"result：123123123"
repeatStr: function (str, count) {
    var text = '';
    for (var i = 0; i < count; i++) {
        text += str;
    }
    return text;
}
```
## 1.4 字符串替换
``` js
//replaceAll('这里是上海，中国第三大城市，广东省省会，简称穗，','上海','广州')
//result："这里是广州，中国第三大城市，广东省省会，简称穗，"
replaceAll: function (str, AFindText, ARepText) {
    raRegExp = new RegExp(AFindText, "g");
    return str.replace(raRegExp, ARepText);
}
```

## 1.5 替换*
``` js
//字符替换*
//replaceStr(字符串,字符格式, 替换方式,替换的字符（默认*）)
//replaceStr('18819322663',[3,5,3],0)
//result：188*****663
//replaceStr('asdasdasdaa',[3,5,3],1)
//result：***asdas***
//replaceStr('1asd88465asdwqe3',[5],0)
//result：*****8465asdwqe3
//replaceStr('1asd88465asdwqe3',[5],1,'+')
//result："1asd88465as+++++"
replaceStr: function (str, regArr, type, ARepText) {
    var regtext = '',
        Reg = null,
        replaceText = ARepText || '*';
    //repeatStr是在上面定义过的（字符串循环复制），大家注意哦
    if (regArr.length === 3 && type === 0) {
        regtext = '(\\w{' + regArr[0] + '})\\w{' + regArr[1] + '}(\\w{' + regArr[2] + '})'
        Reg = new RegExp(regtext);
        var replaceCount = this.repeatStr(replaceText, regArr[1]);
        return str.replace(Reg, '$1' + replaceCount + '$2')
    }
    else if (regArr.length === 3 && type === 1) {
        regtext = '\\w{' + regArr[0] + '}(\\w{' + regArr[1] + '})\\w{' + regArr[2] + '}'
        Reg = new RegExp(regtext);
        var replaceCount1 = this.repeatStr(replaceText, regArr[0]);
        var replaceCount2 = this.repeatStr(replaceText, regArr[2]);
        return str.replace(Reg, replaceCount1 + '$1' + replaceCount2)
    }
    else if (regArr.length === 1 && type === 0) {
        regtext = '(^\\w{' + regArr[0] + '})'
        Reg = new RegExp(regtext);
        var replaceCount = this.repeatStr(replaceText, regArr[0]);
        return str.replace(Reg, replaceCount)
    }
    else if (regArr.length === 1 && type === 1) {
        regtext = '(\\w{' + regArr[0] + '}$)'
        Reg = new RegExp(regtext);
        var replaceCount = this.repeatStr(replaceText, regArr[0]);
        return str.replace(Reg, replaceCount)
    }
}
```

## 1.6 检测字符串
``` js
//检测字符串
//checkType('165226226326','phone')
//result：false
//大家可以根据需要扩展
checkType: function (str, type) {
    switch (type) {
        case 'email':
            return /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(str);
        case 'phone':
            return /^1[3|4|5|7|8][0-9]{9}$/.test(str);
        case 'tel':
            return /^(0\d{2,3}-\d{7,8})(-\d{1,4})?$/.test(str);
        case 'number':
            return /^[0-9]$/.test(str);
        case 'english':
            return /^[a-zA-Z]+$/.test(str);
        case 'text':
            return /^\w+$/.test(str);
        case 'chinese':
            return /^[\u4E00-\u9FA5]+$/.test(str);
        case 'lower':
            return /^[a-z]+$/.test(str);
        case 'upper':
            return /^[A-Z]+$/.test(str);
        default:
            return true;
    }
}
```

## 1.7 检测密码强度
``` js
//checkPwd('12asdASAD')
//result：3(强度等级为3)
checkPwd: function (str) {
    var nowLv = 0;
    if (str.length < 6) {
        return nowLv
    }
    if (/[0-9]/.test(str)) {
        nowLv++
    }
    if (/[a-z]/.test(str)) {
        nowLv++
    }
    if (/[A-Z]/.test(str)) {
        nowLv++
    }
    if (/[\.|-|_]/.test(str)) {
        nowLv++
    }
    return nowLv;
}
```

## 1.8 随机码（toString详解）
``` js
//count取值范围0-36
//randomWord(10)
//result："2584316588472575"
//randomWord(14)
//result："9b405070dd00122640c192caab84537"
//randomWord(36)
//result："83vhdx10rmjkyb9"
randomWord: function (count) {
    return Math.random().toString(count).substring(2);
}
```
### 生成6位数字验证码
``` js
// 方法一
('000000' + Math.floor(Math.random() * 999999)).slice(-6);
// 方法二
Math.random().toString().slice(-6);
// 方法三
Math.random().toFixed(6).slice(-6);
// 方法四
'' + Math.floor(Math.random() * 999999);
```

## 1.9 查找字符串
``` js
可能标题会有点误导，下面我就简单说明一个需求，在字符串'sad44654blog5a1sd67as9dablog4s5d16zxc4sdweasjkblogwqepaskdkblogahseiuadbhjcibloguyeajzxkcabloguyiwezxc967'中找出'blog'的出现次数。代码如下
//var strTest='sad44654blog5a1sd67as9dablog4s5d16zxc4sdweasjkblogwqepaskdkblogahseiuadbhjcibloguyeajzxkcabloguyiwezxc967'
//countStr(strTest,'blog')
//result：6
countStr: function (str, strSplit) {
    return str.split(strSplit).length - 1
}
```

## 1.10 过滤字符串
``` js
//过滤字符串(html标签，表情，特殊字符)
//字符串，替换内容（special-特殊字符,html-html标签,emjoy-emjoy表情,word-小写字母，WORD-大写字母，number-数字,chinese-中文），要替换成什么，默认'',保留哪些特殊字符
//如果需要过滤多种字符，type参数使用,分割，如下栗子
//过滤字符串的html标签，大写字母，中文，特殊字符，全部替换成*,但是特殊字符'%'，'?'，除了这两个，其他特殊字符全部清除
//var str='asd    654a大蠢sasdasdASDQWEXZC6d5#%^*^&*^%^&*$\\"\'#@!()*/-())_\'":"{}?<div></div><img src=""/>啊实打实大蠢猪自行车这些课程';
// filterStr(str,'html,WORD,chinese,special','*','%?')
//result："asd    654a**sasdasd*********6d5#%^*^&*^%^&*$\"'#@!()*/-())_'":"{}?*****************"
filterStr: function (str, type, restr, spstr) {
    var typeArr = type.split(','), _str = str;
    for (var i = 0, len = typeArr.length; i < len; i++) {
        //是否是过滤特殊符号
        if (typeArr[i] === 'special') {
            var pattern, regText = '$()[]{}?\|^*+./\"\'+';
            //是否有哪些特殊符号需要保留
            if (spstr) {
                var _spstr = spstr.split(""), _regText = "[^0-9A-Za-z\\s";
                for (var j = 0, len1 = _spstr.length; j < len1; j++) {
                    if (regText.indexOf(_spstr[j]) === -1) {
                        _regText += _spstr[j];
                    }
                    else {
                        _regText += '\\' + _spstr[j];
                    }
                }
                _regText += ']'
                pattern = new RegExp(_regText, 'g');
            }
            else {
                pattern = new RegExp("[^0-9A-Za-z\\s]", 'g')
            }
        }
        var _restr = restr || '';
        switch (typeArr[i]) {
            case 'special':
                _str = _str.replace(pattern, _restr);
                break;
            case 'html':
                _str = _str.replace(/<\/?[^>]*>/g, _restr);
                break;
            case 'emjoy':
                _str = _str.replace(/[^\u4e00-\u9fa5|\u0000-\u00ff|\u3002|\uFF1F|\uFF01|\uff0c|\u3001|\uff1b|\uff1a|\u3008-\u300f|\u2018|\u2019|\u201c|\u201d|\uff08|\uff09|\u2014|\u2026|\u2013|\uff0e]/g, _restr);
                break;
            case 'word':
                _str = _str.replace(/[a-z]/g, _restr);
                break;
            case 'WORD':
                _str = _str.replace(/[A-Z]/g, _restr);
                break;
            case 'number':
                _str = _str.replace(/[0-9]/g, _restr);
                break;
            case 'chinese':
                _str = _str.replace(/[\u4E00-\u9FA5]/g, _restr);
                break;
        }
    }
    return _str;
}
```

## 1.11 格式化处理字符串
``` js
//formatText('1234asda567asd890')
//result："12,34a,sda,567,asd,890"
//formatText('1234asda567asd890',4,' ')
//result："1 234a sda5 67as d890"
//formatText('1234asda567asd890',4,'-')
//result："1-234a-sda5-67as-d890"
formatText: function (str, size, delimiter) {
    var _size = size || 3, _delimiter = delimiter || ',';
    var regText = '\\B(?=(\\w{' + _size + '})+(?!\\w))';
    var reg = new RegExp(regText, 'g');
    return str.replace(reg, _delimiter);
}
```

## 1.12 找出最长单词
``` js
//longestWord('Find the Longest word in a String')
//result：7
//longestWord('Find|the|Longest|word|in|a|String','|')
//result：7
longestWord: function (str, splitType) {
    var _splitType = splitType || /\s+/g,
        _max = 0,_item='';
    var strArr = str.split(_splitType);
    strArr.forEach(function (item) {
        if (_max < item.length) {
            _max = item.length
            _item=item;
        }
    })
    return {el:_item,max:_max};
}
```

## 1.13 句中单词首字母大写 
``` js
//这个我也一直在纠结，英文标题，即使是首字母大写，也未必每一个单词的首字母都是大写的，但是又不知道哪些应该大写，哪些不应该大写
//titleCaseUp('this is a title')
//"This Is A Title"
titleCaseUp: function (str, splitType) {
    var _splitType = splitType || /\s+/g;
    var strArr = str.split(_splitType),
        result = "", _this = this
    strArr.forEach(function (item) {
        result += _this.changeCase(item, 1) + ' ';
    })
    return this.trim(result, 4)
}  
```
- - -

# 2.数组操作
## 2.1 数组去重
### 2.1.1 方法一
``` js
removeRepeatArray: function (arr) {
    return arr.filter(function (item, index, self) {
        return self.indexOf(item) === index;
    });
}
```

### 2.1.2 方法二
``` js
var arr1 = [1,2,3,2,1,2];
function repeat1(arr){
    for(var i = 0, arr2 = []; i < arr.length; i++){
        if(arr2.indexOf(arr[i]) === -1){
            arr2.push(arr[i]);
        }
    }
    return arr2;
}
```

### 2.1.3 方法三(hash)
``` js
function repeat2(arr) {
    //遍历arr中每个元素，同时声明hash
    for(var i = 0, hash = {}; i < arr.length; i++) {
        //hash中是否包含当前元素值的建
        //如果不包含,就往hash添加一个新元素，以当前元素值为key，value默认为1
        if(hash[arr[i]] === undefined) {
            hash[arr[i]] = 1;
        }
    }
    //将hash转为索引
    var i = 0;
    var arr2 = [];
    for(arr2[i++] in hash);
    return arr2;
}
```

### 2.1.4 方法四
``` js
function repeat3(arr) {
    return arr.sort()
             .join(",,")
             .replace(
              /(^|,,)([^,]+)(,,\2)*/g,
              "$1$2")
             .split(",,");
  }
```

## 2.2 数组顺序打乱
``` js
upsetArr: function(arr) {
    return arr.sort(function() {
        return Math.random() - 0.5
    });
}
```

## 2.3 数组最大值最小值
``` js
//数组最大值
maxArr: function (arr) {
    return Math.max.apply(null, arr);
}
//数组最小值
minArr: function (arr) {
    return Math.min.apply(null, arr);
}
```

## 2.4 数组求和，平均值
``` js
//这一块的封装，主要是针对数字类型的数组
//求和
sumArr: function (arr) {
    return arr.reduce(function (pre, cur) {
        return pre + cur
    })
}
//数组平均值,小数点可能会有很多位，这里不做处理，处理了使用就不灵活！
covArr: function (arr) {
    return this.sumArr(arr) / arr.length;
}
```

## 2.5 从数组中随机获取元素
``` js
randomOne: function (arr) {
    return arr[Math.floor(Math.random() * arr.length)];
}
```

## 2.6 返回数组（字符串）一个元素出现的次数
``` js
//getEleCount('asd56+asdasdwqe','a')
//result：3
//getEleCount([1,2,3,4,5,66,77,22,55,22],22)
//result：2
getEleCount: function (obj, ele) {
    var num = 0;
    for (var i = 0, len = obj.length; i < len; i++) {
        if (ele === obj[i]) {
            num++;
        }
    }
    return num;
}
```

## 2.7 返回数组（字符串）出现次数最多的元素和出现次数
``` js
//arr, rank->长度，默认为数组长度，ranktype，排序方式，默认降序
//返回值：el->元素，count->次数
//getCount([1,2,3,1,2,5,2,4,1,2,6,2,1,3,2])
//默认情况，返回所有元素出现的次数
//result：[{"el":"2","count":6},{"el":"1","count":4},{"el":"3","count":2},{"el":"4","count":1},{"el":"5","count":1},{"el":"6","count":1}]
//getCount([1,2,3,1,2,5,2,4,1,2,6,2,1,3,2],3)
//传参（rank=3），只返回出现次数排序前三的
//result：[{"el":"2","count":6},{"el":"1","count":4},{"el":"3","count":2}]
//getCount([1,2,3,1,2,5,2,4,1,2,6,2,1,3,2],null,1)
//传参（ranktype=1,rank=null），升序返回所有元素出现次数
//result：[{"el":"6","count":1},{"el":"5","count":1},{"el":"4","count":1},{"el":"3","count":2},{"el":"1","count":4},{"el":"2","count":6}]
//getCount([1,2,3,1,2,5,2,4,1,2,6,2,1,3,2],3,1)
//传参（rank=3，ranktype=1），只返回出现次数排序（升序）前三的
//result：[{"el":"6","count":1},{"el":"5","count":1},{"el":"4","count":1}]
getCount: function (arr, rank, ranktype) {
    var obj = {},
        k, arr1 = []
    //记录每一元素出现的次数
    for (var i = 0, len = arr.length; i < len; i++) {
        k = arr[i];
        if (obj[k]) {
            obj[k]++;
        } else {
            obj[k] = 1;
        }
    }
    //保存结果{el-'元素'，count-出现次数}
    for (var o in obj) {
        arr1.push({el: o, count: obj[o]});
    }
    //排序（降序）
    arr1.sort(function (n1, n2) {
        return n2.count - n1.count
    });
    //如果ranktype为1，则为升序，反转数组
    if (ranktype === 1) {
        arr1 = arr1.reverse();
    }
    var rank1 = rank || arr1.length;
    return arr1.slice(0, rank1);
}
```

## 2.8 返回n1-n2下标的数组
``` js
//getArrayNum([0,1,2,3,4,5,6,7,8,9],5,9)
//result：[5, 6, 7, 8, 9]
//getArrayNum([0,1,2,3,4,5,6,7,8,9],2) //不传第二个参数,默认返回从n1到数组结束的元素
//result：[2, 3, 4, 5, 6, 7, 8, 9]
getArrayNum: function (arr, n1, n2) {
    return arr.slice(n1, n2);
}
```

## 2.9 筛选数组
``` js
//删除值为'val'的数组元素
//removeArrayForValue(['test','test1','test2','test','aaa'],'test',1)
//result：["aaa"]   带有'test'的都删除
//removeArrayForValue(['test','test1','test2','test','aaa'],'test')
//result：["test1", "test2", "aaa"]  //数组元素的值全等于'test'才被删除
removeArrayForValue: function (arr, val, type) {
    return arr.filter(function (item) {
        return type ? item.indexOf(val) === -1 : item !== val
    })
}
```

## 2.10 获取对象数组某些项
``` js
//var arr=[{a:1,b:2,c:9},{a:2,b:3,c:5},{a:5,b:9},{a:4,b:2,c:5},{a:4,b:5,c:7}]
//getOptionArray(arr,'a,c')
//result：[{a:1,c:9},{a:2,c:5},{a:5,c:underfind},{a:4,c:5},{a:4,c:7}]
//getOptionArray(arr,'b')
//result：[2, 3, 9, 2, 5]
getOptionArray: function (arr, keys) {
    var newArr = []
    if (!keys) {
        return arr
    }
    var _keys = keys.split(','), newArrOne = {};
    //是否只是需要获取某一项的值
    if (_keys.length === 1) {
        for (var i = 0, len = arr.length; i < len; i++) {
            newArr.push(arr[i][keys])
        }
        return newArr;
    }
    for (var i = 0, len = arr.length; i < len; i++) {
        newArrOne = {};
        for (var j = 0, len1 = _keys.length; j < len1; j++) {
            newArrOne[_keys[j]] = arr[i][_keys[j]]
        }
        newArr.push(newArrOne);
    }
    return newArr
}
```

## 2.11 排除对象数组某些项 
``` js
//var arr=[{a:1,b:2,c:9},{a:2,b:3,c:5},{a:5,b:9},{a:4,b:2,c:5},{a:4,b:5,c:7}]
//filterOptionArray(arr,'a')
//result：[{b:2,c:9},{b:3,c:5},{b:9},{b:2,c:5},{b:5,c:7}]
//filterOptionArray(arr,'a,c')
//result：[{b:2},{b:3},{b:9},{b:2},{b:5}]
filterOptionArray: function (arr, keys) {
    var newArr = []
    var _keys = keys.split(','), newArrOne = {};
    for (var i = 0, len = arr.length; i < len; i++) {
        newArrOne = {};
        for (var key in arr[i]) {
            //如果key不存在排除keys里面,添加数据
            if (_keys.indexOf(key) === -1) {
                newArrOne[key] = arr[i][key];
            }
        }
        newArr.push(newArrOne);
    }
    return newArr
}
```

## 2.12 对象数组排序
``` js
//var arr=[{a:1,b:2,c:9},{a:2,b:3,c:5},{a:5,b:9},{a:4,b:2,c:5},{a:4,b:5,c:7}]
//arraySort(arr,'a,b')a是第一排序条件，b是第二排序条件
//result：[{"a":1,"b":2,"c":9},{"a":2,"b":3,"c":5},{"a":4,"b":2,"c":5},{"a":4,"b":5,"c":7},{"a":5,"b":9}]
arraySort: function (arr, sortText) {
    if (!sortText) {
        return arr
    }
    var _sortText = sortText.split(',').reverse(), _arr = arr.slice(0);
    for (var i = 0, len = _sortText.length; i < len; i++) {
        _arr.sort(function (n1, n2) {
            return n1[_sortText[i]] - n2[_sortText[i]]
        })
    }
    return _arr;
}
```

## 2.13 数组扁平化
### 方法一
``` js
//steamroller([1,2,[4,5,[1,23]]])
//[1, 2, 4, 5, 1, 23]
steamroller: function (arr) {
    var newArr = [],_this=this;
    for (var i = 0; i < arr.length; i++) {
        if (Array.isArray(arr[i])) {
            // 如果是数组，调用(递归)steamroller 将其扁平化
            // 然后再 push 到 newArr 中
            newArr.push.apply(newArr, _this.steamroller(arr[i]));
        } else {
            // 不是数组直接 push 到 newArr 中
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
```
### 方法二
``` js
// flatten([1, [2, 3], ['4', 5, ['6',7,[8]]], [9], 10])
// result: [1, 2, 3, "4", 5, "6", 7, 8, 9, 10]
flatten: function(arr) {
    return JSON.parse(`[${JSON.stringify(foo).replace(/\[|]/g, '')}]`);
}
```
### 方法三
``` js
const flatten = (arr) => arr.reduce((a, b) => a.concat(Array.isArray(b) ? flatten(b) : b), []);
```
### 方法四
``` js
function flatten(arr) {
    return Array.isArray(arr) ? [].concat(...arr.map(flatten)) : arr;
}
```
> 注：更多方法请参考《How to flatten nested array in JavaScript?》

- - -
## cookie
``` js
//cookie
//设置cookie
setCookie: function (name, value, iDay) {
    var oDate = new Date();
    oDate.setDate(oDate.getDate() + iDay);
    document.cookie = name + '=' + value + ';expires=' + oDate;
},
//获取cookie
getCookie: function (name) {
    var arr = document.cookie.split('; ');
    for (var i = 0; i < arr.length; i++) {
        var arr2 = arr[i].split('=');
        if (arr2[0] == name) {
            return arr2[1];
        }
    }
    return '';
}
//删除cookie
removeCookie: function (name) {
    this.setCookie(name, 1, -1);
}
```

## 清除对象中值为空的属性
``` js
//filterParams({a:"",b:null,c:"010",d:123})
//Object {c: "010", d: 123}
filterParams: function (obj) {
    var _newPar = {};
    for (var key in obj) {
        if ((obj[key] === 0 ||obj[key] === false|| obj[key]) && obj[key].toString().replace(/(^\s*)|(\s*$)/g, '') !== '') {
            _newPar[key] = obj[key];
        }
    }
    return _newPar;
}
```

## 现金额大写转换函数
``` js
//upDigit(168752632)
//result："人民币壹亿陆仟捌佰柒拾伍万贰仟陆佰叁拾贰元整"
//upDigit(1682)
//result："人民币壹仟陆佰捌拾贰元整"
//upDigit(-1693)
//result："欠人民币壹仟陆佰玖拾叁元整"
upDigit: function (n) {
    var fraction = ['角', '分', '厘'];
    var digit = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
    var unit = [
        ['元', '万', '亿'],
        ['', '拾', '佰', '仟']
    ];
    var head = n < 0 ? '欠人民币' : '人民币';
    n = Math.abs(n);
    var s = '';
    for (var i = 0; i < fraction.length; i++) {
        s += (digit[Math.floor(n * 10 * Math.pow(10, i)) % 10] + fraction[i]).replace(/零./, '');
    }
    s = s || '整';
    n = Math.floor(n);
    for (var i = 0; i < unit[0].length && n > 0; i++) {
        var p = '';
        for (var j = 0; j < unit[1].length && n > 0; j++) {
            p = digit[n % 10] + unit[1][j] + p;
            n = Math.floor(n / 10);
        }
        s = p.replace(/(零.)*零$/, '').replace(/^$/, '零') + unit[0][i] + s;
        //s = p + unit[0][i] + s;
    }
    return head + s.replace(/(零.)*零元/, '元').replace(/(零.)+/g, '零').replace(/^整$/, '零元整');
} 
```

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
## 获取url参数中某个key的value
``` js
// http://test.com?oldMobileNo=15067057672&newMobileNo=15267996659&idNo=513029198807100037
// getQueryString('newMobileNo')
// result: 15267996659
getQueryString: function (key) {
    var reg = new RegExp("(^|&)" + key + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) {
        return unescape(r[2]);
    }
    return null;
}
```
## url查询参数转json格式
### 方法一
``` js
//getUrlPrmt('test.com/write?draftId=122000011938')
//result：Object{draftId: "122000011938"}
getUrlPrmt: function (url) {
    url = url ? url : window.location.href;
    var _pa = url.substring(url.indexOf('?') + 1),
        _arrS = _pa.split('&'),
        _rs = {};
    for (var i = 0, _len = _arrS.length; i < _len; i++) {
        var pos = _arrS[i].indexOf('=');
        if (pos == -1) {
            continue;
        }
        var name = _arrS[i].substring(0, pos),
            value = window.decodeURIComponent(_arrS[i].substring(pos + 1));
        _rs[name] = value;
    }
    return _rs;
}
```
### 方法二(ES5)
``` js
//query('test.com/write?draftId=122000011938')
//result：Object{draftId: "122000011938"}
query：function (search) {
    if (search === void 0) search = '';
    return (function(querystring) {
        if (querystring === void 0) querystring = '';
        return (function(q) {
            return (querystring.split('&').forEach(function(item) {
                return (function(kv) {
                    return kv[0] && (q[kv[0]] = kv[1]);
                })(item.split('='));
            }), q);
        })({});
    })(search.split('?')[1]);
}
```
### 方法三(ES6)
``` js
//query('test.com/write?draftId=122000011938')
//result：Object{draftId: "122000011938"}
const query = (search = '') => ((querystring = '') => (q => (querystring.split('&').forEach(item => (kv => kv[0] && (q[kv[0]] = kv[1]))(item.split('='))), q))({}))(search.split('?')[1]);
```

## ajax
``` js
/* 封装ajax函数
 * @param {string}obj.type http连接的方式，包括POST和GET两种方式
 * @param {string}obj.url 发送请求的url
 * @param {boolean}obj.async 是否为异步请求，true为异步的，false为同步的
 * @param {object}obj.data 发送的参数，格式为对象类型
 * @param {function}obj.success ajax发送并接收成功调用的回调函数
 * @param {function}obj.error ajax发送失败或者接收失败调用的回调函数
 */
//  ajax({
//      type:'get',
//      url:'xxx',
//      data:{
//          id:'111'
//      },
//      success:function(res){
//          console.log(res)
//      }
//  })
ajax: function (obj) {
    obj = obj || {};
    obj.type = obj.type ? obj.type.toUpperCase() : 'POST';
    obj.url = obj.url || '';
    obj.async = obj.async || true;
    obj.data = obj.data || null;
    obj.success = obj.success || function () {};
    obj.error = obj.error || function () {};
    var xmlHttp = null;
    if (XMLHttpRequest) {
        xmlHttp = new XMLHttpRequest();
    } else {
        xmlHttp = new ActiveXObject('Microsoft.XMLHTTP');
    }
    var params = [];
    for (var key in obj.data) {
        params.push(key + '=' + obj.data[key]);
    }
    var postData = params.join('&');
    if (obj.type.toUpperCase() === 'POST') {
        xmlHttp.open(obj.type, obj.url, obj.async);
        xmlHttp.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded;charset=utf-8');
        xmlHttp.send(postData);
    } else if (obj.type.toUpperCase() === 'GET') {
        xmlHttp.open(obj.type, obj.url + '?' + postData, obj.async);
        xmlHttp.send(null);
    }
    xmlHttp.onreadystatechange = function () {
        // if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
        //     obj.success(xmlHttp.responseText);
        // } else {
        //     obj.error(xmlHttp.responseText);
        // }

        if (xmlHttp.readyState === 4) {
            if ((xmlHttp.status >= 200 && xmlHttp.status < 300) || xmlHttp.status === 304) {
                obj.success(xmlHttp.responseText);
            } else {
                obj.error(xmlHttp.responseText);
            }
        }
    };
}
```

## 图片懒加载
``` js
//图片没加载出来时用一张图片代替
aftLoadImg: function (obj, url, errorUrl,cb) {
    var oImg = new Image(), _this = this;
    oImg.src = url;
    oImg.onload = function () {
        obj.src = oImg.src;
        if (cb && _this.istype(cb, 'function')) {
            cb(obj);
        }
    }
    oImg.onerror=function () {
        obj.src=errorUrl;
        if (cb && _this.istype(cb, 'function')) {
            cb(obj);
        }
    }
},
//图片滚动懒加载
//@className {string} 要遍历图片的类名
//@num {number} 距离多少的时候开始加载 默认 0
//比如，一张图片距离文档顶部3000，num参数设置200，那么在页面滚动到2800的时候，图片加载。不传num参数就滚动，num默认是0，页面滚动到3000就加载
//html代码
//<p><img data-src="https://user-gold-cdn.xitu.io/2017/12/7/160319f12631736f" class="load-img" width='528' height='304' /></p>
//<p><img data-src="https://user-gold-cdn.xitu.io/2017/12/7/160319f12631736f" class="load-img" width='528' height='304' /></p>
//<p><img data-src="https://user-gold-cdn.xitu.io/2017/12/7/160319f12631736f" class="load-img" width='528' height='304' /></p>....
//data-src储存src的数据，到需要加载的时候把data-src的值赋值给src属性，图片就会加载。
//详细可以查看testLoadImg.html
//window.onload = function() {
//    loadImg('load-img',100);
//    window.onscroll = function() {
//        ecDo.loadImg('load-img',100);
//        }
//}
loadImg: function (className, num, errorUrl) {
    var _className = className || 'ec-load-img', _num = num || 0, _this = this,_errorUrl=errorUrl||null;
    var oImgLoad = document.getElementsByClassName(_className);
    for (var i = 0, len = oImgLoad.length; i < len; i++) {
        //如果图片已经滚动到指定的高度
        if (document.documentElement.clientHeight + document.documentElement.scrollTop > oImgLoad[i].offsetTop - _num && !oImgLoad[i].isLoad) {
            //记录图片是否已经加载
            oImgLoad[i].isLoad = true;
            //设置过渡，当图片下来的时候有一个图片透明度变化
            oImgLoad[i].style.cssText = "transition: ''; opacity: 0;"
            if (oImgLoad[i].dataset) {
                this.aftLoadImg(oImgLoad[i], oImgLoad[i].dataset.src, _errorUrl, function (o) {
                    //添加定时器，确保图片已经加载完了，再把图片指定的的class，清掉，避免重复编辑
                    setTimeout(function () {
                        if (o.isLoad) {
                            _this.removeClass(o, _className);
                            o.style.cssText = "";
                        }
                    }, 1000)
                });
            } else {
                this.aftLoadImg(oImgLoad[i], oImgLoad[i].getAttribute("data-src"), _errorUrl, function (o) {
                    //添加定时器，确保图片已经加载完了，再把图片指定的的class，清掉，避免重复编辑
                    setTimeout(function () {
                        if (o.isLoad) {
                            _this.removeClass(o, _className);
                            o.style.cssText = "";
                        }
                    }, 1000)
                });
            }
            (function (i) {
                setTimeout(function () {
                    oImgLoad[i].style.cssText = "transition:all 1s; opacity: 1;";
                }, 16)
            })(i);
        }
    }
}
```

## 关键词加标签 
``` js
//这两个函数多用于搜索的时候，关键词高亮
//创建正则字符
//createKeyExp([前端，过来])
//result:(前端|过来)/g
createKeyExp: function (strArr) {
    var str = "";
    for (var i = 0; i < strArr.length; i++) {
        if (i != strArr.length - 1) {
            str = str + strArr[i] + "|";
        } else {
            str = str + strArr[i];
        }
    }
    return "(" + str + ")";
},
//关键字加标签（多个关键词用空格隔开）
//findKey('守侯我oaks接到了来自下次你离开快乐吉祥留在开城侯','守侯 开','i')
//"<i>守侯</i>我oaks接到了来自下次你离<i>开</i>快乐吉祥留在<i>开</i>城侯"
findKey: function (str, key, el) {
    var arr = null,
        regStr = null,
        content = null,
        Reg = null,
        _el = el || 'span';
    arr = key.split(/\s+/);
    //alert(regStr); //    如：(前端|过来)
    regStr = this.createKeyExp(arr);
    content = str;
    //alert(Reg);//        /如：(前端|过来)/g
    Reg = new RegExp(regStr, "g");
    //过滤html标签 替换标签，往关键字前后加上标签
    content = content.replace(/<\/?[^>]*>/g, '')
    return content.replace(Reg, "<" + _el + ">$1</" + _el + ">");
}
```

## 数据类型判断
``` js
//istype([],'array')
//true
//istype([])
//'[object Array]'
istype: function (o, type) {
    if (type) {
        var _type = type.toLowerCase();
    }
    switch (_type) {
        case 'string':
            return Object.prototype.toString.call(o) === '[object String]';
        case 'number':
            return Object.prototype.toString.call(o) === '[object Number]';
        case 'boolean':
            return Object.prototype.toString.call(o) === '[object Boolean]';
        case 'undefined':
            return Object.prototype.toString.call(o) === '[object Undefined]';
        case 'null':
            return Object.prototype.toString.call(o) === '[object Null]';
        case 'function':
            return Object.prototype.toString.call(o) === '[object Function]';
        case 'array':
            return Object.prototype.toString.call(o) === '[object Array]';
        case 'object':
            return Object.prototype.toString.call(o) === '[object Object]';
        case 'nan':
            return isNaN(o);
        case 'elements':
            return Object.prototype.toString.call(o).indexOf('HTML') !== -1
        default:
            return Object.prototype.toString.call(o)
    }
}
```

## 手机类型判断
``` js
browserInfo: function (type) {
    switch (type) {
        case 'android':
            return navigator.userAgent.toLowerCase().indexOf('android') !== -1
        case 'iphone':
            return navigator.userAgent.toLowerCase().indexOf('iphone') !== -1
        case 'ipad':
            return navigator.userAgent.toLowerCase().indexOf('ipad') !== -1
        case 'weixin':
            return navigator.userAgent.toLowerCase().indexOf('micromessenger') !== -1
        default:
            return navigator.userAgent.toLowerCase()
    }
}
```

## 函数节流
``` js
//多用于鼠标滚动，移动，窗口大小改变等高频率触发事件
// var count=0;
// function fn1(){
//     count++;
//     console.log(count)
// }
// //100ms内连续触发的调用，后一个调用会把前一个调用的等待处理掉，但每隔200ms至少执行一次
// document.onmousemove=delayFn(fn1,100,200)
delayFn: function (fn, delay, mustDelay) {
    var timer = null;
    var t_start;
    return function () {
        var context = this, args = arguments, t_cur = +new Date();
        //先清理上一次的调用触发（上一次调用触发事件不执行）
        clearTimeout(timer);
        //如果不存触发时间，那么当前的时间就是触发时间
        if (!t_start) {
            t_start = t_cur;
        }
        //如果当前时间-触发时间大于最大的间隔时间（mustDelay），触发一次函数运行函数
        if (t_cur - t_start >= mustDelay) {
            fn.apply(context, args);
            t_start = t_cur;
        }
        //否则延迟执行
        else {
            timer = setTimeout(function () {
                fn.apply(context, args);
            }, delay);
        }
    };
}
```
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

## 驼峰命名转下划线
``` js
'componentMapModelRegistry'.match(/^[a-z][a-z0-9]+|[A-Z][a-z0-9]*/g).join('_').toLowerCase()
```

## 统计文字个数
``` js
function wordCount(data) {
    var pattern = /[a-zA-Z0-9_\u0392-\u03c9]+|[\u4E00-\u9FFF\u3400-\u4dbf\uf900-\ufaff\u3040-\u309f\uac00-\ud7af]+/g;
    var m = data.match(pattern);
    var count = 0;
    if ( m === null) return count;
    for (var i = 0; i < m.length; i++) {
        if (m[i].charCodeAt(0) >= 0x4E00) {
            count += m[i].length;
        } else {
            count += 1;
        }
    }
    return count;
}
```

## 特殊字符转义
``` js
// htmlspecialchars('&jfkds<>')
// result: &amp;jfkds&lt;&gt;
function htmlspecialchars (str) {
    var str = str.toString().replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, '&quot;');
    return str;
}
```

## 动态插入js
``` js
function injectScript(src) {
    var s, t;
    s = document.createElement('script');
    s.type = 'text/javascript';
    s.async = true;
    s.src = src;
    t = document.getElementsByTagName('script')[0];

    // 最前面
    t.parentNode.insertBefore(s, t);
    // 最后面
    // t.parentNode.appendChild(s);
}
```
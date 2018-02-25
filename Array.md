> https://github.com/chenhuiYj/ec-do

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

### 2.1.5 方法五(最短代码)
``` js
[...new Set([1, "1", 2, 1, 1, 3])]; // [1, "1", 2, 3]
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
> 注：更多方法请参考[《How to flatten nested array in JavaScript?》](https://stackoverflow.com/questions/27266550/how-to-flatten-nested-array-in-javascript)

## 2.14 将argruments对象转换成数组
``` js
// ES5
var argArray = [].prototype.slice.call(arguments);
var argArray = Array.prototype.slice.call(arguments);

// ES6
var argArray = Array.from(arguments)
var argArray = [...arguments];
```

## 创建指定长度非空数组
在JavaScript中可以通过`new Array(3)`的形式创建一个长度为3的空数组。在老的Chrome中其值为`[undefined x 3]`，在最新的Chrome中为`[empty x 3]`，即空单元数组。在老Chrome中，相当于显示使用`[undefined, undefined, undefined]`的方式创建长度为3的数组。

但是，两者在调用`map()`方法的结果是明显不同的
``` js
var a = new Array(3);
var b = [undefined, undefined, undefined];

a.map((v, i) => i); // [empty × 3]
b.map((v, i) => i); // [0, 1, 2]
```
多数情况我们期望创建的是包含`undefined`值的指定长度的空数组，可以通过下面这种方法来达到目的：
``` js
var a = Array.apply(null, { length: 3 });

a; // [undefined, undefined, undefined]
a.map((v, i) => i); // [0, 1, 2]
```
总之，尽量不要创建和使用空单元数组。

## 得到一个数组的随机副本
### 方法1(underscore的实现方法：原理就是获得一个随机索引，根据随机索引取出数组中随机的一个，与最新的一个值进行交换。)
``` js
_.shuffle = function(obj) {
    var rand;
    var index = 0;
    var shuffled = [];
    each(obj, function(value) {
        rand = _.random(index++);
        shuffled[index - 1] = shuffled[rand];
        shuffled[rand] = value;
    });
    return shuffled;
};
```
### 方法2
``` js
var shuffle = function(array, length) {
    var length = length || array.length,
        newArr = [],
        randomIndex,
        removed;
    array.forEach(function(elem, index) {
        randomIndex = Math.floor(Math.random() * index);
        removed = newArr.splice(randomIndex, 1, elem);
        removed.length && newArr.push(removed[0]);
    });
    return newArr.slice(0, length);
};
```
### 方法3
``` js
// var arr = [3, 6, 8, 1, 0, 10, 67, 23];
// shuffle(arr); // [67, 0, 10, 1, 8, 3, 6, 23]
var shuffle = function(array, length) {
    var length = length || array.length;
    var array = array.slice();
    array.sort(function() {
        return Math.random() - 0.5;    
    });
    return array.slice(0, length);
};
```

## 将一个数组插入另一个数组的指定位置
看到这个标题，最先想到的应该是使用数组的`splice`方法，但是`splice`方法接受的参数是需要一个一个写出来的
``` js
arrayObject.splice(index, howmany, item1,.....,itemX)
```
到这里，可以想到使用`apply`来改变参数的传递方式，最终代码如下：
``` js
var a = [1,2,3,7,8,9];
var b = [4,5,6];
var insertIndex = 3;
Array.prototype.splice.apply(a, Array.prototype.concat(insertIndex, 0, b));
// Array.prototype.splice.apply(a, [insertIndex, 0, b]);
// Array.prototype.splice.call(a, insertIndex, 0, b);
console.log(a); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
# 1.字符串操作
## 1.1 去除字符串空格
``` javascript
//去除空格  type 1-所有空格  2-前后空格  3-前空格 4-后空格
//ecDo.trim('  1235asd',1)
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
//ecDo.changeCase('asdasd',1)
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
//ecDo.repeatStr('123',3)
//"result：123123123"
repeatStr: function (str, count) {
    var text = '';
    for (var i = 0; i < count; i++) {
        text += str;
    }
    return text;
}
```

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
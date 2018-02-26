[test.html](test.html?oldMobileNo=15067057672&newMobileNo=15267996659&idNo=513029199707100037)
[Array](Array.md)
[Date](Date.md)
[Dom](Dom.md)
[Math](Math.md)
[Number](Number.md)
[Operator](Operator.md)
[String](String.md)
[toString](toString.md)

## `cookie`
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

## 设置url参数
``` js
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

## 格式化表单数据
``` js
function formatParam(obj) {
  var query = '', name, value, fullSubName, subName, subValue, innerObj, i;

  for(name in obj) {
    value = obj[name];

    if(value instanceof Array) {
      for(i=0; i<value.length; ++i) {
        subValue = value[i];
        fullSubName = name + '[' + i + ']';
        innerObj = {};
        innerObj[fullSubName] = subValue;
        query += formatParam(innerObj) + '&';
      }
    }
    else if(value instanceof Object) {
      for(subName in value) {
        subValue = value[subName];
        fullSubName = name + '[' + subName + ']';
        innerObj = {};
        innerObj[fullSubName] = subValue;
        query += formatParam(innerObj) + '&';
      }
    }
    else if(value !== undefined && value !== null)
      query += encodeURIComponent(name) + '=' + encodeURIComponent(value) + '&';
  }
  return query.length ? query.substr(0, query.length - 1) : query;
}

var param = {
  name: 'jenemy',
  likes: [0, 1, 3],
  memberCard: [
    { title: '1', id: 1 },
    { title: '2', id: 2 }
  ]
}

formatParam(param); // "name=jenemy&likes%5B0%5D=0&likes%5B1%5D=1&likes%5B2%5D=3&memberCard%5B0%5D%5Btitle%5D=1&memberCard%5B0%5D%5Bid%5D=1&memberCard%5B1%5D%5Btitle%5D=2&memberCard%5B1%5D%5Bid%5D=2"
```

## 在浏览器中根据url下载文件
``` js
function download(url) {
  var isChrome = navigator.userAgent.toLowerCase().indexOf('chrome') > -1;
  var isSafari = navigator.userAgent.toLowerCase().indexOf('safari') > -1;

  if (isChrome || isSafari) {
    var link = document.createElement('a');
    link.href = url;

    if (link.download !== undefined) {
      var fileName = url.substring(url.lastIndexOf('/') + 1, url.length);
      link.download = fileName;
    }

    if (document.createEvent) {
      var e = document.createEvent('MouseEvents');
      e.initEvent('click', true, true);
      link.dispatchEvent(e);
      return true;
    }
  }

  if (url.indexOf('?') === -1) {
    url += '?download';
  }

  window.open(url, '_self');
  return true;
}
```

## `ajax`
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
            // return Object.prototype.toString.call(o).slice(8, -1) === 'Function';
        case 'array':
            return Object.prototype.toString.call(o) === '[object Array]';
            // return Object.prototype.toString.call(o).slice(8, -1) === 'Array';
        case 'object':
            return Object.prototype.toString.call(o) === '[object Object]';
            // return Object.prototype.toString.call(o).slice(8, -1) === 'Object';
        case 'nan':
            return isNaN(o);
        case 'elements':
            return Object.prototype.toString.call(o).indexOf('HTML') !== -1
        default:
            return Object.prototype.toString.call(o)
    }
}
```
## 数据安全类型检查
``` js
function isObject(value) {
  return Object.prototype.toString.call(value).slice(8, -1) === 'Object';
}

function isArray(value) {
  return Object.prototype.toString.call(value).slice(8, -1) === 'Array';
}

function isFunction(value) {
  return Object.prototype.toString.call(value).slice(8, -1) === 'Function';
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

## 格式化数量
``` js
// 方法一
function formatNum (num, n) {
  if (typeof num == "number") {
    num = String(num.toFixed(n || 0));
    var re = /(-?\d+)(\d{3})/;
    while (re.test(num)) num = num.replace(re, "$1,$2");
    return num;
  }
  return num;
}
formatNum(2313123, 3); // "2,313,123.000"

// 方法二
'2313123'.replace(/\B(?=(\d{3})+(?!\d))/g, ','); // "2,313,123"

// 方法三
function formatNum(str) {
  return str.split('').reverse().reduce((prev, next, index) => {
    return ((index % 3) ? next : (next + ',')) + prev
  });
}
formatNum('2313323'); // "2,313,323"
```

## 身份证验证
``` js
function checkCHNCardId(sNo) {
  sNo = sNo.toString();
  if (!/^[0-9]{17}[X0-9]$/.test(sNo)) {
    return false;
  }

  var a, b, c;
  a = parseInt(sNo.substr(0, 1)) * 7 + parseInt(sNo.substr(1, 1)) * 9 + parseInt(sNo.substr(2, 1)) * 10;
  a = a + parseInt(sNo.substr(3, 1)) * 5 + parseInt(sNo.substr(4, 1)) * 8 + parseInt(sNo.substr(5, 1)) * 4;
  a = a + parseInt(sNo.substr(6, 1)) * 2 + parseInt(sNo.substr(7, 1)) * 1 + parseInt(sNo.substr(8, 1)) * 6;
  a = a + parseInt(sNo.substr(9, 1)) * 3 + parseInt(sNo.substr(10, 1)) * 7 + parseInt(sNo.substr(11, 1)) * 9;
  a = a + parseInt(sNo.substr(12, 1)) * 10 + parseInt(sNo.substr(13, 1)) * 5 + parseInt(sNo.substr(14, 1)) * 8;
  a = a + parseInt(sNo.substr(15, 1)) * 4 + parseInt(sNo.substr(16, 1)) * 2;
  b = a % 11;

  if (b == 2) {
    c = sNo.substr(17, 1).toUpperCase();
  } else {
    c = parseInt(sNo.substr(17, 1));
  }

  switch (b) {
    case 0:
      if (c != 1) {
        return false;
      }
      break;
    case 1:
      if (c != 0) {
        return false;
      }
      break;
    case 2:
      if (c != "X") {
        return false;
      }
      break;
    case 3:
      if (c != 9) {
        return false;
      }
      break;
    case 4:
      if (c != 8) {
        return false;
      }
      break;
    case 5:
      if (c != 7) {
        return false;
      }
      break;
    case 6:
      if (c != 6) {
        return false;
      }
      break;
    case 7:
      if (c != 5) {
        return false;
      }
      break;
    case 8:
      if (c != 4) {
        return false;
      }
      break;
    case 9:
      if (c != 3) {
        return false;
      }
      break;
    case 10:
      if (c != 2) {
        return false;
      };
  }
  return true;
}
```

## 使用`void 0`来解决`undefined`被污染问题
``` js
undefined = 1;
!!undefined; // true
!!void(0); // false
```

## 单行写一个评级组件
``` js
"★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
```

## JavaScript 错误处理的方式的正确姿势
``` js
try {
    // do something
} catch (e) {
    window.location.href = "http://stackoverflow.com/search?q=[js]+" + e.message;
}
```

## 匿名函数自执行写法
``` js
( function() {}() );
( function() {} )();
[ function() {}() ];

~ function() {}();
! function() {}();
+ function() {}();
- function() {}();

delete function() {}();
typeof function() {}();
void function() {}();
new function() {}();
new function() {};

var f = function() {}();

1, function() {}();
1 ^ function() {}();
1 > function() {}();
```
## 快速生成UUID
``` js
function uuid() {
  var d = new Date().getTime();
  var uuid = 'xxxxxxxxxxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
    var r = (d + Math.random() * 16) % 16 | 0;
    d = Math.floor(d / 16);
    return (c == 'x' ? r : (r & 0x3 | 0x8)).toString(16);
  });
  return uuid;
};

uuid(); // "33f7f26656cb-499b-b73e-89a921a59ba6"
```

## JavaScript浮点数精度问题
``` js
function isEqual(n1, n2, epsilon) {
  epsilon = epsilon == undefined ? 10 : epsilon; // 默认精度为10
  return n1.toFixed(epsilon) === n2.toFixed(epsilon);
}

0.1 + 0.2; // 0.30000000000000004
isEqual(0.1 + 0.2, 0.3); // true

0.7 + 0.1 + 99.1 + 0.1; // 99.99999999999999
isEqual(0.7 + 0.1 + 99.1 + 0.1, 100); // true
```
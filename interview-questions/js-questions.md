[toc]

### JS篇

#### ['1', '2', '3'].map(parseInt)
看到这个题，下意识说[1, 2, 3]，然而并不是~正确答案是<font color="red">[1, NaN, NaN]</font>
先来看下[map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)函数的用法:
```js
var new_array = arr.map(function callback(currentValue[, index[, array]])) {
    // return element for new_array
}[, thisArg]
```
1. callback
    生成新数组元素的函数，使用三个参数：
    + currentValue
      callback 数组中正在处理的当前元素。
    + index | 可选
      callback 数组中正在处理的当前元素的索引。
    + array | 可选
      map方法调用的数组。
2. thisArg | 可选
   执行callback函数时被用作this
map方法会给原数组中的每个元素都按顺序调用一次callback函数。callback每次执行后的返回值（包括undefined）组合起来形成一个新数组。callback函数只会在有值的索引上被调用，那些从没被赋过值或者delete删除的所有则不会被调用。

接下来，是[parseInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)函数用法：
```js
parseInt(string, radix)
```
1. string
   要被解析的值。如果参数不是一个字符串，则将其转换为字符串，字符串开头的空白符将会被忽略。
2. radix | 可选
   从2到36，代表该进位系统的数字。
将一个字符串string转换为radix进制的整数，radix为介于2-36之间的数。或者<font color="red">NaN</font>。当
    + radix小于2或者大于36，或
    + 第一个非空格字符串不能转换为数字

当参数radix的值为0，或者没有设置该参数时，parseInt()会根据string来判断数字的基数。规则如下：
 + 若string以'0x'或'0X'开头，parseInt()会把string的其余部分解析为十六进制的整数。
 + 如果string以0开头，ES版本小于5时，默认使用八进制，ES5使用的是十进制。
 + 若string以0~9的数字开头，parseInt()将他解析为十进制的整数。

 如果parseInt遇到的字符不是指定radix参数中的数字，它将回了该字符以及所有后续字符，并返回到该点为止已解析的整数值。parseInt将数字截断为整数值。允许前导和尾随空格。
 
 <font color="blue">正常情况下，</font>string左起第一个数一旦大于进制数radix，立即返回NaN；否则才去做运算，直到遇到一个大于等于radix，就停止运算。

```js
// map函数将当前被处理的函数和其索引传入parseInt,
parseInt('1', 0); // radix为0，且string不以'0X'或'0x'开头，按照十进制解析，为1
parseInt('2', 1); // 1进制表示的数中，最大值小于2，所以无法解析，NaN。基数为1（1进制）的时候，返回结果都是NaN
parseInt('3', 2); // 2进制表示的数中，最大值小于3，所以无法解析，NaN。二进制表示数中只能存在01
```

#### 什么是防抖和节流？有啥区别？如何实现？
<font color="blue">相同之处：</font>
二者都是优化JS代码执行频率较高时做的优化，如浏览器的resize、scroll，鼠标的mousemove事件，input的keypress/keydown等事件在触发时，会不停的执行回调函数，浪费资源，影响前端性能。为了优化体验，需要对这类事件的执行进行限制，减少执行次数。
<font color="blue">不同之处：</font>
1. 函数防抖，从上一次被调用后，延迟N秒后再调用，如果N秒内又被触发，则重新计时。

```js
// 思路：每次执行函数之前，取消之前的延时调用。
function debounce(fn, delay = 300) {
    let timeout = null;
    return function() {
        const _this = this;
        if (timeout) { // 清除前一个setTimeout
            clearTimeout(timeout); 
        }
        timeout = setTimeout(() => {
            fn.apply(_this, arguments); // apply指向调用debounce的对象
        }, delay);
    };
}
// test
function say() {
    console.log('hi');
}
document.addEventListener('mousemove', debounce(say));
```

2. 函数节流，在N秒内，最多执行函数一次。
```js
// 思路：每次执行前，判断当前是否有等待执行的延时函数。
function throttle(fn, delay = 300) {
    let canUse = true; // 标记
    return function() {
        const _this = this;
        if (!canUse) { // 判断是否可以继续执行
            return;
        }
        canUse = false;
        setTimeout(() => {
            fn.applay(_this, argumens);
            canUse = true; // 可以执行下次循环了
        }, delay);
    };
}
// test
function say() {
    console.log('hi');
}
document.addEventListener('mousemove', debounce(say));
```
上面只是实现了最基础的功能，还有更全面的。平时使用时可以用[lodash](https://www.lodashjs.com/docs/lodash.throttle)或者[underscore](https://underscorejs.bootcss.com/)~
对实现函数更多功能有兴趣的可以移步[JavaScript专题之跟着underscore学防抖
](https://github.com/mqyqingfeng/Blog/issues/22)，[JavaScript专题之跟着underscore学节流](https://github.com/mqyqingfeng/Blog/issues/26)
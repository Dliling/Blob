[toc]

### ES6

#### 介绍下Set、Map、WeakSet、WeakMap，有何区别
全面的介绍请移步[这里](https://es6.ruanyifeng.com/#docs/set-map)

Set和Map主要的应用场景在于数据重组和数据储存。
Set是一种集合，Map是一种字典。

**Set**
*基本用法*
类似于数组，但成员是唯一的，没有重复的值。
Set本身是一个构造函数，用来生成Set数据结构。
```js
const s = new Set();
// 通过add()向Set结构加入成员
[1, 2, 3, 3, 4, 2, 5].forEach(x => s.add(x));
for (let i of s) {
    console.log(i); // 1, 2, 3, 4, 5
}
```
Set函数可以接受一个数组（或者具有iterable接口的其他数据结构）作为参数，用来初始化。
```js
// 传入一个数组
const set = new Set([1, 2, 3, 3, 4, 2, 5]);

console.log([...set]); // 1, 2, 3, 4, 5
console.log(set.size); // 5

// 传入一个类数组的对象
const set = new Set(document.querySelectorAll('div'));
```
考虑到Set结构成员的唯一性，我们可以用来做去重。
```js
// 数组去重
[...new Set(array)];
// 字符串去重
[...new Set('ababd')].join(''); // abd
```
向Set加入值时，不会做类型转换，即2和'2'是两个不同的值。但是两个NaN是相等的，两个空对象是不相等的。
```js
// 2和'2'是两个不同的值
console.log(new Set([2, '2'])); // {2, '2'}

// 两个NaN是相等的
let s = new Set();
s.add(NaN);
s.add(1);
s.add(NaN);
console.log(s); // {NaN, 1}

// 两个空对象是不相等的
let s = new Set();
s.add({});
s.add(1);
s.add({});
console.log(s, s.size); // {{}, 1, {}} 3
```
*Set实例的属性和方法*

Set实例的属性
 + Set.prototype.constructor：构造函数，默认是Set函数
 + Set.prototype.size：返回Set实例的成员总数

Set实例的方法，操作方法（用于操作数据）和遍历方法（用于遍历成员）。
1. 操作方法
    + Set.prototype.add(value)：添加某个值，返回Set结构本身
    + Set.prototype.delete(value)：删除某个值，返回一个Boolean，表示删除是否成功。
    + Set.prototype.has(value)：该value是否是Set的成员，返回一个Boolean。
    + Set.prototype.clear()：清空所有成员，没有返回值。
2. 遍历方法
    + Set.prototype.keys()：返回键名
    + Set.prototype.values()：返回键值
    + Set.prototype.entries()：返回键值对
    + Set.prototype.forEach()：遍历每个成员。

需要特别指出的是，<font color=blue>Set的遍历顺序就是添加时的顺序。</font>

由于Set结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys()，values()返回的内容一致

<font color=red>Array.from</font>可以将Set结构转为数组，提供了数组去重的另一个方法
```js
Array.from(new Set([1, 1, 2, 3, 2])); // [1, 2, 3]
```
若想在遍历操作中，同步改变原来的Set结构。一种是利用原Set结构映射出一个新的结构，然后赋值给原来的Set结构；另一种就是利用Array.from方法。
```js
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));

set = new Set(Array.from(set, val => val * 2));
```

**WeakSet**

*含义和用法*
与Set类似，成员都不重复，但是，WeakSet的成员只能是对象，不能是其他类型。

WeakSet中的对象都是若引用，即垃圾回收机制不考虑WeakSet对该对象的引用，。若其他对象没有再引用该对象，垃圾回收机制自动回收该对象。

适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，在WeakSet里的引用就会自动消失。所以WeakSet可能会随时消失。因此，WeakSet不使用被引用，内部成员不确定，不可遍历。

```js
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a); // {[1, 2], [3, 4]}
```
上面代码中，a数组的成员是WeakSet的成员。
```js
const b = [3, 4];
const ws = new WeakSet(b); // Uncaught TypeError: Invalid value inn weak set
```

WeakSet有三个方法：
 + WeakSet.prototype.add(value)：添加一个value成员。
 + WeakSet.prototype.delete(value)：删除value，返回Boolean，表示删除是否成功。
 + WeakSet.prototype.has(value)：返回Boolean，表示某个值是否存在。

 **Map**
 *含义和基本用法*
 类似于对象，也是键值对的集合，但是"键"可以是各种类型的值。
 ```js
 const m = new Map();
 const o = {p: 'hello'};
 m.set(o, 'content');
 m.get(o); // content

 // 接受一个数组为参数
 const map = new Map([
     ['name', 'tony'],
     ['title', 'teacher']
 ]);
 map.size // 2
 map.has('name'); // true
 ```
 不仅是数组，任何具有Iterator接口、且每个成员都是一个双元素的数组的数据结构，都可以当做Map构造函数的参数。

 如果对同一个键值多次赋值，后面的值会覆盖前面的值。

 如果读取一个未知的值，则返回undefined。

 只有对同一个对象的引用，Map结构才能将其视为同一个键。
 ```js
 const map = new Map();
 map.set(['a'], 3);
 map.get(['a']); // undefined
 // 这是两个不同的数组实例，内存地址不一样。

// k1和k2的值是一样的，但是被视为两个键。
 const map = new Map();
 const k1 = ['a'];
 const k2 = ['a'];
 map.set(k1, 1).set(k2, 2);
 map.get(k1); // 1
 map.get(k2); // 2
 ```
Map的键实际是和内存地址绑定的，只要内存地址不一样，就视为两个键。

如果Map的键是一个简单类型的值，只要两个值严格相等，就视为同一个键。undefined和null是两个不通的值。但是NaN视为同一个键。

*属性和方法*
1. size属性：返回Map结构的成员总数
2. Map.prototype.set(key, value)：设置键名key对应的值为value，返回整个Map结构。若key已有值，则键值会被更新。可链式调用。
3. Map.prototype.get(key)：读取key对应的键值。若无则为undefined。
4. Map.prototype.has(key)：key是否在Map里。返回Boolean。
5. Map.prototype.delete(key)：删除key，返回Boolean。
6. Map.prototype.clear()：清空，没有返回值。

*遍历方法* <font color=blue>遍历顺序就是插入顺序</font>
1. Map.prototype.keys()：返回键名
1. Map.prototype.values()：返回键值
1. Map.prototype.entries()：返回所有成员
1. Map.prototype.forEach()：遍历所有成员

**WeakMap**
*含义*
WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。

键名引用都是弱引用，垃圾回收机制不将该引用考虑在内。若你想在对象上添加数据，又不想干扰垃圾回收机制，可以使用WeakMap。键值是正常引用。
```js
const wm = new WeakMap();
let key = {};
let obj = {foo: 1};

wm.set(key, obj);
obj = null;
wm.get(key); // {foo: 1}
```
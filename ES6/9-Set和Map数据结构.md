### Set和Map数据结构

Set集合                                                                                 Map集合

Array							                                                           Object

new Set()                                                                              new Map()

增删改查                                                                                 增删改查

遍历																							遍历

set.add(val)    //添加数据                                                    map.set(key,value)    //添加数据

set.delete(val)   //删除数据

set.clear()      //清空数据

Set      --对比Array

里面存放的是值，里面没有重复的数据

Map    --对比Object

里面存放的是键值对，键值可以是任何数据类型，对象的键只能是字符串类型

#### 1.Set

##### 基本用法

ES6提供了新数据结构Set。它类似数组，但成员的值都是唯一的，没有重复的值。

`Set`本身是一个构造函数，用来生成Set数据结构。

```js
const set = new Set()
[1,2,3,4,5].forEach( item => set.add(item))

for(let item of set){
    console.log(item)
}
```

`Set`函数可以接受一个数组（或者具有Iterator接口的其它数据结构）作为参数，用来初始化

```js
const set = new Set([1,2,3,4,5])
[...set]
//[1,2,3,4,5]
console.log(set.size)    //5
```

向Set加入值的时候，不会发生类型转换，所以`5`和`'5'`是两个不同的值。Set内部判断两个值是否不同，使用的算法叫做“Same-value-zero  equality”，它类似于精确相等运算符（===），主要区别是向Set加入值时认为`NaN`等与自身，而精确相等运算符认为`NaN`不等与自身。

```js
let set = new Set()
let a = NaN
let b = NaN
set.add(a)
set.add(b)
set   // Set {NaN}
```

另外，两个对象总是不相等的

```js
let set = new Set()
set.add({})
set.add({})

set.size    //2
```

##### 应用

数组去重

```js
[...new Set([1,2,3,4,5,2,6,4])]    //[1,2,3,4,5,6]
```

字符串去重

```js
[...new Set('ababbc')].join('')    //'abc'
```

##### Set实例的属性和方法

###### 属性

- `Set.prototype.constructor`:构造函数，默认就是`Set`函数
- `Set.prototype.size`:返回Set实例的成员总数

###### 方法

- `Set.prototype.add(value)`:添加某个值，返回Set结构本身
- `Set.prototype.delete(value)`:删除某个值，返回一个布尔值，表示删除是否成功
- `Set.prorotype.has(value)`:返回一个布尔值，表示该值是否为`Set`的成员
- `Set.prototypr.clear()`:清除所有成员，没有返回值

```js
let s = new Set()

s.add(1).add(2).add(2)

s.size  //2,因为在set里面不允许重复成员，所以2只会加入一次

s.has(1)   //true
s.has(2)   //true
s.delete(2)  
s.has(2)   //false
```

##### 遍历操作

Set结构的实例有四个遍历方法，可以用于遍历成员

- `Set.prototype.keys()`:返回键名的遍历器
- `Set.prototype.values()`:返回键值的遍历器
- `Set.prototype.entries()`:返回键值对的遍历器
- `Set.prototype.forEach()`:使用回调函数遍历每个成员

`Set`的遍历顺序就是插入顺序

###### （1）keys(), values(), entries()

keys(),values(),entries()方法返回的都是遍历器对象

由于Set结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys()f方法和values()方法的行为完全一致。

```js
let set = new Set(['red','green','blue'])

for(let item of set.keys()){
    console.log(item)
}
//red
//green
//blue
for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

Set结构的实例默认可遍历，它的默认遍历器生成的函数就是它的`values`方法

```js
Set.prototype[Symbol.iterator] === Set.prototype.values
```

这意味着，可以省略`values`方法，直接用`for...of`循环遍历Set

```js
let set = new Set(['red', 'green', 'blue'])
for(let item of set){
    console.log(item)
}
//red
//green
//blue
```

###### (2)forEach()

Set结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，没有返回值

```js
let set = new Set([1,2,3])
set.forEach((value,key,set){
    console.log(key,value)
})
//1 1
//2 2
//3 3
```

#### 2.WeakSet

##### 含义

WeakSet结构与Set类似，也是不重复的值的集合，但是，它与Set有两个区别。

首先，WeakSet的成员只能是对象，而不能是其他类型的值

```js
const ws = new WeakSet()
ws.add(1)
//报错
ws.add({})
```

其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。

#### 3.Map

##### 含义和基本用法

JavaScript的对象，本质上是键值对的集合但传统上只能用字符串当做键。折给他的使用带来了很大的限制

为了解决这个问题，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值度可以当做键。

```js
let map = new Map()
const obj = {p: 'hello'}
map.set(obj,'content')
map.get(obj)   //'content'

map.has(obj)   //true
map.delete(obj)  //true
map.has(obj)   //false
```

对象相同的键连续赋值，后一次的值会覆盖前一次的值

如果读取一个未知的键，则返回`undefined`

**注意**只有对同一个对象的引用，Map结构才将其视为同一个键，引用数据类型必须内存地址一样，才可能是同一个键

如果Map的键是一个简单类型的值（数字，字符串，布尔值），则只要两个值严格相等，Map将其视为一个键，比如`0`和`-0`就是一个键，布尔值true和字符串true则是两个不同的键。虽然`NaN`不严格相等于自身，但Map将其视为同一个键。

##### 实例的属性和操作方法

###### （1）size属性

`size`属性返回Map结构的成员总数

```js
const map = new Map()
map.set('foo',true)
map.set('bar',false)
map.size   //2
```

###### （2）Map.prototype.set(key,value)

`set`方法设置键名`key`对应的键值`value`,然后返回整个Map结构，如果`key`已经有值，则键值会被更新，否则就生成该键

```js
const map = new Map()

map.set('a', '123')
map.set('a', '666')
map.get(a)   // '666'
```

###### （3）Map.prototype.get(key)

`get`方法读取`key`对应的键值，如果找不到`key`,返回`undefined`

```js
const map = new Map()
```

```javascript
const m = new Map();

const hello = function() {console.log('hello');};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!
```

###### （4）Map.prototype.has(key)

`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```javascript
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true
```

###### （5）Map.prototype.delete(key)

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

```javascript
const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false
```

###### （6）Map.prototype.clear()

`clear`方法清除所有成员，没有返回值。

```javascript
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```

##### 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

需要特别注意的是，Map 的遍历顺序就是插入顺序。

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

上面代码最后的那个例子，表示 Map 结构的默认遍历器接口（`Symbol.iterator`属性），就是`entries`方法。

```javascript
map[Symbol.iterator] === map.entries
// true
```

Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

```javascript
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

结合数组的`map`方法、`filter`方法，可以实现 Map 的遍历和过滤（Map 本身没有`map`和`filter`方法）。

```javascript
const map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

const map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生 Map 结构 {1 => 'a', 2 => 'b'}

const map2 = new Map(
  [...map0].map(([k, v]) => [k * 2, '_' + v])
    );
// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```

此外，Map 还有一个`forEach`方法，与数组的`forEach`方法类似，也可以实现遍历。

```javascript
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

`forEach`方法还可以接受第二个参数，用来绑定`this`。

```javascript
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
```

上面代码中，`forEach`方法的回调函数的`this`，就指向`reporter`。

##### 与其他数据结构的互相转换

**（1）Map 转为数组**

前面已经提过，Map 转为数组最方便的方法，就是使用扩展运算符（`...`）。

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

**（2）数组 转为 Map**

将数组传入 Map 构造函数，就可以转为 Map。

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

**（3）Map 转为对象**

如果所有 Map 的键都是字符串，它可以无损地转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

**（4）对象转为 Map**

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

**（5）Map 转为 JSON**

Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。

```javascript
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

**（6）JSON 转为 Map**

JSON 转为 Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```

但是，有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。

```javascript
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```




























## 1、all

如果数组所有元素满足函数条件，则返回true。调用时，如果省略第二个参数，则默认传递布尔值。

```js
const all = (arr, fn = Boolean) => arr.every(fn);

all([4, 2, 3], x => x > 1); // true
all([1, 2, 3]); // true
```

## 2、allEqual

判断数组中的元素是否都相等

```
const allEqual = arr => arr.every(val => val === arr[0]);

allEqual([1, 2, 3, 4, 5, 6]); // false
allEqual([1, 1, 1, 1]); // true
```

## 3、approximatelyEqual

此代码示例检查两个数字是否近似相等，差异值可以通过传参的形式进行设置

```
const approximatelyEqual = (v1, v2, epsilon = 0.001) => Math.abs(v1 - v2) < epsilon;

approximatelyEqual(Math.PI / 2.0, 1.5708); // true
```

## 4、arrayToCSV

此段代码将没有逗号或双引号的元素转换成带有逗号分隔符的字符串即CSV格式识别的形式。

```
const arrayToCSV = (arr, delimiter = ',') =>
  arr.map(v => v.map(x => `"${x}"`).join(delimiter)).join('\n');
  
arrayToCSV([['a', 'b'], ['c', 'd']]); // '"a","b"\n"c","d"'
arrayToCSV([['a', 'b'], ['c', 'd']], ';'); // '"a";"b"\n"c";"d"'
```

## 5、arrayToHtmlList

此段代码将数组元素转换成<li>标记，并将此元素添加至给定的ID元素标记内。

```
const arrayToHtmlList = (arr, listID) =>
  (el => (
    (el = document.querySelector('#' + listID)),
    (el.innerHTML += arr.map(item => `<li>${item}</li>`).join(''))
  ))();
  
arrayToHtmlList(['item 1', 'item 2'], 'myListID');
```

## 6、attempt

此段代码执行一个函数，将剩余的参数传回函数当参数，返回相应的结果，并能捕获异常。

```
const attempt = (fn, ...args) => {
  try {
    return fn(...args);
  } catch (e) {
    return e instanceof Error ? e : new Error(e);
  }
};
var elements = attempt(function(selector) {
  return document.querySelectorAll(selector);
}, '>_>');
if (elements instanceof Error) elements = []; // elements = []
```

## 7、average

此段代码返回两个或多个数的平均数。

```
const average = (...nums) => nums.reduce((acc, val) => acc + val, 0) / nums.length;
average(...[1, 2, 3]); // 2
average(1, 2, 3); // 2
```

## 8、averageBy

一个 map()函数和 reduce()函数结合的例子，此函数先通过 map() 函数将对象转换成数组，然后在调用reduce()函数进行累加，然后根据数组长度返回平均值。

```
const averageBy = (arr, fn) =>
  arr.map(typeof fn === 'function' ? fn : val => val[fn]).reduce((acc, val) => acc + val, 0) /
  arr.length;
  
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], o => o.n); // 5
averageBy([{ n: 4 }, { n: 2 }, { n: 8 }, { n: 6 }], 'n'); // 5
```

## 9、bifurcate

此函数包含两个参数，类型都为数组，依据第二个参数的真假条件，将一个参数的数组进行分组，条件为真的放入第一个数组，其它的放入第二个数组。这里运用了Array.prototype.reduce() 和 Array.prototype.push() 相结合的形式。

```
const bifurcate = (arr, filter) =>
  arr.reduce((acc, val, i) => (acc[filter[i] ? 0 : 1].push(val), acc), [[], []]);
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]); 
// [ ['beep', 'boop', 'bar'], ['foo'] ]
```

## 10、bifurcateBy

此段代码将数组按照指定的函数逻辑进行分组，满足函数条件的逻辑为真，放入第一个数组中，其它不满足的放入第二个数组 。这里运用了Array.prototype.reduce() 和 Array.prototype.push() 相结合的形式，基于函数过滤逻辑，通过 Array.prototype.push() 函数将其添加到数组中。

```js
const bifurcateBy = (arr, fn) =>
  arr.reduce((acc, val, i) => (acc[fn(val, i) ? 0 : 1].push(val), acc), [[], []]);
  
bifurcateBy(['beep', 'boop', 'foo', 'bar'], x => x[0] === 'b'); 
// [ ['beep', 'boop', 'bar'], ['foo'] ]
```

## 11、bottomVisible

用于检测页面是否滚动到页面底部。

```
const bottomVisible = () =>
  document.documentElement.clientHeight + window.scrollY >=
  (document.documentElement.scrollHeight || document.documentElement.clientHeight);

bottomVisible(); // true
```

## 12、byteSize

此代码返回字符串的字节长度。这里用到了Blob对象，Blob（Binary Large Object）对象代表了一段二进制数据，提供了一系列操作接口。其他操作二进制数据的API（比如File对象），都是建立在Blob对象基础上的，继承了它的属性和方法。生成Blob对象有两种方法：一种是使用Blob构造函数，另一种是对现有的Blob对象使用slice方法切出一部分。

```
const byteSize = str => new Blob([str]).size;

byteSize('😀'); // 4
byteSize('Hello World'); // 11
```

## 13、capitalize

将字符串的首字母转成大写,这里主要运用到了ES6的展开语法在数组中的运用。

```
const capitalize = ([first, ...rest]) =>
  first.toUpperCase() + rest.join('');
  
capitalize('fooBar'); // 'FooBar'
capitalize('fooBar', true); // 'FooBar'
```

## 14、capitalizeEveryWord

将一个句子中每个单词首字母转换成大写字母，这里中要运用了正则表达式进行替换。

```
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());

capitalizeEveryWord('hello world!'); // 'Hello World!'
```

## 15、castArray

此段代码将非数值的值转换成数组对象。

```
const castArray = val => (Array.isArray(val) ? val : [val]);

castArray('foo'); // ['foo']
castArray([1]); // [1]
```

## 16、compact

将数组中移除值为 false 的内容。

```
const compact = arr => arr.filter(Boolean);

compact([0, 1, false, 2, '', 3, 'a', 'e' * 23, NaN, 's', 34]); 
// [ 1, 2, 3, 'a', 's', 34 ]
```

## 17、countOccurrences

统计数组中某个值出现的次数

```
const countOccurrences = (arr, val) => arr.reduce((a, v) => (v === val ? a + 1 : a), 0);
countOccurrences([1, 1, 2, 1, 2, 3], 1); // 3
```

## 18、Create Directory

此代码段使用 existSync() 检查目录是否存在，然后使用 mkdirSync() 创建目录（如果不存在）。

```
const fs = require('fs');
const createDirIfNotExists = dir => (!fs.existsSync(dir) ? fs.mkdirSync(dir) : undefined);
createDirIfNotExists('test'); 
// creates the directory 'test', if it doesn't exist
```

## 19、currentURL

返回当前访问的 URL 地址。

```
const currentURL = () => window.location.href;

currentURL(); // 'https://medium.com/@fatosmorina'
```

## 20、dayOfYear

返回当前是今年的第几天

```
const dayOfYear = date =>
  Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date()); // 272
```

## 21、decapitalize

将字符串的首字母转换成小写字母

```
const decapitalize = ([first, ...rest]) =>
  first.toLowerCase() + rest.join('')

decapitalize('FooBar'); // 'fooBar'
```

**22、deepFlatten**

通过递归的形式，将多维数组展平成一维数组。

```
const deepFlatten = arr => [].concat(...arr.map(v => (Array.isArray(v) ? deepFlatten(v) : v)));

deepFlatten([1, [2], [[3], 4], 5]); // [1,2,3,4,5]
```

# 23、default

去重对象的属性，如果对象中含有重复的属性，以前面的为准。

```
const defaults = (obj, ...defs) => Object.assign({}, obj, ...defs.reverse(), obj);

defaults({ a: 1 }, { b: 2 }, { b: 6 }, { a: 3 }); // { a: 1, b: 2 }
```

# 24、defer

延迟函数的调用，即异步调用函数。

```
const defer = (fn, ...args) => setTimeout(fn, 1, ...args);

defer(console.log, 'a'), console.log('b'); // logs 'b' then 'a'
```

# 25、degreesToRads

此段代码将标准的度数，转换成弧度。

```
const degreesToRads = deg => (deg * Math.PI) / 180.0;

degreesToRads(90.0); // ~1.5708
```

# 26、difference

此段代码查找两个给定数组的差异，查找出前者数组在后者数组中不存在元素。

```
const difference = (a, b) => {
  const s = new Set(b);
  return a.filter(x => !s.has(x));
};

difference([1, 2, 3], [1, 2, 4]); // [3]
```

# 27、differenceBy

通过给定的函数来处理需要对比差异的数组，查找出前者数组在后者数组中不存在元素。

```
const differenceBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.filter(x => !s.has(fn(x)));
};

differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [1.2]
differenceBy([{ x: 2 }, { x: 1 }], [{ x: 1 }], v => v.x); // [ { x: 2 } ]
```

# 28、differenceWith

此段代码按照给定函数逻辑筛选需要对比差异的数组，查找出前者数组在后者数组中不存在元素。

```
const differenceWith = (arr, val, comp) => arr.filter(a => val.findIndex(b => comp(a, b)) === -1);

differenceWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0], (a, b) => Math.round(a) === Math.round(b)); 
// [1, 1.2]
```

# 29、digitize

将输入的数字拆分成单个数字组成的数组。

```
const digitize = n => [...`${n}`].map(i => parseInt(i));

digitize(431); // [4, 3, 1]
```

# 30、distance

计算两点之间的距离

```
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);

distance(1, 1, 2, 3); // 2.23606797749979
```

# 31、drop

此段代码将给定的数组从左边开始删除 n 个元素

```
const drop = (arr, n = 1) => arr.slice(n);

drop([1, 2, 3]); // [2,3]
drop([1, 2, 3], 2); // [3]
drop([1, 2, 3], 42); // []
```

# 32、dropRight

此段代码将给定的数组从右边开始删除 n 个元素

```
const dropRight = (arr, n = 1) => arr.slice(0, -n);

dropRight([1, 2, 3]); // [1,2]
dropRight([1, 2, 3], 2); // [1]
dropRight([1, 2, 3], 42); // []
```

# 33、dropRightWhile

此段代码将给定的数组按照给定的函数条件从右开始删除，直到当前元素满足函数条件为True时，停止删除，并返回数组剩余元素。

```
const dropRightWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[arr.length - 1])) arr = arr.slice(0, -1);
  return arr;
};

dropRightWhile([1, 2, 3, 4], n => n < 3); // [1, 2]
```

# 34、dropWhile

按照给定的函数条件筛选数组，不满足函数条件的将从数组中移除。

```
const dropWhile = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr = arr.slice(1);
  return arr;
};

dropWhile([1, 2, 3, 4], n => n >= 3); // [3,4]
```

# 35、elementContains

接收两个DOM元素对象参数，判断后者是否是前者的子元素。

```
const elementContains = (parent, child) => parent !== child && parent.contains(child);

elementContains(document.querySelector('head'), document.querySelector('title')); // true
elementContains(document.querySelector('body'), document.querySelector('body')); // false
```

# 36、filterNonUnique

移除数组中重复的元素

```
const filterNonUnique = arr => [ …new Set(arr)];
filterNonUnique([1, 2, 2, 3, 4, 4, 5]); // [1, 2, 3, 4, 5]
```

# 37、findKey

按照给定的函数条件，查找第一个满足条件对象的键值。

```
const findKey = (obj, fn) => Object.keys(obj).find(key => fn(obj[key], key, obj));

findKey(
  {
    barney: { age: 36, active: true },
    fred: { age: 40, active: false },
    pebbles: { age: 1, active: true }
  },
  o => o['active']
); // 'barney'
```

# 38、findLast

按照给定的函数条件筛选数组，将最后一个满足条件的元素进行删除。

```
const findLast = (arr, fn) => arr.filter(fn).pop();

findLast([1, 2, 3, 4], n => n % 2 === 1); // 3
```

# 39、flatten

按照指定数组的深度，将嵌套数组进行展平。

```
const flatten = (arr, depth = 1) =>
  arr.reduce((a, v) => a.concat(depth > 1 && Array.isArray(v) ? flatten(v, depth - 1) : v), []);

flatten([1, [2], 3, 4]); // [1, 2, 3, 4]
flatten([1, [2, [3, [4, 5], 6], 7], 8], 2); // [1, 2, 3, [4, 5], 6, 7, 8]
```

# 40、forEachRight

按照给定的函数条件，从数组的右边往左依次进行执行。

```
const forEachRight = (arr, callback) =>
  arr
    .slice(0)
    .reverse()
    .forEach(callback);
    
forEachRight([1, 2, 3, 4], val => console.log(val)); // '4', '3', '2', '1'
```

# 41、forOwn

此段代码按照给定的函数条件，进行迭代对象。

```
const forOwn = (obj, fn) => Object.keys(obj).forEach(key => fn(obj[key], key, obj));
forOwn({ foo: 'bar', a: 1 }, v => console.log(v)); // 'bar', 1
```

# 42、functionName

此段代码输出函数的名称。

```
const functionName = fn => (console.debug(fn.name), fn);

functionName(Math.max); // max (logged in debug channel of console)
```

# 43、getColonTimeFromDate

此段代码从Date对象里获取当前时间。

```
const getColonTimeFromDate = date => date.toTimeString().slice(0, 8);
getColonTimeFromDate(new Date()); // "08:38:00"
```

# 44、getDaysDiffBetweenDates

此段代码返回两个日期之间相差多少天

```
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
  (dateFinal - dateInitial) / (1000 * 3600 * 24);
  
getDaysDiffBetweenDates(new Date('2019-01-13'), new Date('2019-01-15')); // 2
```

# 45、getStyle

此代码返回DOM元素节点对应的属性值。

```
const getStyle = (el, ruleName) => getComputedStyle(el)[ruleName];

getStyle(document.querySelector('p'), 'font-size'); // '16px'
```

# 46、getType

此段代码的主要功能就是返回数据的类型。

```
const getType = v =>
  v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();
  
getType(new Set([1, 2, 3])); // 'set'
```

# 47、hasClass

此段代码返回DOM元素是否包含指定的Class样式。

```
const hasClass = (el, className) => el.classList.contains(className);
hasClass(document.querySelector('p.special'), 'special'); // true
```

# 48、head

此段代码输出数组的第一个元素。

```
const head = arr => arr[0];

head([1, 2, 3]); // 1
```

# 49、hide

此段代码隐藏指定的DOM元素。

```
const hide = (...el) => [...el].forEach(e => (e.style.display = 'none'));

hide(document.querySelectorAll('img')); // Hides all <img> elements on the page
```

# 50、httpsRedirect

此段代码的功能就是将http网址重定向https网址。

```
const httpsRedirect = () => {
  if (location.protocol !== 'https:') location.replace('https://' + location.href.split('//')[1]);
};

httpsRedirect(); // If you are on http://mydomain.com, you are redirected to https://mydomain.com
```

# 51、indexOfAll

此代码可以返回数组中某个值对应的所有索引值，如果不包含该值，则返回一个空数组。

```js
const indexOfAll = (arr, val) => arr.reduce((acc, el, i) => (el === val ? [...acc, i] : acc), []);

indexOfAll([1, 2, 3, 1, 2, 3], 1); // [0,3]
indexOfAll([1, 2, 3], 4); // []
```

# 52、initial

此段代码返回数组中除最后一个元素的所有元素

```
const initial = arr => arr.slice(0, -1);

initial([1, 2, 3]); // [1,2]const initial = arr => arr.slice(0, -1);
initial([1, 2, 3]); // [1,2]
```

# 53、insertAfter

此段代码的功能主要是在给定的DOM节点后插入新的节点内容

```
const insertAfter = (el, htmlString) => el.insertAdjacentHTML('afterend', htmlString);

insertAfter(document.getElementById('myId'), '<p>after</p>'); // <div id="myId">...</div> <p>after</p>
```

# 54、insertBefore

此段代码的功能主要是在给定的DOM节点前插入新的节点内容

```
const insertBefore = (el, htmlString) => el.insertAdjacentHTML('beforebegin', htmlString);

insertBefore(document.getElementById('myId'), '<p>before</p>'); // <p>before</p> <div id="myId">...</div>
```

# 55、intersection

此段代码返回两个数组元素之间的交集。

```
const intersection = (a, b) => {
  const s = new Set(b);
  return a.filter(x => s.has(x));
};

intersection([1, 2, 3], [4, 3, 2]); // [2, 3]
```

# 56、intersectionBy

按照给定的函数处理需要对比的数组元素，然后根据处理后的数组，找出交集，最后从第一个数组中将对应的元素输出。

```js
const intersectionBy = (a, b, fn) => {
  const s = new Set(b.map(fn));
  return a.filter(x => s.has(fn(x)));
};

intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor); // [2.1]
```

# 57、intersectionBy

按照给定的函数对比两个数组的差异，然后找出交集，最后从第一个数组中将对应的元素输出。

```js
const intersectionWith = (a, b, comp) => a.filter(x => b.findIndex(y => comp(x, y)) !== -1);

intersectionWith([1, 1.2, 1.5, 3, 0], [1.9, 3, 0, 3.9], (a, b) => Math.round(a) === Math.round(b)); // [1.5, 3, 0]
```

# 58、is

此段代码用于判断数据是否为指定的数据类型，如果是则返回true。

```js
const is = (type, val) => ![, null].includes(val) && val.constructor === type;

is(Array, [1]); // true
is(ArrayBuffer, new ArrayBuffer()); // true
is(Map, new Map()); // true
is(RegExp, /./g); // true
is(Set, new Set()); // true
is(WeakMap, new WeakMap()); // true
is(WeakSet, new WeakSet()); // true
is(String, ''); // true
is(String, new String('')); // true
is(Number, 1); // true
is(Number, new Number(1)); // true
is(Boolean, true); // true
is(Boolean, new Boolean(true)); // true
```

# 59、isAfterDate

接收两个日期类型的参数，判断前者的日期是否晚于后者的日期。

```
const isAfterDate = (dateA, dateB) => dateA > dateB;

isAfterDate(new Date(2010, 10, 21), new Date(2010, 10, 20)); // true
```

# 60、isAnagram

用于检测两个单词是否相似。

```
const isAnagram = (str1, str2) => {
  const normalize = str =>
    str
      .toLowerCase()
      .replace(/[^a-z0-9]/gi, '')
      .split('')
      .sort()
      .join('');
  return normalize(str1) === normalize(str2);
};

isAnagram('iceman', 'cinema'); // true
```

# 61、isArrayLike

此段代码用于检测对象是否为类数组对象,是否可迭代。

```
const isArrayLike = obj => obj != null && typeof obj[Symbol.iterator] === 'function';

isArrayLike(document.querySelectorAll('.className')); // true
isArrayLike('abc'); // true
isArrayLike(null); // false
```

# 62、isBeforeDate

接收两个日期类型的参数，判断前者的日期是否早于后者的日期。

```
const isBeforeDate = (dateA, dateB) => dateA < dateB;

isBeforeDate(new Date(2010, 10, 20), new Date(2010, 10, 21)); // true
```

# 63、isBoolean

此段代码用于检查参数是否为布尔类型。

```
const isBoolean = val => typeof val === 'boolean';

isBoolean(null); // false
isBoolean(false); // true
```

# 64、Object.key(obj)

获取对象的key数组

```js
let jsonObj = { Name: ‘richard‘, Value: ‘8‘ };

Object.keys(jsonObj); // [ 'Name', 'Value' ]
Object.keys(jsonObj).length; // 2
```

# **64、getColonTimeFromDate**

用于判断程序运行环境是否在浏览器，这有助于避免在node环境运行前端模块时出错。

```
const isBrowser = () => ![typeof window, typeof document].includes('undefined');
isBrowser(); // true (browser)isBrowser(); // false (Node)
```

# **65、isBrowserTabFocused**

用于判断当前页面是否处于活动状态（显示状态）。

```
const isBrowserTabFocused = () => !document.hidden;isBrowserTabFocused(); // true
```

# **66、isLowerCase**

用于判断当前字符串是否都为小写。

```
const isLowerCase = str => str === str.toLowerCase();
isLowerCase('abc'); // trueisLowerCase('a3@$'); // trueisLowerCase('Ab4'); // false
```

# **67、isNil**

用于判断当前变量的值是否为 null 或 undefined 类型。

```
const isNil = val => val === undefined || val === null;
isNil(null); // trueisNil(undefined); // true
```

# **68、isNull**

用于判断当前变量的值是否为 null 类型。

```
const isNull = val => val === null;
isNull(null); // true
```

# **69、isNumber**

用于检查当前的值是否为数字类型。

```
function isNumber(n) {    return !isNaN(parseFloat(n)) && isFinite(n);}
isNumber('1'); // falseisNumber(1); // true
```

# **70、isObject**

用于判断参数的值是否是对象，这里运用了Object 构造函数创建一个对象包装器，如果是对象类型，将会原值返回。

```
const isObject = obj => obj === Object(obj);
isObject([1, 2, 3, 4]); // trueisObject([]); // trueisObject(['Hello!']); // trueisObject({ a: 1 }); // trueisObject({}); // trueisObject(true); // false
```

# **71、isObjectLike**

用于检查参数的值是否为null以及类型是否为对象。

```
const isObjectLike = val => val !== null && typeof val === 'object';
isObjectLike({}); // trueisObjectLike([1, 2, 3]); // trueisObjectLike(x => x); // falseisObjectLike(null); // false
```

# **72、isPlainObject**

此代码段检查参数的值是否是由Object构造函数创建的对象。

```
const isPlainObject = val => !!val && typeof val === 'object' && val.constructor === Object;
isPlainObject({ a: 1 }); // trueisPlainObject(new Map()); // false
```

# **73、isPromiseLike**

用于检查当前的对象是否类似Promise函数。

```
const isPromiseLike = obj =>  obj !== null &&  (typeof obj === 'object' || typeof obj === 'function') &&  typeof obj.then === 'function';  isPromiseLike({  then: function() {    return '';  }}); // trueisPromiseLike(null); // falseisPromiseLike({}); // false
```

# **74、isSameDate**

用于判断给定的两个日期是否是同一天。

```
const isSameDate = (dateA, dateB) => dateA.toISOString() === dateB.toISOString();
isSameDate(new Date(2010, 10, 20), new Date(2010, 10, 20)); // true
```

# **75、isString**

用于检查当前的值是否为字符串类型。

```
const isString = val => typeof val === 'string';
isString('10'); // true
```

# **76、isSymbol**

用于判断参数的值是否是 Symbol 类型。

```
const isSymbol = val => typeof val === 'symbol';
isSymbol(Symbol('x')); // true
```

# **77、isUndefined**

用于判断参数的类型是否是 Undefined 类型。

```
const isUndefined = val => val === undefined;
isUndefined(undefined); // true
```

# **78、isUpperCase**

用于判断当前字符串的字母是否都为大写。

```
const isUpperCase = str => str === str.toUpperCase();
isUpperCase('ABC'); // trueisLowerCase('A3@$'); // trueisLowerCase('aB4'); // false
```

# **79、isValidJSON**

用于判断给定的字符串是否是 JSON 字符串。

```
const isValidJSON = str => {  try {    JSON.parse(str);    return true;  } catch (e) {    return false;  }};
isValidJSON('{"name":"Adam","age":20}'); // trueisValidJSON('{"name":"Adam",age:"20"}'); // falseisValidJSON(null); // true
```

# **80、last**

此函数功能返回数组的最后一个元素。

```
const last = arr => arr[arr.length - 1];
last([1, 2, 3]); // 3
```

# **81、matches**

此函数功能用于比较两个对象，以确定第一个对象是否包含与第二个对象相同的属性与值。

```
onst matches = (obj, source) =>  Object.keys(source).every(key => obj.hasOwnProperty(key) && obj[key] === source[key]);  matches({ age: 25, hair: 'long', beard: true }, { hair: 'long', beard: true }); // truematches({ hair: 'long', beard: true }, { age: 25, hair: 'long', beard: true }); // false
```

# **82、maxDate**

此代码段查找日期数组中最大的日期进行输出。

```
const maxDate = (...dates) => new Date(Math.max.apply(null, ...dates));
const array = [  new Date(2017, 4, 13),  new Date(2018, 2, 12),  new Date(2016, 0, 10),  new Date(2016, 0, 9)];maxDate(array); // 2018-03-11T22:00:00.000Z
```

# **83、maxN**

此段代码输出数组中前 n 位最大的数。

```
const maxN = (arr, n = 1) => [...arr].sort((a, b) => b - a).slice(0, n);
maxN([1, 2, 3]); // [3]maxN([1, 2, 3], 2); // [3,2]
```

# **84、minDate**

此代码段查找日期数组中最早的日期进行输出。

```
const minDate = (...dates) => new Date(Math.min.apply(null, ...dates));
const array = [  new Date(2017, 4, 13),  new Date(2018, 2, 12),  new Date(2016, 0, 10),  new Date(2016, 0, 9)];minDate(array); // 2016-01-08T22:00:00.000Z
```

# **85、minN**

此代码段返回n列表中的最小元素。如果n大于或等于列表的长度，则返回原始列表（按升序排序）。

```
const minN = (arr, n = 1) => [...arr].sort((a, b) => a - b).slice(0, n);
minN([1, 2, 3]); // [1]
minN([1, 2, 3], 2); // [1,2]
```

# **86、negate** 

此代码段可用于将 not 运算符 ( !) 应用于带有参数的谓词函数。

```
const negate = func => (...args) => !func(...args);
[1, 2, 3, 4, 5, 6].filter(negate(n => n % 2 === 0)); // [ 1, 3, 5 ]
```

# **87、 nodeListToArray**

此代码段可用于将 nodeList 转换为数组。

```
const nodeListToArray = nodeList => [...nodeList];
nodeListToArray(document.childNodes); // [ <!DOCTYPE html>, html ]
```

**88、pad**

如果字符串短于指定长度，则此代码段可用于在字符串的两侧填充指定字符。

```
const pad = (str, length, char = ' ') =>  str.padStart((str.length + length) / 2, char).padEnd(length, char);
pad('cat', 8); // '  cat   'pad(String(42), 6, '0'); // '004200'pad('foobar', 3); // 'foobar'
```

# **89、 radsToDegrees**

此代码段可用于将角度从弧度转换为度数。

```
const radsToDegrees = rad => (rad * 180.0) / Math.PI;
radsToDegrees(Math.PI / 2); // 90
```

# **90、随机生成十六进制颜色代码**

此代码段可用于生成随机的十六进制颜色代码。

```
const randomHexColorCode = () => {  let n = (Math.random() * 0xfffff * 1000000).toString(16);  return '#' + n.slice(0, 6);};
randomHexColorCode(); // "#e34155"
```

# **91、 randomIntArrayInRange**

此代码段可用于生成具有n指定范围内的随机整数的数组。

```
const randomIntArrayInRange = (min, max, n = 1) =>  Array.from({ length: n }, () => Math.floor(Math.random() * (max - min + 1)) + min);
randomIntArrayInRange(12, 35, 10); // [ 34, 14, 27, 17, 30, 27, 20, 26, 21, 14 ]
```

# **92、randomIntegerInRange**

此代码段可用于生成指定范围内的随机整数。

```
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
randomIntegerInRange(0, 5); // 3
```

# **93、RandomNumberInRange**

此代码段可用于返回指定范围内的随机数。

```
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;
randomNumberInRange(2, 10); // 6.0211363285087005
```

# **94、ReadFileLines**

此代码段可用于通过从文件中获取行数组来读取文件。

```js
const fs = require('fs');
const readFileLines = filename =>  
	fs    
        .readFileSync(filename)   
        .toString('UTF8')   
        .split('\n');
let arr = readFileLines('test.txt');console.log(arr); // ['line1', 'line2', 'line3']
```

# **95、 重定向到一个 URL**

此代码段可用于重定向到指定的 URL。

```
const redirect = (url, asLink = true) =>  asLink ? (window.location.href = url) : window.location.replace(url);
redirect('https://google.com');
```

# **96、反转字符串**

此代码段可用于反转字符串。

```
const reverseString = str => [...str].reverse().join('');
reverseString('foobar'); // 'raboof'
```

# **97、round**

此代码段可用于将数字四舍五入到指定的位数。

```
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
round(1.005, 2); // 1.01
```

# **98、runPromisesInSeries**

此代码段可用于连续运行一系列 Promise。

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
const delay = d => new Promise(r => setTimeout(r, d));
runPromisesInSeries([() => delay(1000), () => delay(2000)]); // Executes each promise sequentially, taking a total of 3 seconds to complete
```

# **99、sample**

此代码段可用于从数组中获取随机数。

```
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
sample([3, 7, 9, 11]); // 9
```

# **100、sampleSize**

此代码段可用于从数组中的唯一位置获取 n 个随机元素，直至数组的大小。 使用 Fisher-Yates 算法对数组中的元素进行混洗。

```
const sampleSize = ([...arr], n = 1) => {  let m = arr.length;  while (m) {    const i = Math.floor(Math.random() * m--);    [arr[m], arr[i]] = [arr[i], arr[m]];  }  return arr.slice(0, n);};
sampleSize([1, 2, 3], 2); // [3,1]sampleSize([1, 2, 3], 4); // [2,3,1]
```

# **101、scrollToTop**

此代码段可用于平滑滚动到当前页面的顶部。

```js
const scrollToTop = () => {  
    const c = document.documentElement.scrollTop || document.body.scrollTop;  
    if (c > 0) {    
        window.requestAnimationFrame(scrollToTop);
        window.scrollTo(0, c - c / 8);  
    }
};
scrollToTop();
```

# **102、serializeCookie**

此代码段可用于将 cookie 名称-值对序列化为 Set-Cookie 标头字符串。

```
const serializeCookie = (name, val) => `${encodeURIComponent(name)}=${encodeURIComponent(val)}`;
serializeCookie('foo', 'bar'); // 'foo=bar'
```

# **103、setStyle**

此代码段可用于为特定元素设置 CSS 规则的值。

```
const setStyle = (el, ruleName, val) => (el.style[ruleName] = val);
setStyle(document.querySelector('p'), 'font-size', '20px');// The first <p> element on the page will have a font-size of 20px
```

# **104、shallowClone**

此代码段可用于创建对象的浅层克隆。

```js
const shallowClone = obj => Object.assign({}, obj);
const a = { x: true, y: 1 };
const b = shallowClone(a); // a !== b
```

# **105、Show**

此代码段可用于显示指定的所有元素。

```
const show = (...el) => [...el].forEach(e => (e.style.display = ''));
show(...document.querySelectorAll('img')); // Shows all <img> elements on the page
```

# **106、shuffle**

此代码段可用于使用Fisher-Yates 算法随机排序数组的元素。

```js
const shuffle = ([...arr]) => {  
    let m = arr.length;  
    while (m) {    
        const i = Math.floor(Math.random() * m--);    
        [arr[m], arr[i]] = [arr[i], arr[m]]; 
    }  
    return arr;
};
const foo = [1, 2, 3];
shuffle(foo); // [2, 3, 1], foo = [1, 2, 3]
```

# **107、similarity**

此代码段可用于返回出现在两个数组中的元素数组。

```
const similarity = (arr, values) => arr.filter(v => values.includes(v));
similarity([1, 2, 3], [1, 2, 4]); // [1, 2]
```

# **108、sleep**

此代码段可用于通过将异步函数置于睡眠状态来延迟其执行。

```js
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));
async function sleepyWork() {  
    console.log("I'm going to sleep for 1 second.");  
    await sleep(1000);  
    console.log('I woke up after 1 second.');
}
```

# **109、smoothScroll**

此代码段可用于将调用它的元素平滑地滚动到浏览器窗口的可见区域。

```
const smoothScroll = element =>  document.querySelector(element).scrollIntoView({    behavior: 'smooth'  });
smoothScroll('#fooBar'); // scrolls smoothly to the element with the id fooBarsmoothScroll('.fooBar'); // scrolls smoothly to the first element with a class of fooBar
```

# **110、 sortCharactersInString**

此代码段可用于按字母顺序对字符串中的字符进行排序。

```
const sortCharactersInString = str => [...str].sort((a, b) => a.localeCompare(b)).join('');
sortCharactersInString('cabbage'); // 'aabbceg'
```

# **111、splitLines**

此代码段可用于将多行字符串拆分为行数组。

```
const splitLines = str => str.split(/\r?\n/);
splitLines('This\nis a\nmultiline\nstring.\n'); // ['This', 'is a', 'multiline', 'string.' , '']
```

# **112、stripHTMLTags**

此代码段可用于从字符串中删除 HTML/XML 标记。

```
const stripHTMLTags = str => str.replace(/<[^>]*>/g, '');
stripHTMLTags('<p><em>lorem</em> <strong>ipsum</strong></p>'); // 'lorem ipsum'
```

# **113、sum**

此代码段可用于查找两个或多个数字或数组的总和。

```
const sum = (...arr) => [...arr].reduce((acc, val) => acc + val, 0);
sum(1, 2, 3, 4); // 10sum(...[1, 2, 3, 4]); // 10
```

# **114、tail**

此代码段可用于获取包含数组中除第一个元素之外的所有元素的数组。如果数组只有一个元素，那么将返回具有该元素的数组。

```
const tail = arr => (arr.length > 1 ? arr.slice(1) : arr);
tail([1, 2, 3]); // [2,3]tail([1]); // [1]
```

# **115、take**

此代码段可用于获取从开头删除 n 个元素的数组。

```
const take = (arr, n = 1) => arr.slice(0, n);
take([1, 2, 3], 5); // [1, 2, 3]take([1, 2, 3], 0); // []
```

# **116、takeRight**

此代码段可用于获取n 从末尾删除元素的数组。

```
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);
takeRight([1, 2, 3], 2); // [ 2, 3 ]takeRight([1, 2, 3]); // [3]
```

# **117、timeTaken**

此代码段可用于找出执行函数所需的时间。

```
const timeTaken = callback => {  console.time('timeTaken');  const r = callback();  console.timeEnd('timeTaken');  return r;};
timeTaken(() => Math.pow(2, 10)); // 1024, (logged): timeTaken: 0.02099609375ms
```

# **118、times**

此代码段可用于迭代回调n 时间。

```
const times = (n, fn, context = undefined) => {  let i = 0;  while (fn.call(context, i) !== false && ++i < n) {}};
var output = '';times(5, i => (output += i));console.log(output); // 01234
```

# **119、 toCurrency**

此代码段可用于格式化数字（如货币）。

```
const toCurrency = (n, curr, LanguageFormat = undefined) =>  Intl.NumberFormat(LanguageFormat, { style: 'currency', currency: curr }).format(n);
toCurrency(123456.789, 'EUR'); // €123,456.79  | currency: Euro | currencyLangFormat: LocaltoCurrency(123456.789, 'USD', 'en-us'); // $123,456.79  | currency: US Dollar | currencyLangFormat: English (United States)toCurrency(123456.789, 'USD', 'fa'); // ۱۲۳٬۴۵۶٫۷۹ ؜$ | currency: US Dollar | currencyLangFormat: FarsitoCurrency(322342436423.2435, 'JPY'); // ¥322,342,436,423 | currency: Japanese Yen | currencyLangFormat: LocaltoCurrency(322342436423.2435, 'JPY', 'fi'); // 322 342 436 423 ¥ | currency: Japanese Yen | currencyLangFormat: Finnish
```

# **120、 toDecimalMark**

此代码段使用toLocaleString() 函数将浮点运算转换为小数点形式，方法是使用数字生成逗号分隔的字符串。

```
const toDecimalMark = num => num.toLocaleString('en-US');
toDecimalMark(12305030388.9087); // "12,305,030,388.909"
```

# **121、toggleClass**

此代码段可用于切换元素的类。

```
const toggleClass = (el, className) => el.classList.toggle(className);
toggleClass(document.querySelector('p.special'), 'special'); // The paragraph will not have the 'special' class anymore
```

# **122、tomorrow**

此代码段可用于获取明天日期的字符串表示。

```
const tomorrow = () => {  let t = new Date();  t.setDate(t.getDate() + 1);  return t.toISOString().split('T')[0];};
tomorrow(); // 2019-09-08 (if current date is 2018-09-08)
```

# **123、unfold**

此代码段可用于使用迭代器函数和初始种子值构建数组。

```
const unfold = (fn, seed) => {  let result = [],    val = [null, seed];  while ((val = fn(val[1]))) result.push(val[0]);  return result;};
var f = n => (n > 50 ? false : [-n, n + 10]);unfold(f, 10); // [-10, -20, -30, -40, -50]
```

# **124、union**

此代码段可用于查找两个数组的并集，从而生成一个包含来自两个数组但不重复的元素的数组。

```
const union = (a, b) => Array.from(new Set([...a, ...b]));
union([1, 2, 3], [4, 3, 2]); // [1,2,3,4]
```

# **125、uniqueElements**

这段代码使用 ES6 Set 和 ...rest 运算符来只获取每个元素一次。

```
const uniqueElements = arr => [...new Set(arr)];
uniqueElements([1, 2, 2, 3, 4, 4, 5]); // [1, 2, 3, 4, 5]
```

# **126、validateNumber**

此代码段可用于检查值是否为数字。

```
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n;
validateNumber('10'); // true
```

# **127、words**

此代码段将字符串转换为单词数组。

```
const words = (str, pattern = /[^a-zA-Z-]+/) => str.split(pattern).filter(Boolean);
words('I love javaScript!!'); // ["I", "love", "javaScript"]words('python, javaScript & coffee'); // ["python", "javaScript", "coffee"]
```
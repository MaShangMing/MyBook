![img](https://mmbiz.qpic.cn/mmbiz_png/hNaVUggh5iaribISG5IuFJicHXm5CnNrvgHCOtTcXpphf3MTicmvrUvtxoafKdiaWHLVZcskKPofhn6alCFd24L1ylw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 1.创建数组

### 1.1 ES5的方式

回忆下ES5中创建数组的方式：

调用Array的构造函数，即

```
new Array()
new Array(size)
new Array(element0, element1, ..., elementn);
```

用数组字面量语法，例如：

```
var arr1 = [1,2,3];
```

其中，调用Array的构造函数时，要注意下面这三点：

- (1)如果传入了一个数组型的值，则数组的长度`length`属性会被设为该值，而数组的元素都是`undefined`;
- (2)如果传入了一个非数值型的值，则该值会被设为数组中的唯一项；
- (3)如果传入了多个值，则都被设为数组元素；

验证(1)

传入了一个数组型的值：

```
var arr = new Array(3);
console.log(arr); // [empty × 3]
console.log(arr.length); // 3
console.log(arr[0]); // undefined
console.log(arr[1]); // undefined
console.log(arr[2]); // undefined
```

验证(2)

传入了一个非数值型的值：

```
var arr = new Array("3");
console.log(arr); // ["3"]
console.log(arr.length); // 1
console.log(arr[0]); // 3
```

验证(3)

传入了多个值：

```
var arr = new Array(3,"3");
console.log(arr); // [3, "3"]
console.log(arr.length); // 2
console.log(arr[0]); // 3
console.log(arr[1]); // 3
```

可以看出，使用new Array()创建数组时，要特别注意传入一个值时，这个值的类型。而如果想就传入一个数值，且这个值就是数组中的唯一一个元素时，只能用数组字面量语法了。

Luckily，ES6中创建数组的方法就不需要考虑这么多，下面介绍`Array.of()`和`Array.from()`

### 1.2 ES6的方式

#### 1.2.1Array.of()

针对上述问题，`Array.of()`就可以解决。不论传几个参数、是什么类型的参数，使用`Array.of()`会把所有传入的参数都被设为数组元素

验证(1)

传入了一个数组型的值：

```
let arr = Array.of(3);
console.log(arr); // [3]
console.log(arr.length); // 1
console.log(arr[0]); // 3
```

验证(2)

传入了一个非数值型的值：

```
let arr = Array.of("3");
console.log(arr); // ["3"]
console.log(arr.length); // 1
console.log(arr[0]); // 3
```

验证(3)

传入了多个值：

```
let arr = Array.of(3,"3");
console.log(arr); // [3, "3"]
console.log(arr.length); // 2
console.log(arr[0]); // 3
console.log(arr[1]); // 3
```

可以看出，使用`Array.of()`创建数组时，会把所有传入的参数都被设为数组元素。

#### 1.2.2 Array.from()

用途：可将类似数组的对象、可遍历的对象转为`真正的数组`。

要想把类似数组的对象转为数组，在ES5中的实现方法：

```
Array.prototype.slice.call(arrayLike);
let arrayLike = {
    '0': 'element0',
    '1': 'element1',
    '2': 'element2',
    length: 3
};
let arr = Array.prototype.slice.call(arrayLike); 
console.log(arr); // ["element0", "element1", "element2"]
```

可以说ES5的这种方法语义上不够清晰，在ES6中可以使用`Array.from()`方法实现：

验证：

```
let arrayLike = {
    0: 'element0',
    1: 'element1',
    2: 'element2',
    length: 3
};
let arr = Array.from(arrayLike); 
console.log(arr); // ["element0", "element1", "element2"]
```

Array.from()支持三个参数：

- 第一个参数是类数组对象或可遍历的对象；
- 第二个参数(可选)是一个函数，可以对一个参数中的对象中的每一个的值进行转换；
- 第三个参数(可选)是函数的this值。

其中，常见的类数组的对象是 ：DOM 操作返回的 `NodeList` 集合，以及函数内部的`arguments`对象。

所谓类似数组的对象，本质特征只有一点，即必须有`length`属性。因此，任何有length属性的对象，都可以通过Array.from方法转为数组。

可遍历的对象：含有`Symbol.iterator`属性的对象，如Set和Map

验证(1)

函数内的arguments对象，转换为数组：

```
function test(){
    let arr = Array.from(arguments);
    return arr;
}
let arr1 = test(1,2,3,4);
console.log(arr1); // [1, 2, 3, 4]
```

验证(2)

传入函数作为第二个参数，转换对象中的每个值

```
function test() {
    let arr = Array.from(arguments, val => val * 2);
    return arr;
}
let arr1 = test(1, 2, 3, 4);
console.log(arr1); // [2, 4, 6, 8]
```

验证(3)

可遍历对象转换为数组

```
let set = new Set(['a', 'b'])
console.log(Array.from(set)) // ['a', 'b']
```

## 2.查找元素

ES5中可以用`indexOf`、`lastIndexOf()`查找某个值是否出现在字符串中。

ES6中可以用`find()`、`findIndex()`在数组中查找匹配的元素。

其中，`find()`方法是返回查找到的第一个值，而`findIndex()`是返回查找到的第一个值的index，即索引位置。这两个方法都接受两个参数：

- 第一个参数是回调函数；
- 第二个参数(可选)是用于指定回调函数中的this值。

验证：

find()返回的是满足条件的第一个值，findIndex()返回的是满足条件的第一个值的索引。

```
let arr = [1, 2, 3, 4, 5]
console.log(arr.find(item => item > 3)) // 4
console.log(arr.findIndex(item => item > 3)) // 3
```

## 3.填充数组

### 3.1 fill()

fill()：用指定的值填充一个到多个数组元素。

其中，当只传入一个值时，会用这个值重写数组中的所有值。

该方法接受三个参数：

- 第一个参数是要填充的值；
- 第二个参数(可选) 表示填充的开始索引；
- 第三个参数(可选) 表示结束索引的前一个索引。

验证(1)

只传入一个值

```
let arr = [1, 2, 3, 4, 5];
console.log(arr.fill(6)); // [6, 6, 6, 6, 6]
```

验证(2)

如果第二个参数或第三个参数为负值，可将值+数组.length来计算位置

```
let arr = [1, 2, 3, 4, 5];
console.log(arr.fill(6, -4, -1)); // [1,6,6,6,5]
```

上面第二个参数和第三个参数为负值，实际计算后的值分别为：-4+5，-1+5，即1,4。那么相当于arr.fill(6,1,4);从索引1到索引4前一个位置，即从索引1到索引3，用数值6填充，结果为：`[1,6,6,6,5]`

类似的方法还有`copyWithin()`方法

### 3.2 copyWithin()

该方法也可接受三个参数：

- 第一个参数是开始粘贴值的索引位置
- 第二个参数(可选)是开始复制值的索引位置
- 第三个参数(可选)是停止复制值的位置（不包含当前位置）

注意：所有参数都可以是负值，处理方法和fill()一样，需加上arr.length来计算

验证(1)：

copyWithin()传入一个参数：

```
let arr = [1, 2, 3, 4, 5];
// 从索引位置2开始粘贴
// 1,2,3,4,5填充
console.log(arr.copyWithin(2)); // [1, 2, 1, 2, 3]
```

传入两个参数：

```
let arr = [1, 2, 3, 4, 5];
// 从索引位置2开始粘贴
// 从索引位置1开始复制 
// 2，3，4，5填充
// console.log(arr.copyWithin(2, 1)) // [1, 2, 2, 3, 4] 
```

传入三个参数：

```
let arr = [1, 2, 3, 4, 5];
// 从索引位置2开始粘贴
// 从索引位置1开始复制
// 到索引位置2之前结束复制，即到位置1
// 2填充
// console.log(arr.copyWithin(2, 1, 2)) // [1,2,2,4,5]
```

验证(2)：

参数传入负值

```
// 从索引位置-3+5 =2开始复制
// 从索引位置 -1+5=4之前结束复制，即到位置3
// 3,4填充
// 从索引位置2开始粘贴
console.log(arr.copyWithin(2,-3,-1)) // [1,2,3,4,5]
```

## 4.小结

本文主要总结了ES6中数组部分的扩展。包括两个创建数组的新方法：`Array.of()`、`Array.from()。`两个在数组中根据条件来查找匹配的元素的方法：`find()`、`findIndex()`。还有两个填充数组的方法：`fill()`、`copyWithin()`。如有问题，欢迎指正。

最近几天会陆续更新的~~，觉得总结的可以的话，麻烦给小编点一个  `在看`, 谢谢！
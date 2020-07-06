## 1.ES6对象字面量

### 1.1简化对象属性定义

**验证(1):**

简化属性定义：

```js
// ES5
function test(name, age) {
    return {
        name: name,
        age: age
    }
}
// 等价于
function test(name, age) {
    return {
        name,
        age
    }
}
```

当一个对象的属性和本地变量同名时，可以简单地中写属性名。

### 1.2 对象方法简写

**验证(2)：**

对象方法可以简写，去掉冒号和function关键字：

```js
// ES5
var person = {
    name: "Peter",
    age: 26,
    showAge: function () {
        console.log('age is', this.age)
    }
}
// 等价于
var person = {
    name: "Peter",
    age: 26,
    showAge() {
        console.log('age is', this.age)
    }
}
```

### 1.3同一个对象定义多个同名属性不报错

**验证(3):**

同一个对象定义多个同名属性不报错

```js
var person = {
    name:'Peter',
    name:'Tom'
}
console.log(person.name) // Tom
```

ES5在严格模式下会去校验是否有同名属性，ES6则无论在严格模型下，还是非严格模式下，都不会去校验属性是否重复。

## 2. Object.is() 和Object.assing()

### 2.1 Object.is()

有些像“===”运算符，可接受两个参数进行比较。如果两个参数的类型一致，并且值也相同，则返回true。

验证：

```js
console.log(Object.is(1,"1")); // false 
Object.is()和===运算符的区别：

console.log(Object.is(+0, -0)); // false
console.log(+0 === -0); // true

console.log(Object.is(NaN, NaN)) // true
console.log(NaN === NaN) // false
```

### 2.2 Object.assign(target,source1,source2,...)

返回第一个接收对象，可以接受任意个源对象，如果多个源对象有相同的属性，则后面的会覆盖前面的。

验证(1):

```js
var target = {};
Object.assign(target, {
    name: 'tony',
    age: '24'
})
console.log(target) // {name: "tony", age: "24"}    
```

**验证(2):**

如果后面的多个源对象source1,source2有同名的属性，则后面的源对象会覆盖前面的

```js
var target = {};
Object.assign(target, {
    name: 'tony',
    age: '24'
}, {
        age: '28'
    })
console.log(target) // {name: "tony", age: "28"}
```

**验证(3):**

```js
  var target = {};
  function source() { }
  source.prototype = {
      constructor: source,
      hello: function () { console.log('hello~~') }
  }
  Object.assign(target, source.prototype)
  target.hello(); //hello~~   
```

**验证(4):**

忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

```js
var target = {};
var source1 = {
    age: '24'
}
function Person(name) {
    this.name = name;
}
Person.prototype.hello = function () {
    console.log(this.name)
}
var source2 = new Person("tony");
// 使用Object.defineProperty()为source2对象定义一个名为hobby的属性，且设定为可枚举的
Object.defineProperty(source2, "hobby", {
    enumerable: true,
    value: 'reading'
})

Object.assign(target, source1, source2)
console.log(target) // {age: "24", name: "tony", hobby: "reading"} 
```

**去掉上述enumerable属性（默认为false），再看下结果：**

```js
var target = {};
var source1 = {
    age: '24'
}
function Person(name) {
    this.name = name;
}
Person.prototype.hello = function () {
    console.log(this.name)
}
var source2 = new Person("tony");
// 使用Object.defineProperty()为source2对象定义一个名为hobby的属性，且设定为不可枚举的
Object.defineProperty(source2, "hobby", {
    value: 'reading'
})

Object.assign(target, source1, source2)
console.log(target) // {age: "24", name: "tony"}    
```

可以看出Object.assign()会忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

**【补充】有四个操作会忽略enumerable为false的属性，即不可枚举的属性：**

- for...in循环：只遍历对象自身的和继承的可枚举的属性。
- Object.keys()：返回对象自身的所有可枚举的属性的键名。
- JSON.stringify()：只串行化对象自身的可枚举的属性。
- Object.assign()：只拷贝对象自身的可枚举的属性。

## 3.定义了自身属性枚举顺序

**自有属性枚举顺序的基本规则：**

- 1，所有数字键按升序排序；
- 2，所有字符串键按它们被加入对象的顺序排序；
- 3，所有symbol键按照它们被加入对象的顺序排序；

**验证(1)：**

可以用Object.getOwnPropertyNames(obj)方法查看对象自身的所有属性（不含Symbol属性，包含不可枚举属性）的键名。

```js
var obj = {
    2: 1,
    name: 'tony',
    0: 1,
    age: '24',
    hobby: 'reading',
    1: 1
}
console.log(Object.getOwnPropertyNames(obj)) // ["0", "1", "2", "name", "age", "hobby"]
```

可以看出，字符串键是跟在数值键之后，数值键按升序排序，字符串键按加入对象的顺序排序。

【补充】：

ES6 一共有 5 种方法可以遍历对象的属性。

- for...in

for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

- Object.keys(obj)

Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

- Object.getOwnPropertyNames(obj)

Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

- Object.getOwnPropertySymbols(obj)

Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。

- Reflect.ownKeys(obj)

Reflect.ownKeys返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

## 4.创建对象后修改对象原型：Object.setPrototypeOf()

Object.setPrototypeOf()方法的作用：改变任意指定对象的原型，接受两个参数：

-- 被改变原型的对象

-- 替代第一个参数原型的对象

**验证(1):**

```js
var dog = {
    hello() {
        console.log('a dog barks')
    }
}
var person = {
    hello() {
        console.log('say hello')
    }
}

// 以person为原型
var person1 = Object.create(person);
person1.hello(); // say hello
console.log(person.isPrototypeOf(person1)) // true

// 将person1的原型设置为dog
Object.setPrototypeOf(person1, dog)
person1.hello(); // a dog barks
console.log(person.isPrototypeOf(person1)) // false
console.log(dog.isPrototypeOf(person1)) // true
```

说明：person1的原型原本是person，通过Object.setPrototypeOf(person1,dog)后，把person1的原型设置为了dog。

## 5.super关键字

ES5中，`this`关键字总是指向函数所在的当前对象。

ES6 中的关键字`super`，指向当前对象的原型对象。

**验证(1)：**

可以用`super`更方便地访问对象的原型，来引用对象原型上所有的方法。

ES5:

```js
  var dog = {
      hello() {
          return 'a dog barks'
      }
  }
  var person = {
      hello() {
          return 'say hello'
      }
  }
  var friend = {
      hello() {
          let msg = Object.getPrototypeOf(this).hello.call(this)
          console.log(msg);
      }
  }
  Object.setPrototypeOf(friend, dog);
  friend.hello(); // a dog barks
  Object.setPrototypeOf(friend, person);
  friend.hello(); // say hello
```

Object.getPrototypeOf(this)就是指向对象的原型，ES6中可以用super替换：

```js
Object.getPrototypeOf(this).hello.call(this)

// 等价于
super.hello()
var dog = {
    hello() {
        return 'a dog barks'
    }
}
var person = {
    hello() {
        return 'say hello'
    }
}
var friend = {
    hello() {
        let msg = super.hello()
        console.log(msg);
    }
}
Object.setPrototypeOf(friend, dog);
friend.hello(); // a dog barks
Object.setPrototypeOf(friend, person);
friend.hello(); // say hello
```

从结果可以看出效果是一样的。

**验证(2)**

必须要在简写方法的对象中使用super，其他地方声明中使用则会报语法错误。

```
Uncaught SyntaxError: 'super' keyword unexpected here
```

还是上面的示例：

```js
 var dog = {
        hello() {
            return 'a dog barks'
        }
    }
    var person = {
        hello() {
            return 'say hello'
        }
    }
    var friend = {
        hello: function () {
            let msg = super.hello()
            console.log(msg);
        }
    }
    Object.setPrototypeOf(friend, dog);
    friend.hello();
    Object.setPrototypeOf(friend, person);
    friend.hello(); // Uncaught SyntaxError: 'super' keyword unexpected here
```

## 6.小结

本节内容主要总结了ES6中对象的一些扩展。包括对象字面量上的变更、`Object.is()`（注意下和===的区别）、`Object.assign()`方法，对象自身属性的枚举属性的顺序、`Object.setPrototypeOf()`方法可以在创建对象后改变它的原型，以及可以通过super关键字调用对象原型的方法。
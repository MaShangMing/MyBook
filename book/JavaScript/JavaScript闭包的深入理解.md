闭包算是javascript中一个比较难理解的概念，想要深入理解闭包的原理，首先需要搞清楚其他几个概念：

## **一、栈内存和堆内存**

学过C/C++的同学可能知道，计算机系统将内存分为栈和堆两部分（大学的基础课，忘掉的赶紧重新捡起来）。

栈内存（连续的存储空间，类似数据结构中的栈）：主要用来存放数值、字符、内存地址等小数据。

堆内存（散列的存储空间，类似数据结构中的链表）：存放可以动态变化的大数据。

## **二、基本类型和引用类型**

JavaScript将变量分为两种类型：

基本类型：Number、String、Boolean 、undefined、null（值被保存在栈内存中）

引用类型：Object、Array、function（具体内容被保存在堆内存中，在栈内存中仅保存堆内存的地址）

![img](https://mmbiz.qpic.cn/mmbiz_jpg/eXCSRjyNYcafLkVRUoEG78s3oHNpAnugOgAVpHdNjrrr1fUXnLw6tXmXIiaIbWibwr1iaVeUcibof4tLAgdKA3FUug/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如上图，当在程序中在执行中有如下情况：

1、声明变量a为基本类型时，直接在栈内存中保存它的值为100；

2、当将a赋值给b时，b在栈内存中新建空间，将a的值复制过来

（注：之后a和b就没有关系了，再改变a或b的值，不影响另外一个，它们是独立的）

3、声明变量p1为引用类型时，将p1的内容保存在堆内存中，并将堆内存的物理地址保存在栈内存中

4、当将p1赋值给p2时，p2在栈内存中新建空间，仅复制堆内存的物理地址

（注：p1和p2中都保存的是指向堆内存的地址，即指的是同一个对象，当修改p1对象的属性后，p2对象的属性同时被修改）。

另外，在计算机语言中还有一些很重要的特性：

1、修改基本类型的值，实际上是新建空间存一个新值，然后将变量名指向新的空间（旧值依然存在栈内存中，只是缺少变量名指向它）

2、删除引用类型，其实并不删除堆内存中的内容，仅删除了栈内存中的物理地址（对象的内容依然存在堆内存中，只是缺少了地址的指向）

![img](https://mmbiz.qpic.cn/mmbiz_jpg/eXCSRjyNYcafLkVRUoEG78s3oHNpAnug4xr3cDN8w7eZrEicicNzkRrefJjYVZdNM53N9uib8upsauVibFlwtvoiaOA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

（注：计算机关于内存的管理，跟我们正常想到的不一样，例如硬盘恢复就是利用这个原理，为删除的内容重新建立一个指向即可访问）

## 二、变量作用域

javascript中变量又分为全局变量和局部变量

全局变量：在全局环境中声明的变量

局部变量：在函数中声明的变量

当函数在执行时，会创建一个封闭的执行期上下文环境，函数内部声明的变量仅可在函数内部使用，外部无法访问，而全局变量则在任何地方都可以使用。

## 三、预编译

JavaScript的运行为三步：语法分析》预编译》解释执行

1、语法分析：通篇扫描js文件，检查是否有低级语法错误

2、预编译四部曲：（发生在解释执行的前一刻）
　　a、创建AO对象（执行期上下文对象，全局为GO）
　　b、将形参和变量声明作为AO对象的属性名，值为undefined
　　c、将实参值传递给形参，即赋值给AO对象对应属性名
　　d、将函数声明为AO对象的方法名，值为函数体

3、解释执行：解释一行，执行一行。

```js
function test(a){   
    var b=1; 
    function c(){}  
}
test(2);
/* 函数预编译四部曲（函数执行前一刻，不执行不会预编译），全局预编译同理 
* 1---testAO{} 
* 2---testAO
{a:undefined,b:undefined} 
* 3---testAO
{a:2,b:undefined} 
* 4---testAO
{a:2,b:1,c:function(){}} 
*/
```

## 四、作用域链

每个JavaScript函数都是一个对象，对象中有些属性可以访问（比如name），有些属性不可以访问（比如[[scope]]仅供js引擎使用）

[[scope]]用来存储了运行期上下文对象的集合（即作用域链），作用域链中除了自身创建的AO对象外，还包括了所有父级运行期上下文对象（AO）

```js
function a(){  
	function b(){    
        var b = 234;     
    }  
    var a = 123;  
    b();
}
var glob = 100;
a();
```



![img](https://mmbiz.qpic.cn/mmbiz_jpg/eXCSRjyNYcafLkVRUoEG78s3oHNpAnug7F7aj9jQwDQQrTY4xW8Cm65TH5q4pJqxHapaOAHZ3xaF4seM0CxxXA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_jpg/eXCSRjyNYcafLkVRUoEG78s3oHNpAnugSXBuSp1nzo3XZpzprop8uUbcPUNCOsRrpwvvqcCialuZhSLYtHcDUYg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当b执行完成后，b的AO要被销毁，即b的[[scope]]第0位将被置空，如果再次执行b，将新建一个新的AO将其地址存到第0位，

当a也执行完成后，a的AO要被销毁，即a的[[scope]]第0位将被置空，同时a的AO中存着b，b也将被一同销毁

 

在了解如上这些概念后，我们再来看下面这个经典的闭包，你会有一个全新的认识

```js
function a(){  
    var b=123;
    function c(){
        console.log(b+=1);  
    }  
    return c;
}
var d=a();
d();
```

当这段代码在执行时的顺序如下：

1、预编译全局，生成执行上下文对象GO{d:undefined,a:function(){}}

2、定义a函数，将a函数的[[scope]]属性设置为{0:GO}

3、预编译a函数，生成a的执行上下文对象aAO{b:undefined,c:function(){}}，修改a函数的[[scope]]属性为{0:aAO,1:GO}

4、执行a函数，给aAO的属性赋值{b:123,c:function(){}}

5、定义c函数，将c函数的[[scope]]属性设置为{0:aAO,1:GO}，并将c返回给d

6、a函数执行完毕，销毁[[scope]]属性第0位对aAO对象的引用

7、执行d函数（等于执行c函数）之前，先预编译生成c的执行上下文对象cAO{}，修改c函数的[[scope]]属性为{0:cAO,1:aAO,2:GO}

8、执行c函数，b变量在cAO中没有，到[[scope]]属性中的下一位aAO中获取.
**1、JavaScript有哪些垃圾回收机制？**

有以下垃圾回收机制。

**标记清除（ mark and sweep）**

这是 JavaScript最常见的垃圾回收方式。当变量进入执行环境的时候，比如在函数中声明一个变量，垃圾回收器将其标记为“进入环境”。当变量离开环境的时候（函数执行结束），将其标记为“离开环境”。

垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量，以及被环境中变量所引用的变量（闭包）的标记。在完成这些之后仍然存在的标记就是要删除的变量。

**引用计数（ reference counting）**

在低版本的E中经常会发生内存泄漏，很多时候就是因为它采用引用计数的方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数。

当声明了一个变量并将个引用类型赋值给该变量的时候，这个值的引用次数就加1.如果该变量的值变成了另外一个，则这个值的引用次数减1.当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没法被访问。

因此，可以将它占用的空间回收，这样垃圾回收器会在运行的时候清理引用次数为0的值占用的空间在正中虽然 JavaScript对象通过标记清除的方式进行垃圾回收，但是BOM与DOM对象是用引用计数的方式回收垃圾的。

也就是说，只要涉及BOM和DOM，就会出现循环引用问题

**2、列举几种类型的DOM节点**

有以下几类DOM节点。

整个文档是一个文档（ Document）节点。

每个HTML标签是一个元素（ Element）节点。

每一个HTML属性是一个属性（ Attribute）节点。

包含在HTML元素中的文本是文本（Text）节点。

**3、谈谈 script标签中 defer和 async属性的区别。**

区别如下。

（1） defer属性规定是否延迟执行脚本，直到页面加载为止， async属性规定脚本一旦可用，就异步执行。

（2） defer并行加载 JavaScript文件，会按照页面上 script标签的顺序执行， async并行加载 JavaScript文件，下载完成立即执行，不会按照页面上 script标签的顺序执行。

**4、说说你对闭包的理解。**

使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染；缺点是闭包会常驻内存，增加内存使用量，使用不当很容易造成内存泄漏。在JavaScript中，函数即闭包，只有函数才会产生作用域闭包有3个特性

（1）函数嵌套函数。

（2）在函数内部可以引用外部的参数和变量

（3）参数和变量不会以垃圾回收机制回收

**5、解释一下 unshift0方法。**

该方法在数组启动时起作用，与 push()不同。它将参数成员添加到数组的顶部下面给出一段示例代。

- 
- 
- 
- 

```
var name=["john"] name. unshift（"charlie"）；name.unshift（"joseph"，"Jane"）； console. log（name）；
```

输出如下所示。

- 

```
[" joseph "， Jane "，" charlie "，" john "]
```

**6、encodeR0和 decodeR0的作用是什么？**

encodeURI()用于将URL转换为十六进制编码。而 decodeURI()用于将编码的URL转换回正常URL。

**7、为什么不建议在 JavaScript中使用 innerHTML？**

通过 innerHTML修改内容，每次都会刷新，因此很慢。在 innerHTML中没有验证的机会，因此更容易在文档中插入错误代码，使网页不稳定。

**8、如何在不支持 JavaScript的旧浏览器中隐藏 JavaScript代码？**

在< script>标签之后的代码中添加“<！--”，不带引号。

在< /script>标签之前添加“//-->”，代码中没有引号。

旧浏览器现在将 JavaScript代码视为一个长的HTML注释，而支持 JavaScript的浏览器则将"<！-"和"//-->"作为一行注释。

**9、在DOM操作中怎样创建、添加、移除、替换、插入和查找节点？**

具体方法如下。

（1）通过以下代码创建新节点。

- 
- 
- 
- 
- 
- 

```
createDocument Fragment ()//创建一个D0M片段createElement ()//创建一个具体的元素createTextNode ()//创建一个文本节点
```

（2）通过以下代码添加、移除、替换、插入节点

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
appendchild()removechild()eplacechild ()insertBefore ()//并没有 insertAfter（）（3）通过以下代码查找节点。getElementsByTagName ()//通过标签名称查找节点getElementsByName ()//通过元素的name属性的值查找节点（IE容错能力较强，会得到一个数//组，其中包括id等于name值的节点）getElementById（//通过元素Id查找节点，具有唯一性
```

**10、如何实现浏览器内多个标签页之间的通信？**

调用 localstorge、 cookie等数据存储通信方式

**11、null和 undefined的区别是什么？**

null是一个表示“无”的对象，转为数值时为0；undefined是一个表示“无”的原始值，转为数值时为NaN。

当声明的变量还未初始化时，变量的默认值为 undefined 。

null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。

undefined表示“缺少值”，即此处应该有一个值，但是还没有定义，典型用法是如下。

（1）如果变量声明了，但没有赋值，它就等于 undefined

（2）当调用函数时，如果没有提供应该提供的参数，该参数就等于 undefined。

（3）如果对象没有赋值，该属性的值为 undefined。

（4）当函数没有返回值时，默认返回 undefined。

 null表示“没有对象”，即此处不应该有值，典型用法是如下。

（1）作为函数的参数，表示该函数的参数不是对象。

（2）作为对象原型链的终点。

**12、new操作符的作用是什么？**

作用如下：

（1）创建一个空对象。

（2）由this变量引用该对象

（3）该对象继承该函数的原型（更改原型链的指向）

（4）把属性和方法加入到this引用的对象中。

（5）新创建的对象由this引用，最后隐式地返回this，过程如下：

- 
- 
- 

```
var obj ={}；obj._ _ proto_ _ Base .prototype； Base .call（obj);
```

**13、JavaScript延迟加载的方式有哪些？**

包括 defer和 async、动态创建DOM（创建 script，插入DOM中，加载完毕后回调、按需异步载入 JavaScript。

**14、call()和apply()的区别和作用是什么？**

作用都是在函数执行的时候，动态改变函数的运行环境（执行上下文）。

call和 apply的第一个参数都是改变运行环境的对象。

区别如下。

call从第二个参数开始，每一个参数会依次传递给调用函数；apply的第二个参数是数组，数组的每一个成员会依次传递给调用函数。

如

- 

```
func, call（funcl, varl, var2， var3）
```

对应的 apply写法为：

- 

```
func. apply （funcl, [varl, var2， var3]）
```

**15、哪些操作会造成内存泄漏？**

内存泄漏指不再拥有或需要任何对象（数据）之后，它们仍然存在于内存中。

提示：垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为0（没有其他对象引用过该对象），或对该对象的唯一引用是循环的，那么该对象占用的内存立即被回收。

如果 setTimeout的第一个参数使用字符串而非函数，会引发内存泄漏闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）等会造内存泄漏。

**16、列举E与 firefox的不同之处。**

不同之处如下

（1）IE支持 currentStyle；Firefox使用 get ComputStyle。

（2）IE使用 inner Text；Firefox使用 textContent。

（3）在透明度滤镜方面，正使用 filter:alpha（ opacity=num）；Firefox使用-moz- opacity :num

（4）在事件方面，IE使用 attachEvent:Firefox使用 add Event Listener

（5）对于鼠标位置：IE使用 event. clientX；Firefox使用 event. pageX。

（6）IE使用 event. srcElement；Firefox使用 event. target

（7）要消除list的原点，IE中仅须使 margin：0即可达到最终效果；Firefox中需要设置margin：0、 padding：0和 list-style:none

（8）CSS圆角：IE7以下不支持圆角。

**17、讲解一下 JavaScript对象的几种创建方式。**

有以下创建方式：

（1） Object构造函数式。

（2）对象字面量式。

（3）工厂模式。

（4）安全工厂模式。

（5）构造函数模式。

（6）原型模式。

（7）混合构造函数和原型模式。

（8）动态原型模式。

（9）寄生构造函数模式。

（10）稳妥构造函数模式。

**18、如何实现异步编程？**

具体方法如下：

方法1，通过回调函数。优点是简单、容易理解和部署；缺点是不利于代码的阅读和维护，各个部分之间高度耦合（ Coupling），流程混乱，而且每个任务只能指定一个回调函数。

方法2，通过事件监听，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以“去耦合”（ Decoupling），有利于实现模块化；缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。

方法3，采用发布/订阅方式。性质与“事件监听”类似，但是明显优于后者。

方法4，通过 Promise对象实现， Promise对象是 Commonjs工作组提出的一种规范，旨在为异步编程提供统一接口。它的思想是，每一个异步任务返回一个 Promise对象，该对象有一个then方法，允许指定回调函数。

**19、请解释一下 JavaScript的同源策略。**

同源策略是客户端脚本（尤其是 JavaScript）的重要安全度量标准。它最早出自Netscape Navigator2.0，目的是防止某个文档或脚本从多个不同源装载。

这里的同源策略指的是协议、域名、端口相同。同源策略是一种安全协议。指一段脚本只能读取来自同一来源的窗口和文档的属性。

**20、为什么要有同源限制？**

我们举例说明。比如一个黑客，他利用 Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名、密码登录时，他的页面就可以通过 Javascript读取到你表单上 Input中的内容，这样黑客就会轻松得到你的用户名和密码。

**21、在 JavaScript中，为什么说函数是第一类对象？**

第一类函数即 JavaScript中的函数。这通常意味着这些函数可以作为参数传递给其他函数，作为其他函数的值返回，分配给变量，也可以存储在数据结构中。

**22、什么是事件？E与 Firefox的事件机制有什么区别？如何阻止冒泡？**

事件是在网页中的某个操作（有的操作对应多个事件）例如，当单击一个按钮时，就会产生一个事件，它可以被 JavaScript侦测到，在事件处理机制上，正E支持事件冒泡；Firefox同时支持两种事件模型，也就是捕获型事件和冒泡型事件。

阻止方法是 ev.stop Propagation.注意旧版E中的方法 ev. cancelBubble=true.

**23、函数声明与函数表达式的区别？**

在 JavaScript中，在向执行环境中加载数据时，解析器对函数声明和函数表达式并非是一视同仁的。解析器会首先读取函数声明，并使它在执行任何代码之前可用（可以访问）。至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正解析和执行它。

**24、如何删除一个 cookie？**

为了删除 cookie，要修改 expires，代码如下。

document. cookie ='user=icketang；expires ='+ new Date（0）

**25、编写一个方法，求一个字符串的长度（单位是字节）**

假设一个英文字符占用一字节，一个中文字符占用两字节：

- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function GetBytes（str）{ var len=str .length； var bytes= len； for（var i-0：i<len；1++）{if （str. charcodeAt （i）>255） bytes++；}return bytes；}alert（ GetBytes（"hello 有课前端网！"））；
```

**26、对于元素， attribute和 property的区别是什么？**

attribute是DOM元素在文档中作为HTML标签拥有的属性；property就是DOM元素在 JavaScript中作为对象拥有的属性。

对于HTML的标准属性来说， attribute和 property是同步的，会自动更新，但是对于自定义的属性来说，它们是不同步的。

**27、解释延迟脚本在 JavaScript中的作用。**

默认情况下，在页面加载期间，HTML代码的解析将暂停，直到脚本停止执行。

这意味着，如果服务器速度较慢或者脚本特别“沉重”，则会导致网页延迟。在使用Deferred时，脚本会延迟执行，直到HTML解析器运行。这缩短了网页的加载时间，并且它们的显示速度更快。

**28、什么是闭包（ closure）？**

为了说明闭包，创建一个闭包。

- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function hello（）{//函数执行完毕，变量仍然存在var num= 100；var showResult= function（）{ alert （num）；} num++；return showResult ；}var showResult= he1lo();showResult（）//执行结果：弹出101
```

执行 hello()后， hello（）闭包内部的变量会存在，而闭包内部函数的内部变量不会存在，使得 JavaScript的垃圾回收机制不会收回hello()占用的资源，因为hell()中内部函数的执行需要依赖 hello()中的变量。

**29、如何判断一个对象是否属于某个类？**

使用 instanceof关键字，判断一个对象是否是类的实例化对象；使用 constructor属性，判断一个对象是否是类的构造函数。

**30、JavaScript中如何使用事件处理程序？**

事件是由用户与页面的交互（例如单击链接或填写表单）导致的操作。需要个事件处理程序来保证所有事件的正确执行。事件处理程序是对象的额外属性。此属性包括事件的名称和事件发生时采取的操作。

**31、在 JavaScript中有一个函数，执行直接对象查找时，它始终不会查找原型，这个函数是什么？**

hasOwnProperty。

**32、在 JavaScript中如何使用DOM？**

DOM代表文档对象模型，并且负责文档中各种对象的相互交互。DOM是开发网页所必需的，其中包括诸如段落、链接等对象。可以操作这些对象，如添加或删除等。为此，DOM还需要向网页添加额外的功能。

**33、documen.wrte和 innerHTML的区别是什么？**

document.wite重绘整个页面；innerHTML可以重绘页面的一部分。

**34、在 JavaScript中读取文件的方法是什么？**

可以通过如下方式读取服务器中的文件内容。

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function readAjaxEile（url） {//创建xhr var xhr =new XMLHttpRequest（）；/监听状态xhr. onreadystatechange=function（）{//监听状态值是4if（xhr. readystate == 4 && xhr. status = = =200）{console. log（xhr. responseText）}//打开请求xhr.open（'GET'， url, true）//发送数据xhr, send（null）}
```

可以通过如下方式读取本地计算机中的内容。

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
function readInputFile（id） { var file= document. getElementById（id). files[0]；//实例化 FileReader var reader=new FileReader（）；//读取文件reader. readAsText （file）//监听返回reader, onload= function （data） { console. log （data, this .result）}}
```

**35、如何分配对象属性？**

将属性分配给对象的方式与赋值给变量的方式相同。例如，表单对象的操作值以下列方式分配给" submit"：document.form. action=" submit'"

**36、请说几条书写 JavaScript语句的基本规范。**

基本规范如下：

（1）不要在同一行声明多个变量。

（2）应使用==/！==来比较true/ false或者数值。

（3）使用对象字面量替代 new Array这种形式。

（4）不要使用全局函数。

（5） switch语句必须带有 default分支。

（6）函数不应该有时有返回值，有时没有返回值。

（7）for循环必须使用大括号括起来。

（8）if语句必须使用大括号括起来。

9）for-in循环中的变量应该使用war关键字明确限定的作用域，从而避免作用域污染。

**37、eva的功能是什么？**

它的功能是把对应的字符串解析成 Javascript代码并运行应该避免使用eval，它会造成程序不安全，非常影响性能（执行两次，一次解析成JavaScript语句，一次执行）

**38、["1，"2，"3"].map（ parselnt）的执行结果是多少？**

[1，NaN,NaN]，因为 parseInt需要两个参数（val, radix），其中 radix表示解析时用的基数（进制）；map传递了3个参数（item, index,aray），对应的radix不合法导致解析失败。

**39、谈谈你对this对象的理解。**

this是 JavaScript的一个关键字，随着函数使用场合的不同，this的值会发生变化。但是有一个总原则，即this指的是调用函数的那个对象一般情况下，this是全局对象 Global，可以作为方法调用

**40、Web- garden和web-farm有什么不同？**

web-garden和 web-farm都是网络托管系统。唯一的区别是 web-garden是在单个服务器中包含许多处理器的设置，而web-farm是使用多个服务器的较大设置。

**41、说一下 document. write0的用法。**

document. write()方法可以用在两个地方，页面载入过程中用实时脚本创建页面内容，以及用延时脚本创建本窗口或新窗口的内容document. write只能重绘整个页面， innerHTML可以重绘页面的一部分。

**42、在 JavaScript中什么是类（伪）数组？如何将类（伪）数组转化为标准数组？**

典型的类（伪）数组是函数的 argument参数，在调用 getElements By TagName和 document .childNodes方法时，它们返回的 NodeList对象都属于伪数组。可以使用Array .prototype. slice. call（ fake Array）将数组转化为真正的Aray对象。

**43、JavaScript中callee和 caller的作用是什么？**

caller返回一个关于函数的引用，该函数调用了当前函数；callee返回正在执行的函数，也就是指定的 function对象的正文。

**44、讲一下手写数组快速排序的步骤。**

"快速排序”的思想很简单，整个排序过程只需要3步

（1）在数据集之中，选择一个元素作为“基准”（ pivot）。

（2）将所有小于“基准”的元素，都移到“基准”的左边；将所有大于“基准”的元素，都移到“基准”的右边。

（3）对“基准”左边和右边的两个子集，不断重复第（1）步和第（2）步，直到所有子集只剩下一个元素为止。

**45、如何统计字符串“ aaaabbbccccddfgh”中字母的个数或统计最多的字母数？**

具体代码如下

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
var str =aaaabbbecccddfgh"； function dealstr（str）{ var obj={}；for （var i= 0；i< str length：i++）{var v=str.charAt （i）；if （obj[v] && obj [v].value === v）{++obj[v]. count } else { obj[v] ={count：1， va⊥ue:v}}} return obj ；}var obj= dealstr（str）；for (key in obj）{ console. log （obj[key] .value +'=' obj[ key].count）}
```

**46、写一个 function，清除字符串前后的空格（兼容所有浏览器）。**

具体代码如下

- 
- 
- 

```
function trim（str）{if （str && typeof str === "string"）{ return str.replace（/^\s+1\s+$/g，""）；//去除前后空白符。
```

**47、列出不同浏览器中关于 JavaScript兼容性的两个常见问题。**

（1）事件绑定兼容性问题。

IE8以下的浏览器不支持用 add Event Listener来绑定事件，使用 attachement可以解决这个问题

（2） stopPropagation兼容性问题

IE8以下的浏览器不支持用 e .stopPropagation()来阻止事件传播，使用 e .return Value =false可以解决这个问题。

**48、闭包的优缺点是什么？**

优点是不产生全局变量，实现属性私有化缺点是闭包中的数据会常驻内存，在不用的时候需要删除，否则会导致内存溢出（内存泄漏）。

**49、用 JavaScript实现一个数组合并的方法（要求去重）。**

代码如下。

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
var arrl =['a']; var arr2 =['b'， 'c'];var arr3=['c', ['d'], 'e'， undefined, null];var concat =（ function() {//去重合并arr1和arr2var _concat =function （arrl, arr2）for （var i =0， len= arr2.length； i< len； i++){~ arrl. indexOf (arr2[i])|| arrl. push(arr2[i])}}//返回数组去重合并方法return function(){ var result =[];for (var i=0， len= arguments .length；i< len：i++){ _concat （result, arguments [i])return result}})()
```

执行concat（arrl,ar2，ar3）后，会返回['a',null, undefined，'e'，['d],'c','b']。

**50、说明正则表达式给所有string对象添加去除首尾空白符的方法（trim方法）。**

代码如下。

- 
- 
- 

```
prototype. trim= function(){return this .replace（/^\s+I\s+$/g，" ）；}；
```

**51、说明用 JavaScript实现一个提取电话号码的方法。**

代码如下

- 
- 
- 

```
var str="12345678901 021-12345678 有课前端网 0418-1234567  13112345678"； var reg=/（1\d{0}）|（0\d{2,3}\-\d{7,8}）/g；alert（str.match（reg）；
```

测试“12345678901 021-12345678有课前端网0418-1234567 13112345678”，得到的结果应该是：[12345678901,021-12345678,0418-1234567,13112345678]

**52、JavaScript中常用的逻辑运算符有哪些？**

"and”（&&）运算符、“or”（‖）运算符和"not"（！）运算符，它们可以在 JavaScript中使用。

**53、什么是事件代理（事件委托）？**

事件代理（ Event Delegation），又称为事件委托，是 JavaScript中绑定事件的常用技巧。顾名思义，“事件代理”就是把原本需要绑定的事件委托给父元素，让父元素负責事件监听。事件代理的原理是DOM元素的事件冒泡。使用事件代理的好处是可以提高性能。

**54、什么是 JavaScript？**

JavaScript是客户端和服务器端的脚本语言，可以插入HTML页面中，并且是目前较热门的Web开发语言，同时， JavaScript也是面向对象的编程语言。

**55、列举Java和 JavaScript的不同之处。**

Java是一门十分完整、成熟的编程语言。相比之下， JavaScript是一个可以被引入HTML页面的编程语言。这两种语言并不完全相互依赖，而是针对不同的意图而设计的。Java是一种面向对象编程（OOP）或结构化编程语言，类似的语言有C++；而 JavaScript是客户端脚本语言，它称为非结构化编程。

**56、JavaScript和ASP脚本相比，哪个更快？**

JavaScript更快。JavaScript是一种客户端语言，因此它不需要Web服务器的协助就可以执行；ASP是服务器端语言，因此它总是比 JavaScript慢，值得注意的是， JavaScript现在也可用于服务器端语言（ Node. js）

**57、什么是负无穷大？**

Infinity代表了超出 JavaScript处理范围的数值。也就是说， JavaScript无法处理的数值都是 Infinity.实践证明， JavaScript所能处理的最大值（ Number. MAX VALUE）是17976931348623157e+308，超过该数则为正无穷大；而最小值（ Number. MIN VALUE）

是5e-324，小于该数则为0.所以负无穷大代表的是小于- Number MAX VALUE的数字， JavaScript中对应静态变量 Number NEGATIVE INFINITY

**58、如何将 JavaScript代码分解成几行？**

M：在字符串语句中可以通过在第一行末尾使用反斜杠“\”来完成，例如， document. write（"This is \a program"）。

如果不是在字符串语句中更改为新行，那么 JavaScript会忽略行中的断点下面的代码是完美的，但并不建议这样做，因为阻碍了调试。

- 
- 
- 

```
var x=l, y=2，z=X+y;
```

**59、什么是未声明和未定义的变量？**

未声明的变量是程序中不存在且未声明的变量。如果程序尝试读取未声明变量的值，则会在运行时遇到错误。未定义的变量是在程序中声明但尚未给出任何值的变量如果程序尝试读取未定义变量的值，则返回未定义的值60.：如何编写可动态添加新元素的代码？

下面给出一段示例代码

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
<！DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><tit1e>有课前端网—专业前端技术学习网</tit1e></head><body><p id="ickt">ickt</p><script type="text/javascript">function addNode (){ var p= document. createElement（'p'）；var textNode document, createTextNode（'有课前端网'）p .appendchild（textNode);document. getElementById（'ickt'） .appendChild（p）}addNode ()</script></body></html>
```

**61、什么是全局变量？这些变量如何声明？使用全局变量有哪些问题？**

全局变量是整个代码中都可用的变量，也就是说，这些变量没有任何作用域var关键字用于声明局部变量，如果省略var关键字，则声明一个全局变量使用全局变量面临的问题是局部变量和全局变量名称的冲突。此外，很难调试和测试依赖于全局变量的代码。

**62、解释 JavaScript中定时器的工作，并说明使用定时器的缺点。**

定时器用于在设定的时间执行一段代码，或者在给定的时间间隔内重复该代码这通过使用函数 setTimeout、setInterval和 clearInterva来完成。

setTimeout（ function, delay）函数用于启动在所属延迟之后调用特定功能的定时器。

setInterval（ function,dlay）函数用于在提到的延迟中重复执行给定的功能，只有在取消时才停止。

clearInterval（id）函数指示定时器停止定时器在一个线程内运行，因此事件可能需要排队等待执行。

**63、ViewState和 SessionState有什么区别？**

View State特定于会话中的页面； SessionState特定于可在Web应用程序中的所有页面上访问的用户特定数据。

**64、什么是===运算符？**

===称为严格等式运算符，当两个操作数具有相同的值和类型时，该运算符返回true。

**65、说明如何使用 JavaScript提交表单。**

要使用 JavaScript提交表单，可以使用以下代码。

document .form [0] .submit();

**66、元素的样式/类如何改变？**

可以通过以下方式改变元素的样式。

- 

```
document. getElementById（"myText").style. fontsize ="20";
```

可以通过以下方式改变元素的类。

- 

```
document. getElementById（"myText "）.className ="anyclass"；
```

**67、JavaScript中的循环结构都有哪些？**

for、 while、do.… while、 for in、 for of（ES6新增的）

**68、如何在 JavaScript中将base字符串转换为 integer？**

parselnt()函数解析一个字符串参数，并返回一个指定基数的整数。 parselnt()将要转换的字符串作为其第一个参数，第二个参数是给定字符串的转换进制基数。

为了将4F（基数16）转换为整数，可以使用代码 parrent（"4F"，16）。

**69、说明“==”和“===”的区别。**

“==”仅检查值相等性，而“===”用于更严格的等式判定。如果两个变量的值或类型不同，则后者返回 false。

**70、3+2+“7”的结果是什么？**

由于3和2是整数，它们将直接相加，同时由于“7”是一个字符串，将会被直连接，因此结果将是57。

**71、如何检测客户端机器上的操作系统？**

为了检测客户端机器上的操作系统，应使用 navigator.app Version字符串（属性）。

**72、JavaScript中的null表示什么？**

null用于表示无值或无对象。它意味着没有对象或空字符串，没有有效的布尔没有数值和数组对象

**73、delete操作符的功能是什么？**

delete操作符用于删除对象中的某个属性，但不能删除变量、函数等。

**74、JavaScript中有哪些类型的弹出框？**

ua； alert、 confirm和 prompt。

**75、void（0）的作用是什么？**

void操作符使表达式的运算结果返回 undefined。

void（0）用于防止页面刷新，并在调用时传递参数“0”。

void（0）用于调用另一种方法而不刷新页面。

**76、如何强制页面加载 JavaScript中的其他页面？**

必须插入以下代码才能达到预期效果。

- 
- 
- 

```
<script language="JavaScript"  type="text/javascript"><!--location.href="http://newhost/newpath/newfile.html";//--></script>
```

**77、转义字符是用来做什么的？**

当使用特殊字符（如单引号、双引号、撇号和&符号）时，将使用转义字符（反斜杠）。在字符前放置反斜杠，使其显示。

下面给出两个示例。

- 
- 

```
document. write"I m a "good"boy "document. write"I m a\"good\"boy"
```

**78、什么是 JavaScript cookie？**

cookie是存储在访问者计算机中的变量。每当一台计算机通过浏览器请求某个页面时，就会发送这个 cookie。可以使用 JavaScript来创建和获取 cookie的值。

**79、解释 JavaScript中的pop()方法。**

pop()方法与shift()方法类似，但不同之处在于shift()方法在数组的开头工作。此外，pop()方法将最后一个元素从给定的数组中取出并返回，然后改变被调用的数组例如：

- 
- 
- 

```
var colors = ["red"，"blue"，"green"]； colors. pop ()；// colors ：["red"，"blue"]
```

**80、在 JavaScript中使用 innerHTML的缺点是什么？**

缺点如下：

（1）内容随处可见

（2）不能像“追加到 innerHTML”一样使用。

（3）即使使用+=，如" innerHTML= innerhTML+'htm'"，旧的内容仍然会被HTML替换。

（4）整个 innerHTML内容被重新解析并构建成元素，因此它的速度要慢得多。

（5） innerHTML不提供验证，因此可能会在文档中插入具有破坏性的HTML并将其中断。

**81、break和 continue语句的作用是什么？**

break语句从当前循环中退出； continue语句继续下一个循环语句。

**82、在 JavaScript中， datatypes的两个基本组是什么？**

两个基本组是原始类型和引用类型。

原始类型包括数字和布尔类型。引用类型包括更复杂的类型，如字符串和日期。

**83、如何创建通用对象？**

通用对象可以通过以下代码创建。

var o= new Object ()。

**84、typeof是用来做什么的？**

typeof是一个运算符，用于返回变量类型的字符串描述。

**85、哪些关键字用于处理异常？**

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
try...catch...finally用于处理 JavaScript中的异常。try{执行代码}catch（exp）{ 抛出错误提示信息}finally {无论try/catch的结果如何都会执行。}
```

**86、JavaScript中不同类型的错误有几种？**

有3种类型的错误。

Load time errors，该错误发生于加载网页时，例如出现语法错误等状况，称为加载时间错误，并且会动态生成错误。

Run time errors，由于在HTML语言中滥用命令而导致的错误。

Logical errors，这是由于在具有不同操作的函数上执行了错误逻辑而发生的错误。

**87、在 JavaScript中，push方法的作用是什么？**

push方法用于将一个或多个元素添加或附加到数组的末尾。使用这种方法，可通过传递多个参数来附加多个元素。

**88、在 JavaScript中， unshift方法的作用是什么？**

unshift方法就像在数组开头工作的push方法。该方法用于将一个或多个元素添加到数组的开头。

**89、如何为对象添加属性？**

为对象添加属性有两种常用语法。

中括号语法，比如obj[" class"]=12。

点语法，比如 obj. class=12。

**90、获得 CheckBox状态的方式是什么？**

alert（ document getElement Byld（'checkbox1'） .checked；

如果 CheckBox选中，此警告将返回TRUE。

**91、解释一下 window. onload和 onDocumentReady。**

在载入页面的所有信息之前，不运行 window. onload。这导致在执行任何代码之前会出现延迟。

window.onDocumentReady在加载DOM之后加载代码。这允许代码更早地执行（早于 window. onload）。

**92、如何理解 JavaScript中的闭包？**

闭包就是能够读取其他函数内部变量的函数。

闭包的用途有两个，一是可以读取函数内部的变量，二是让这些变量的值始终保持在内存中。

**93、如何把一个值附加到数组中？**

可以在数组末尾处添加成员arr[ arr length]= value；或者调用push方法 arr.push（value）。

**94、解释一下for-in循环。**

for-in循环用于循环对象的属性。

for-in循环的语法如下。

for （var iable name in object){}

在每次循环中，来自对象的一个属性与变量名相关联，循环继续，直到对象的所有属性都被遍历。

**95、描述一下 JavaScript中的匿名函数。**

被声明为没有任何命名标识符的函数称为匿名函数。一般来说，匿名函数在声明后无法访问。

匿名函数声明示例如下。

- 
- 
- 

```
var anon=function(){alert('I am anonymous' );anon();
```

**96、和DOM事件流的区别是什么？**

区别如下。

（1）执行顺序不一样

（2）参数不一样。

（3）事件名称是否加on不一样。

（4）this指向问题不一样。

**97、阐述一下事件冒泡。**

Java Script允许DOM元素嵌套在一起。在这种情况下，如果单击子级的处理程序，父级的处理程序也将执行同样的工作。

**98、JavaScript里函数参数 arguments是数组吗？**

在函数代码中，使用特殊对象 arguments，开发者无须明确指出参数名，使用下标就可以访问相应的参数。

arguments虽然有数组的性质，但其并非真正的数组。它只是一个类数组对象，并没有数组的方法，不能像真正的数组那样调用 .join()、， .concat()、.pop()等方法。

**99、什么是构造函数？它与普通函数有什么区别？**

构造函数是一种特殊的方法，主要用来创建对象时初始化对象，经常与new运算符一起使用，创建对象的语句中构造函数的名称必须与类名完全相同。

与普通函数相比，区别如下

（1）构造函数只能由new关键字调用

（2）构造函数可以创建实例化对象

（3）构造函数是类的标志。

**100、请解释一下 JavaScript和CSS阻塞。**

JavaScript的阻塞特性是所有浏览器在下载 JavaScript代码的时候，会阻止其他一切活动，比如其他资源的下载，内容的呈现等，直到 JavaScript代码下载、解析、执行完毕后才开始继续并行下载其他资源并渲染内容。

为了提高用户体验，新一代浏览器都支持并行下载 JavaScript代码，但是 JavaScript代码的下载仍然会阻塞其他资源的下载（例如图片、CSS文件等）。

为了防止 JavaScript修改DOM树，浏览器需要重新构建DOM树，所以就会阻塞其他资源的下载和渲染。

嵌入的 JavaScript代码会阻塞所有内容的呈现，而外部 JavaScript代码只会阻塞其后内容的显示，两种方式都会阻塞其后资源的下载。也就是说，外部脚本不会阻塞外部脚本的加载，但会阻塞外部脚本的执行。

CSS本来是可以并行加载的，但是当CSS后面跟着嵌入的 JavaScript代码的时候，该CSS就会阻塞后面资源的下载。

而当把嵌入的 JavaScript代码放到CSS前面时，就不会出现阻塞的情况了（在IE6下CSS都会阻塞加载）。

根本原因是因为浏览器会维持HTML中CSS和 JavaScript代码的顺序，样式表必须在嵌入的 JavaScript代码执行前先加载、解析完。而嵌入的 JavaScript代码会阻塞后面的资源加载，所以就会出现CSS阻塞资源加载的情况。
# **1、CSS attr函数**

该函数attr()返回所选元素的属性值。它使我们可以进入HTML，获取属性的内容，并将其提供给CSS content属性。

看下面的例子：

```css
/* <div data-example="Medium"></div> */ 
div:after {     
    content: attr(data-example); 
}
```

下面的示例将Medium在页面上显示属性。你可以在网页上尝试。

Codepen示例地址：https://codepen.io/MehdiAoussiad/pen/wvzBQwb

# **2、calc函数**

该函数calc()允许您执行计算以确定CSS属性值。所有主要浏览器都支持它。

这个函数有两个参数和来自操作者的计算结果（+，-，*，/）你提供它，具有或不具有伴随的单元中提供的那些参数是数字。

这是一个例子：

```css
.element {
    width：calc（100vw-80px） ; 
}
```

使用函数计算div元素的宽度calc()。css

Codepen示例地址：https://codepen.io/MehdiAoussiad/pen/RwGNqPe

# **3、var函数**

该函数var() 用于插入CSS变量的值。这对于创建一些CSS变量以在我们的代码中的许多地方使用它们很有用。

看下面的例子：

```css
:root {  
    --main-bg-color: coral;  
    --main-txt-color: blue;  
    --main-padding: 15px;
}
#div1 {  
    background-color: var(--main-bg-color);  
    color: var(--main-txt-color);  
    padding: var(--main-padding);
}
#div2 {  
    background-color: var(--main-bg-color);  
    color: var(--main-txt-color);  
    padding: var(--main-padding);
}
```

如上所示，我们在root元素中创建了值，然后使用function在div元素中使用了它们var()。

# **4、过滤功能**

该功能filter()将图形更改应用于输入图像和元素的外观。是我们可以实现的效果如下：（ ，blur，brightness，contrast，grayscale，hue-rotate，opacity，invert，sepia，）。saturate drop-shadow我认为，如果我没记错的话，还有更多。

这是一个例子：

```css
.element1 {     
    filter: drop-shadow(0.25rem 0 0.75rem #ef9035); 
}// Or:
.element2 {  
    filter: blur(20px);
}
```

Codepen示例地址：https://codepen.io/MehdiAoussiad/pen/JjRoeEL
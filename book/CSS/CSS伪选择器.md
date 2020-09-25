## **1、::first-line | 选择首行文本**

这个伪元素选择器选择换行之前文本的首行。

```css
p:first-line {    color: lightcoral;}
```

## **2、::first-letter | 选择首字母**

这个伪元素选择器应用于元素中文本的首字母。

```css
.innerDiv p:first-letter {    color: lightcoral;     font-size: 40px  }
```

## **3、::selection | 选择高亮（被选中）的区域**

应用于任何被用户选中的高亮区域。

通过 `::selection` 伪元素选择器，我们可以将样式应用于高亮区域。

```css
div::selection {    background: yellow;}
```

## **4、:root | 根元素**

`:root` 伪类选中文档的根元素。在 HTML 中，为 HTML 元素。在 RSS 中，则为 RSS 元素.

这个伪类选择器应用于根元素，多用于存储全局 CSS 自定义属性。

## **5、:empty | 仅当元素为空时触发**

这个伪类选择器将选中没有任何子项的元素。该元素必须为空。如果一个元素没有空格、可见的内容、后代元素，则为空元素。

```css
div:empty {    border: 2px solid orange;}<div></div><div></div><div></div>
```

这个规则将应用于空的 `div` 元素。这个规则将应用于第一个和第二个 `div`，因为他们是真为空，而第三个 `div` 包含空格。

## **6、:only-child | 选择仅有的子元素**

匹配父元素中没有任何兄弟元素的子元素。

```css
.innerDiv p:only-child {    color: orangered;  }
```

## **7、:first-of-type | 选择第一个指定类型的子元素**

```css
.innerDiv p:first-of-type {    color: orangered;    }
```

这将应用于 `.innerDiv` 下的第一个 `p` 元素。

```html
<div class="innerDiv">    <div>Div1</div>       <p>These are the necessary steps</p>        <p>hiya</p>           <p>  Do <em>not</em> push the brake at the same time as the accelerator.    </p>                  <div>Div2</div>   </div>
```

这个 `p`（“These are the necessary step”）将被选中。

## **8、:last-of-type | 选择最后一个指定类型的子元素**

像 `:first-of-type` 一样，但是会应用于最后一个同类型的子元素。

```css
.innerDiv p:last-of-type {    color: orangered;   }
```

这将应用于 `innerDiv` 下的最后一个 `p` 段落元素。

```html
<div class="innerDiv">    <p>These are the necessary steps</p>        <p>hiya</p>              <div>Div1</div>                <p>        Do the same.    </p>                     <div>Div2</div>       </div>
```

因此，这个 `p` 元素（“Do the same”）将被选中。

## **9、:nth-of-type() | 选择特定类型的子元素**

这个选择器将从指定的父元素的孩子列表中选择某种类型的子元素。

```css
.innerDiv p:nth-of-type(1) {    color: orangered;    }
```

## **10、:nth-last-of-type() | 选择列表末尾中指定类型的子元素**

这将选择最后一个指定类型的子元素。

```css
.innerDiv p:nth-last-of-type() {     color: orangered;     }
```

这将选择 `innerDiv` 列表元素中包含的最后一个段落类型子元素。

```html
<div class="innerDiv">    <p>These are the necessary steps</p>      <p>hiya</p>          <div>Div1</div>           <p>        Do the same.    </p>               <div>Div2</div>        </div>
```

`innerDiv` 中最后一个段落子元素 `p`（“Do the same”）将会被选中。

## **11、:link | 选择一个未访问过的超链接**

这个选择器应用于未被访问过的链接。常用于带有 href 属性的 `a` 锚元素。

```css
a:link {    color: orangered; }<a href="/login">Login<a>
```

这将选中未被点击过带有 `href` 的指定界面的 `a` 锚点元素，选中的元素中的文字将会显示为橙色。

## **12、:checked | 选择一个选中的复选框**

这个应用于已经被选中的复选框。

```css
input:checked {    border: 2px solid lightcoral;   }
```

这个规则应用到所有被选中的复选框。

## **13、:valid | 选择一个通过验证的元素**

这主要用于可视化表单元素，以让用户判断是否验证通过。验证通过时，默认元素带有 `valid` 属性。

```css
input:valid {    boder-color: lightsalmon;    }
```

## **14、:invalid | 选择一个未通过验证的元素**

像 `:valid` 一样，但是会应用到未通过验证的元素。

```css
input[type="text"]:invalid {    border-color: red;}
```

## **15、:lang() | 选择指定语言的元素**

应用于指定了语言的元素。

可以通过以下两种方式使用：

```css
p:lang(fr) {    background: yellow;    }
```

或者

```css
p[lang|="fr"] {background: yellow;}
<p lang="fr">Paragraph 1</p>
```

## **16、:not() | 对于选择取反（这是一个运算符）**

否定伪类选择器选中相反的。

让我们看一个示例：

```html
.innerDiv :not(p) {    color: lightcoral;  }   
<div class="innerDiv">       
    <p>Paragraph 1</p>        
    <p>Paragraph 2</p>           
    <div>Div 1</div>                
    <p>Paragraph 3</p>                  
    <div>Div 2</div>
</div>
```

`Div 1` 和 `Div 2` 会被选中，因为他们不是 `p` 元素。

**17、清除浮动的原理**

原理：如果子元素全部浮动，父元素就会塌陷，所以在所有浮动子元素后添加一个没有浮动元素把父元素撑起来，这个元素找不到、不占据空间，不能让它里面有内容，有内容也要隐藏

```css
.clearfix:after{content:'.';
　　　　　　　　　　clear:both;
　　　　　　　　　　display:block;
　　　　　　　　　　height:0;
　　　　　　　　　　overflow:hidden;
　　　　　　　　　　visibility:hidden;
　　　　　　　　}
.clearfix:after{zoom：1；}/*解决IE的问题*/
//visibility：hidden；隐藏元素，但是位置留着
//display：none；隐藏元素，不占据空间，彻底隐藏
//after：伪对象选择符
```


## Vue中实现输入框Input输入格式限制

#### **1、使用修饰符实现数字输入**

在VUE中可以在v-modal后添加修饰符的形式来限制输入，比如：

```vue
<input v-model.number="testValue" type="number">
```

.number可以实现限制数字输入，但是会有以下问题：

会出现type="number"自带样式，当然可以通过添加以下css清除

```css
/* 普通IE浏览器 样式清除 */
input::-webkit-outer-spin-button,input::-webkit-inner-spin-button{  
    -webkit-appearance: none !important;
}
/* 火狐浏览器样式清除 */
input[type="number"]{  
    -moz-appearance:textfield;
}
```

可以无限输入特殊符号+-.，会导致清空data中的值testValue

这里的修饰符也无法实现定制限制输入，不能满足要求

#### **2、监听输入框变化**

通过@input监听更新数据，实现只能输入数字，而且可以自行定制限制输入内容

```vue
<input v-model="testValue" @input="testValue = testValue.replace(/[^\d]/g,'')">
```

此方法可以满足需求，但是无法封装进行批量使用。

#### **3、封装全局指令**

封装input限制输入指令。

```js
//input.js
const addListener = function(el, type, fn) {  
    el.addEventListener(type, fn, false)
}
//去掉空格
const spaceFilter = function(el) {  
    addListener(el, 'input', () => {    
        el.value = el.value.replace(/\s+/, '')  
    })
}
// 限制只能输入整数和小数（适用于价格类、最多两位小数）
const priceFilter = function(el) {  
    addListener(el, 'input', () => {    
        el.value = (el.value.match(/^\d*(\.?\d{0,2})/g)[0]) || null    
        if (isNaN(el.value)) {      
            el.value = ''    
        }  
    })
}
// 限制只能输入字母数字（适用于运单号）
const integerLetterFilter = function(el) {  
    addListener(el, 'input', () => {    
        el.value = el.value.replace(/[\W]/g,'')
        el.dispatchEvent(new Event('input'))  
    })
}
export default {  
    bind(el, binding) {    
        if (el.tagName.toLowerCase() !== 'input') {      
            el = el.getElementsByTagName('input')[0]    
        }    
        spaceFilter(el)    
        switch (binding.arg) {      
            case 'price':        
                priceFilter(el)        
                break      
            case 'integerLetter':        
                integerLetterFilter(el)        
                break      
            default:        
                console.warn('未知指令类型',binding.arg)        
                break    
        }  
    }}
```

注册全局自定义指令

```js
//main.js
import inputFilter from '@/directives/InputFilter.js'
vue.directive('inputFilter', inputFilter)
```

使用v-input-filter指令

```vue
<input v-modal="testValue" v-input-filter:price>
```

这样封装在使用时会出现一个隐蔽的bug，就是在输入指令中正则限制以外的字符时，视图中输入框显示是正确的，但是在浏览器控制栏Vue Devtools中的testValue最后一位字符是最后输入的时的字符。

比如输入abc、123abc输入框内是 、123，但实际testValue值是c、123c。

原因是vue中绑定的值是通过监听input进行赋值的，直接修改输入框值不会触发input事件，需要通过dispatchEvent再次手动触发input事件，但是在监听的input回调中再触发input会无限循环，所以调整为监听keyup事件来修改值，修改如下:

```js
//input.js
···
// 防抖
let debounce = (fn, delay) => {
    var delay = delay || 300;
    var timer;
    return function() {
        var th = this;
        var args = arguments;
        if (timer) {      
            clearTimeout(timer);    
        }    
        timer = setTimeout(function() {      
            timer = null;      
            fn.apply(th, args);    
        }, delay);  
    };
}
// 限制只能输入整数和小数（适用于价格类、最多两位小数）
const priceFilter = function(el) {  
    addListener(el, 'keyup', debounce(() => {
        //添加防抖 方便添加小数点    
        el.value = (el.value.match(/^\d*(\.?\d{0,2})/g)[0]) || null
        if (isNaN(el.value)) {      
            el.value = ''    
        }
        //格式化去掉却没有输入小数位的小数点    
        el.value = +el.value
        //触发input事件    
        el.dispatchEvent(new Event('input'))  }))
}
// 限制只能输入字母数字（适用于运单号）
const integerLetterFilter = function(el) {  
    addListener(el, 'keyup', () => {    
        el.value = el.value.replace(/[\W]/g,'')    
        el.dispatchEvent(new Event('input'))  
    })
}
···
```






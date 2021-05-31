## 起因

要捕获 JavaScript 代码中的异常一般会采用 try catch，不过 try catch 的使用是否是对代码性能产生影响呢？答案是肯定有的，但是有多少不得而知。

淘宝前端线上脚本错误的捕获方法：

```js
window.JSTracker = window.JSTracker || []; 
try{  
    //your code 
}catch(e){  
    JSTracker.push(e);  
    throw e; //建议将错误再次抛出，避免测试无法发现异常 
} 
```

## 设计实验方式

简单的设计方案也就是对比实验。

空白组1：[无 try catch 的情况下对数据取模1千万次耗时]

```js
 <!DOCTYPE html> 
     <html> 
     <head>    
     <title>1 无try catch的情况耗时</title>    
<script>        
     !function() {            //无try catch的情况耗时            
         var t = new Date();             //耗时代码开始            
         for (var i = 0; i < 100000000; i++) {                
             var p = i % 2;            
         }            //耗时代码结束            
         document.write(new Date() - t);        
     }();    
</script> 
</head> 
<body> 
</body> 
</html> 
```

参照组2：[将耗时代码用 try 包围，内联耗时代码]

```
<!DOCTYPE html> <html> <head>    <title>2 在 try 中内联代码的耗时情况</title>    <script>        !function() {             //在 try 中内联代码的耗时情况            var t = new Date();            try{                //耗时代码开始                for (var i = 0; i < 100000000; i++) {                    var p = i % 2;                }                //耗时代码结束                throw new Error();            }catch(e){             }            document.write(new Date() - t);        }();    </script> </head> <body> </body> </html> 
```

参照组3：[将耗时代码用 try 包围，外联耗时代码]

```
<!DOCTYPE html> <html> <head>    <title>3 在 try 中内联代码的耗时情况</title>    <script>        !function() {            function run(){                //耗时代码开始                for (var i = 0; i < 100000000; i++) {                    var p = i % 2;                }                //耗时代码结束            }            //在 try 中内联代码的耗时情况            var t = new Date();            try{                run();                throw new Error();            }catch(e){             }            document.write(new Date() - t);        }();    </script> </head> <body> </body> </html> 
```

参照组4：[将耗时代码用 catch 包围，内联耗时代码]

```
<!DOCTYPE html> <html> <head>    <title>4 在 catch 中内联代码的耗时情况</title>    <script>        !function() {             //在 catch 中内联代码的耗时情况            var t = new Date();            try{                throw new Error();            }catch(e){                //耗时代码开始                for (var i = 0; i < 100000000; i++) {                    var p = i % 2;                }                //耗时代码结束             }            document.write(new Date() - t);        }();    </script> </head> <body> </body> </html> 
```

参照组5：[将耗时代码用 catch 包围，外联耗时代码]

```
<!DOCTYPE html> <html> <head>    <title>5 在 catch 中内联代码的耗时情况</title>    <script>        !function() {            function run(){                //耗时代码开始                for (var i = 0; i < 100000000; i++) {                    var p = i % 2;                }                //耗时代码结束            }            //在 catch 中内联代码的耗时情况            var t = new Date();            try{                throw new Error();            }catch(e){                run();            }            document.write(new Date() - t);        }();    </script> </head> <body> </body> </html> 
```

## 运行结果（只选取了 Chrome 作为示例）

| -        | 不使用 try-catch | try 中耗时，内联代码 | try 中耗时，外联代码 | catch 中耗时，内联代码 | catch 中耗时，外联代码 |
| -------- | ---------------- | -------------------- | -------------------- | ---------------------- | ---------------------- |
| Chrome44 | 98.2             | 1026.9               | 107.7                | 1028.5                 | 105.9                  |

## 给出总结

- 使用 try catch 的使用无论是在 try 中的代码还是在 catch 中的代码性能消耗都是一样的。
- 需要注意的性能消耗在于 try catch 中不要直接塞进去太多的代码（声明太多的变量），最好是吧所有要执行的代码放在另一个 function 中，通过调用这个 function 来执行。

针对第二点，可以查看 ECMA 中关于 try catch 的解释，在代码进入 try catch 的时候 js引擎会拷贝当前的词法环境，拷贝的其实就是当前 scope 下的所有的变量。

## 建议

在使用 try catch 的时候尽量把 try catch 放在一个相对干净的 scope 中，同时在 try catch 语句中也尽量保证足够少的变量，最好通过函数调用方式来 try catch。

## 试验中的现象解释

测试过程中还是发现了一个疑问, 以下两段代码在 Chrome 44 中运行出来的结果差距非常大，加了一句空的 try catch 之后平均为：850ms，加上之前为：140ms。

```
!function() {     //无 try catch 的情况耗时     var t = new Date();      //耗时代码开始     for (var i = 0;  i < 100000000;  i++) {         var p = i % 2;     }     //耗时代码结束     document.write(new Date() - t);     try{     }catch(e){      } }(); 
!function() {     //无 try catch 的情况耗时     var t = new Date();      //耗时代码开始     for (var i = 0;  i < 100000000;  i++) {         var p = i % 2;     }     //耗时代码结束     document.write(new Date() - t);  }(); 
```

其实原因很简单
只要把代码改为这样 耗时就降下来了：

```
!function() {         !function() {             //无 try catch 的情况耗时             var t = new Date();              //耗时代码开始             for (var i = 0;  i < 100000000;  i++) {                 var p = i % 2;             }             //耗时代码结束             document.write(new Date() - t);         }();         try{         }catch(e){          }     }();
```
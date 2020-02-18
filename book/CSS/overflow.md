# overflow

#### 1. overflow改变时，样式变化问题

例：

```css
overflow: hidden;
```

由于overflow变化后，会生成一个新的box，默认的vertical-align：baseline发生改变；

解决办法就是，设置vertical-align

```css
vertical-align: top/bottom/middle;
```


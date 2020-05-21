注：主要用于对触摸屏的手势限制，禁止放大缩小

```js
<script>
    window.addEventListener("touchstart", function(event){ event.preventDefault()}, false);
  	document.addEventListener('gesturestart', function(event) {event.preventDefault();},false);
</script>
```

另有meta方式，属于常规限制

```js
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1,user-scalable=no">
```


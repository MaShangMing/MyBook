# Section1.1

\1. ncontextmenu="window.event.returnValue=false" 将彻底屏蔽鼠标右键

<table borderncontextmenu=return(false)><td>no</table> 可用于Table

\2. <body nselectstart="return false"> 取消选取、防止复制

3.onpaste="return false" 不准粘贴

4.oncopy="return false;" ncut="return false;" 防止复制

\5. <link rel="Shortcut Icon"href="favicon.ico"> IE地址栏前换成自己的图标

\6. <link rel="Bookmark"href="favicon.ico"> 可以在收藏夹中显示出你的图标

\7. <inputstyle="ime-mode:disabled"> 关闭输入法

\8. 永远都会带着框架

<script. language="JavaScript"><!--

if (window == top)top.location.href = "frames.htm"; //frames.htm为框架网页

// --></script>

\9. 防止被人frame.

<SCRIPT. LANGUAGE=JAVASCRIPT><!--

if (top.location != self.location)top.location=self.location;

// --></SCRIPT>

\10. 网页将不能被另存为

<noscript><iframe.src=*.html></iframe></noscript>

\11. 查看网页源代码

<input type=button value=查看网页源代码

onclick="window.location = "view-source:"+"http://www.pconline.com.cn"">

12.删除时确认

<a href="javascript:if(confirm("确实要删除吗?"))location="boos.asp? &areyou=删除&page=1"">删除</a>

13.屏蔽功能键Shift,Alt,Ctrl

<script>

function look(){

if(event.shiftKey)

alert("禁止按Shift键!");//可以换成ALT　CTRL

}

document.onkeydown=look;

</script>

\14. 网页不会被缓存

<META. HTTP-EQUIV="pragma" CONTENT="no-cache">

<META. HTTP-EQUIV="Cache-Control"CONTENT="no-cache, must-revalidate">

<META. HTTP-EQUIV="expires"CONTENT="Wed, 26 Feb 1997 08:21:57 GMT">

或者<META. HTTP-EQUIV="expires"CONTENT="0">

15.怎样让表单没有凹凸感？

<input type=text style="border:1 solid #000000">

或 <input type=text style="border-left:none;border-right:none; border -top:none; border-bottom: 1 solid#000000"></textarea>
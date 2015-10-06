# jquery
##window.onload与$(document).ready()的区别
* 执行时机：window.onload：网页中所有元素完全加载到浏览器后执行，js此时才可以访问网页中的任何元素；$(document).ready()：DOM完全就绪时就可以被调用，网页中的所有元素对jquery来说是可以访问的，但并不意味着这些元素关联的文件都已经下载完毕。
`````` $(window).load(function(){
//解决关联文件未下载完导致的属性失效问题
}) ``````
* 多次使用：onload事件一次只能保存对一个函数的引用，它会自动用后面的函数覆盖前面的函数，为了达到两个函数顺序触发的效果，只能再创建一个新的js方法来实现；$(document).ready()每次调用都在现有的行为上追加新的行为，顺序依次执行。
``````window.onload = function(){
	one();
	two();
}
$(document).ready(function(){
	one();
})
$(document).ready(function(){
	two();
})``````
* 简写方式
``````$(document).ready(function(){})
$(function(){})
$().ready(function(){})
``````
****** 



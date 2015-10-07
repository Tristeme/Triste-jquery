# jquery
##事件
####window.onload与$(document).ready()的区别
* 执行时机：window.onload：网页中所有元素完全加载到浏览器后执行，js此时才可以访问网页中的任何元素；$(document).ready()：DOM完全就绪时就可以被调用，网页中的所有元素对jquery来说是可以访问的，但并不意味着这些元素关联的文件都已经下载完毕。
`````` 
$(window).load(function(){
//解决关联文件未下载完导致的属性失效问题
}) 
``````
* 多次使用：onload事件一次只能保存对一个函数的引用，它会自动用后面的函数覆盖前面的函数，为了达到两个函数顺序触发的效果，只能再创建一个新的js方法来实现；$(document).ready()每次调用都在现有的行为上追加新的行为，顺序依次执行。
``````
window.onload = function(){
	one();
	two();
}
$(document).ready(function(){
	one();
})
$(document).ready(function(){
	two();
})
``````
* 简写方式
``````
$(document).ready(function(){})
$(function(){})
$().ready(function(){})
```````
####事件绑定
``````
bind(type [,data],fn)
``````
type:事件类型：blur,focus,load,resize,scroll,unload,click,dblclick,mousedown,mouseup,mousemove,mouseover,mouseout,mouseenter,mouseleave,change,select,submit,keydown,keypress,keyup,error
data:可选参数，作为event.data属性值传递给事件对象的额外数据对象
fn:用来绑定的处理函数
``````
//绑定click事件，单击标题后显示内容
$(function(){
	$("h5.head").bind("click",function(){
		$(this).next().show();
	})
})
//this引用的是携带相应行为的DOM元素
$(function(){
	$("h5.head").bind("click",function(){
		$content = $(this).next();
		if($content.is(":visible")){
			$content.hide();
		}
		else{
			$content.show();
		}
	})
})
//简写绑定事件
$function(){
	$("h5.head").mouseover(function(){
		$(this).next().show();
	}).mouseout(function(){
		$(this).next().hide();
	})
}
``````
####合成事件
hover(enter,leave)；用于模拟光标悬停事件
toggle(fn1,fn2,...fnN);用于模拟鼠标连续点击事件
``````
$function(){
	$("h5.head").hover(function(){
		$(this).next().show();
	}).(function(){
		$(this).next().hide();
	})
}
``````
* ie6中":hover"仅可用于超链接元素，对于其他元素可以使用hover（）方法
* hover（）方法是替代jquery中的bind("mouseenter"),bind("mouseleave")，而不是bind("mouseover"),bind("mouseout")，因此当需要触发hover方法的第二个函数时，需要用trigger("mouseleave")

``````
$(function(){
	$("h5.head").toggle(function(){
		$(this).next().hide();
	}.funtion(){
		$(this).next().show();
	})
})
``````
toggle()方法的另一个作用：切换元素的可见状态
``````
$(function(){
	$("h5.head").toggle(function(){
		$(this).next().toggle();
	}.funtion(){
		$(this).next().toggle();
	})
})
``````
####事件冒泡
  在页面上可以有多个事件，也可以有多个元素相应同一个事件。有必要对事件的作用范围进行限制。
#####事件对象：
``````
$("element").bind("click",function(event){
//当点击event元素时，事件对象就被创建，这个事件对象只有事件处理函数才能访问。事件处理函数执行完毕后，事件对象被销毁。
});
``````
#####停止事件冒泡：阻止事件中其他对象的事件处理函数被执行，stopPropagation();
``````
$("span").bind("click",function(event){
	var txt = $('#msg').html() + "<p>内层span元素被点击</p>"；
	$('#msg').html(txt);
	event.stopPropagation();
});
//当单机span元素时，只会触发span元素上的click事件，而不会触发div和body元素的click事件
//可改写为return false;
``````
#####阻止默认行为：preventDefault()
``````
$(function(){
	$("#sub").bind("click",function(event){
		var username = $("#username").val();
		if(username==""){
			$("#msg").html("<p>不能为空！</p>")；
			event.preventDefault();
		}
	})
})
//可改写为return false;
``````
#####事件捕获

####事件对象的属性
* event.type获取事件类型
* event.preventDefault()
* event.stopPropagation()
* event.target获取到触发事件的元素
``````
$("a[href = 'http://triste.me']").click(function(event){
	var tg = event.target;
	alert(tg.href);
	return false;
})
``````
* event.relatedTarget：在标准DOM中，mouseover和mouseout所发生的元素可以通过event.target来访问，相关元素是通过event.relatedTarget来访问。
* event.pageY,event.pageX：获取光标相对于页面的x坐标和y坐标，如果页面上有滚动条，则还要加上滚动条的宽度或高度
* event.which:在鼠标单击事件中获取鼠标的左中右键，在键盘事件中获取键盘的按键
* event.metaKey:为键盘事件中获取<ctrl>按键
####移除事件
unbind([type],[data]);
【事件类型】，【将要移除的函数】
* 如果没有参数，则删除所有绑定的事件
* 如果提供了事件类型作为参数，则只删除该类型的绑定事件
* 如果把在绑定时传递的处理函数作为第二个参数，则只有这个特定的事件处理函数会被删除
``````
$('#delAll').click(function(){
	$('#btn').unbind("click");
})
``````
为匿名函数指定一个变量，就快要移除其中一个事件
one()：对于只需要触发一次，随后就要立即解除绑定的情况
one(type,[data],fn)

####模拟操作

``````
$('#btn').trigger("click");
$('#btn').click();//简化写法

//触发自定义事件
$('#btn').bind("myClick",function(){
	$('#test').append("<p></p>")
})
$('#btn').trigger("myClick");
``````
trigger(type,[data]);
type:要触发的事件类型；
[data]：要传递给事件处理函数的附加数据，以数组形式传递
``````
$('#btn').bind("myClick",function(event,message1,message2){
	$('#test').append("<p>"+message1+message2+"</p>");
})
$('#btn').trigger("myClick",["我的自定义","事件")；
``````
触发绑定的特定事件，同时取消对此事件的默认操作
``````
$("input").triggerHandler("focus");


####其他用法
* 绑定多个事件类型
``````
$(function(){
	$("div").bind("mouseover mouseout",function(){
		$(this).toggleClass("over");
	})
})
``````
* 添加事件命名空间，便于管理
``````
$(function(){
	$("div").bind("click.plugin",function(){
		$("body").append("<p>click事件</p>");
	})
	$("div").bind("mouseover.plugin",function(){
		$("body").append("<p>mouseover事件</p>");
	})
	$("div").bind("dblclick",function(){
		$("body").append("<p>dblclick事件</p>");
	})
	$("button").click(function(){
		$("div").unbind(".plugin");
	});
});

* 相同事件名称，不同命名空间执行方法
``````
$(function(){
	$("div").bind("click",function(){
		$("body").append("<p>click事件</p>")
	});
	$("div").bind("click.plugin",function(){
		$("body").append("<p>click.plugin事件</p>")
	});
	$("button").click(function(){
		$("div").trigger("click!")//只触发click事件
	})
})
``````
trigger("click!")！为了匹配所有不包含在命名空间中的click方法


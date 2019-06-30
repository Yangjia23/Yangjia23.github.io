---
title: js面试题——循环绑定事件
date: 2017-12-29 14:48:23
tags:
     - JS
---
一道面试中90%会碰到的题目，废话少说，直接看题
<!-- more -->
### 一.题目
  以下代码实现5个input按钮循环绑定click点击事件，绑定完成后的点击1,2,3,4,5分别弹出0,1,2,3,4五个字符。
  - 下面代码能否实现？说出原因
  - 给出你的解决办法

  
  ```html
  <div id='btnBox'>
  	<input type="button" value="button_1">
  	<input type="button" value="button_2">
  	<input type="button" value="button_3">
  	<input type="button" value="button_4">
  	<input type="button" value="button_5">
  <div>
  <script>
  	var btnBox = document.getElementById("btnBox"),
  	    inputs = btnBox.getElementsByTagName("input");

  	var l = inputs.length;
  	for(var i=0;i<l;++i){
  		inputs[i].onclick = function(){
  			alert(i)
  		}
  	}
  </script>
  ```

### 二.解答
  1.上面代码无法实现要求，每次点击弹出5. [在线运行](http://jsbin.com/beqivez/edit?html,output)
  2.原因:

   - 所有的事件绑定都是异步操作(绑定的时候，点击事件并未执行；点击事件触发的时候，循环绑定事件已经结束)
   - 循环结束后i的值为5,点击事件所绑定的是匿名函数，会形成私有作用域，点击时，私有作用域下没有私有变量i,会到父级作用域查找 i, 此时的全局变量i的值为5.

  3.扩展点:
   - 同步: JS中当前这个任务未完成，下面的任务都不会执行，只有等当前任务彻底任务完成才执行下面的任务；
   - 异步：JS中当前这个任务未完成，需要等一会再完成，此时我们可以继续执行下面的任务；

  4.解决办法:
    - 自定义属性 [在线运行](http://jsbin.com/jeyiwe/edit?html,output)

    ```javascript
  	for(var i=0;i<l;++i){
  	    inputs[i].myIndex = i;
  		inputs[i].onclick = function(){
  			alert(this.myIndex)
  		}
  	}
    ```
	- 闭包1 [在线运行](http://jsbin.com/bijohok/1/edit?html,output)、

	```javascript
	for(var i=0;i<l;++i){
  	  (function(i){
	    inputs[i].onclick = function(){
  		  alert(i)
  	    }
  	  })(i)
  	}
	```
	- 闭包2 [在线运行](http://jsbin.com/wopexu/edit?html,console,output)

	```javascript
  	for(var i=0;i<l;++i){
		inputs[i].onclick = (function(i){
  			return function(){
  				alert(i)
  			}
  		})(i);
  	}
	```
	- ES6,块级作用域 let [在线运行](http://jsbin.com/silomek/edit?html,console,output)

    ```javascript
  	for(let i=0;i<l;++i){
  		inputs[i].onclick = function(){
  			alert(i)
  		}
  	}
    ```



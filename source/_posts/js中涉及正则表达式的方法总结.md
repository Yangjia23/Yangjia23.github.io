---
title: js中涉及正则表达式的方法总结
date: 2017-12-22 10:06:56
tags:
     - JS
---
js中有些方法涉及到正则表达式，总结一下
<!-- more -->
### 一、RegExp 对象

RegExp对象有 test()、exec() 两方法

- **test()方法**
  test()方法接受一个字符串参数，在模式与该参数匹配的情况下返回 true ,否则，返回 false
  ```javascript
  var test = "0556-1234567";
  var pattern = /\d{4}-\d{7}/;
  console.log(pattern(test)); //true
  ```

- **exec()方法** 
  exec()方法是为捕获组设计的，该方法接受一个字符串对象，然后返回包含第一个匹配信息的数组，未匹配成功返回null。
  返回的数组包含俩额外属性:index 和 input

  >index: 匹配项在字符串中的位置
  >input: 应用正则表达式的字符串

  数组中第一项是与整个模式匹配的字符串，其他项是与模式中捕获组匹配的字符串（若模式中没有分组，则数组就包含一项）
  ```javascript
  var text = "Tom is a good student";
  var parrent = /Tom (is a (good student)+)+/;
  var result = parrent.exec(text);

  console.log(result[0]); // "Tom is a good student"
  console.log(result[1]); // "is a good student", 第一个捕获组
  console.log(result[2]); // "good student", 第二个捕获组
  console.log(result.index); //0
  console.log(result.input); //"Tom is a good student"
  ```
  *对于 exec() 方法，即使在模式中设置了全局标志(g),每次也只返回第一个匹配项*

  非全局匹配
  ```javascript
  var text = 'a1b2c3d4';
  var reg1 = /\d/;  //未设置 g
  var matches = reg1.exec(text);

  console.log(matches[0]); // "1"
  console.log(matches.index); //1
  console.log(reg1.lastIndex); // 0

  matches = reg1.exec(text);
  console.log(matches[0]); // "1"
  console.log(matches.index); //1
  console.log(reg1.lastIndex); // 0
  ```

  全局匹配
  ```javascript
  var text = 'a1b2c3d4';
  var reg1 = /\d/g; 
  var matches = reg1.exec(text);

  console.log(matches[0]); // "1"
  console.log(matches.index); //1
  console.log(reg1.lastIndex); // 2

  matches = reg1.exec(text);
  console.log(matches[0]); // "2"
  console.log(matches.index); //3
  console.log(reg1.lastIndex); // 4
  ```
  注意模式的**lastIndex**属性的变化，在非全局模式下始终保持不变；在全局匹配模式下，lastIndex 在每次调用 exec() 后都会增加


### 二.String 对象
   String 对象有 search(),replace(),match(),split() 四个方法涉及正则表达式
  
   - **search() 方法**
     该方法接受唯一的参数：由字符串或 RegExp 对象对象指定的一个正则表达式，返回字符串第一个匹配项的索引，未找到匹配项返回 -1
     ```javascript
     console.log("cat,hat,bat".search(/at/)); //1
     ```
   - **match()方法**
     该方法与 search() 方法一样，接受唯一参数，要么一个正则表达式，要么一个 RegExp 对象

  	 非全局匹配模式下， **str.match(reg)** 与 **reg.exec(str)** 返回结果相同,返回数组包含 俩额外参数*index* 和 *lastIndex*
  	 ```javascript
	   var str = 'a1b2c3d4';
     var reg = /\d/;
     var result = str.match(reg);

     console.log(result[0]);//1
     console.log(result.index); //1
     console.log(reg.lastIndex);//0

     result = str.match(reg);
     console.log(result[0]);  //1
     console.log(result.index); //1
     console.log(reg.lastIndex);//0
  	 ```

  	 全局匹配模式下，返回数组包含**所有匹配项**，且不再有 *index* 和 *lastIndex* 参数
	 ```javascript
	 var str = 'a1b2c3d4';
   var reg = /\d/g;
   var result = str.match(reg);

   console.log(result);   //[1,2,3,4]
   console.log(reg.lastIndex);   //underfined
	 ```
   - **replace()方法**
     该方法接受两个参数： **第一个参数可以是一个字符串 或 一个RegExp 对象；第二个参数可以是一个字符串 或 一个函数**。
     
     第一个参数为字符串，则只会替换第一个子字符串
     ```javascript
     var text = "1b2,2b2,3b2,4b2";
     var result = text.replace("b2","2b");
     console.log(result);  //"12b,2b2,3b2,4b2"
     ```
     想要替换所有，则第一个参数替换为 正则
     ```javascript
     var text = "1b2,2b2,3b2,4b2";
     var result = text.replace(/b2/g,"2b");
     console.log(result);  //"12b,22b,32b,42b"
     ```

     当第二个参数为字符串时，有些特殊的字符序列将代表不一样的文本信息，特殊的 $
     ![](/images/1222_01.png)

     第二个参数还可以是个函数，函数返回的字符串，表示被替换的匹配项

     在只有一个匹配项的情况下，会向函数传递三个参数：**模式的匹配项**、**模式匹配项在字符串中的位置**、**原始字符串**

     在有多个匹配项的情况下，会向函数传递多的参数：**模式的第一匹配项**、**模式的第二匹配项**、**模式的第三匹配项**...**模式匹配项在字符串中的位置**、**原始字符串**



   - **split()方法** 

     split()基于指定的分隔符将一个字符串分割给多个子字符串，并将结果放入一个数组
     ```javascript
     var pattern = / /ig;
     var str = 'This is a Box!,That is a Box too';         
     console.log(str.split(pattern));//将空格拆开分组成数组

     // ["This", "is", "a", "Box!,That", "is", "a", "Box", "too"]
     ```







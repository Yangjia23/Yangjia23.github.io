---
title: js面试题——数组去重
date: 2017-12-29 19:10:04
tags:
     - JS
---
数组去重的几种方法
<!-- more -->
### 1.原始方法
  使用indexOf()方法来去除重复元素 [在线运行](https://jsbin.com/liciqog/edit?html,console)
    ```javascript
    function unique1(arr){
      var backArr = [];
      for(var i=0;i<arr.length;++i){
         var current = arr[i];
         if(backArr.indexOf(current) === -1){
            backArr.push(current)
         }
      };
      return backArr;
    }
    
    var arr = [1,2,2,3,4,5,6,5];
    console.log(unique1(arr));
    ```
  写出这种方法正常不过，考官肯定还得问还有啥别的方法不？

### 2.filter()方法
  使用filter()简化外层循环  [在线运行](https://jsbin.com/cizimub/edit?html,console)
  ```javascript
  function unique2(arr){
      var backArr = arr.filter(function(item,index,array){
        return array.indexOf(item) === index;
      });
      return backArr;
  }
    
  var arr = [1,3,2,2,4,3,1];
  console.log(unique2(arr));
  ```
### 3.Object键值对
  上面两种方法都是定义一个函数，再进行传值，更好的做法是在数组原型上定义方法，这样任何数组都可直接调用 [在线运行](https://jsbin.com/kawimof/edit?html,console)
  ```javascript
  ~function(){
    var apt = Array.prototype;
    apt.unique3 = function unique3(){
       var Obj = {};
       for(var i=0;i<this.length;++i){
          var item = this[i];
          //已有重复值
          if(typeof Obj[item] !== 'undefined'){
            this[i] = this[this.length-1];
            this.length --;
            i--;
            continue;
          }
          Obj[item] = item;
       };
       Obj = null;
       return this;
    }
  }();
  console.log([1,2,3,2,1].unique3());
  ```
### 4.ES6 set数据结构
  ES6 提供新的数据结构Set,类似于数组，但其成员值都是唯一的，没有重复值 [在线运行](https://jsbin.com/loqisu/edit?html,console)
  ```javascript
  function dedupe(array){
    return Array.from(new Set(array));
  }
  console.log(dedupe([1,2,3,2,1]));
  ```


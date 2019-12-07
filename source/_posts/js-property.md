---
title: The Difference Between __proto__ And prototype
date: 2017-02-21 16:03:37
categories: JavaScript #文章文类
tags: [Prototype] #文章标签，多于一项时用这种格式
---
![JavaScript proto chain](/img/js_proto_chain.png)
There are several answers here how to check if a property exists in an object.

I was always using

if(myObj.hasOwnProperty('propName'))
but I wonder if there is any difference from
if('propName' in myObj){

<!--more-->

# An example

``` js
var test = function() {}

test.prototype.newProp = function() {}

var instance = new test();

instance.hasOwnProperty('newProp'); // false
'newProp' in instance // true

```

# another example
``` js
var o  = {"name":"sun nan","university":"Deakin","course":"Bachelor of Information Technology (Programming)-Deakin","email":"417757848@qq.com","first_name":"Nan","last_name":"Sun","date_of_birth":"1993-09-13"}

o.prototype.print = function(){
    for(var x in y){
        console.log(x, y[x]);
    }
}
```
undefined error will be raise



# next example

``` js
//在JavaScript的世界中，所有的函数都能作为构造函数，构造出一个对象
   //下面我给自己构造一个女神做对象
   function NvShen () {
     this.name = "Alice";
   }
   //现在我设置NvShen这个函数的prototype属性
   //一般来说直接用匿名的对象就行，我这里是为了方便理解，
   //先定义一个hand对象再把hand赋值给NvShen的prototype
   var hand = {
     whichOne: "right hand",
     someFunction: function(){
       console.log("not safe for work.");
     }
   };
   NvShen.prototype = hand; 

   //这个时候，我们可以用NvShen作为构造函数，构造出myObject对象
   var myObject = new NvShen();
   console.log(myObject.__proto__ === NvShen.prototype) //true
```
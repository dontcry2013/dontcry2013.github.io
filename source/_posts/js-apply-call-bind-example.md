---
title: Examples Of Understanding JavaScript Apply, Call And Bind
date: 2018-04-16 09:57:24
categories: JavaScript #文章文类
tags: [Example] #文章标签，多于一项时用这种格式
---

# apply, call and bind
They are the embedded method of a function. The following examples show usage to bind this object to a callback function to replace the global this. 

<!--more-->


``` js
var op = {a: 1};
var a = 2;

(function(){
  	console.log(this.a);
})()  // 2

(function(){
    console.log(this.a);
}()) // 2

(function(){
	console.log(this.a)
}.apply(op))

(function(){
  	console.log(this.a)
}).call(op)

(function(){
  	console.log(this.a);
}).bind(op)()  // 1

(function(){
    console.log(this.a);
}.bind(op)()) // 1

(function(arg1, arg2){
  	console.log(this, arg1, arg2)
}).call(op, "ss", "sss")

(function(arg1, arg2){
	console.log(this, arg1, arg2)
}).apply(op, ["ss", "sss"])

setTimeout(function(arg){
	console.log(op, arg)
}.call(op, "ss"), 19, "sss")  // {a: 1} "ss"
```

---
title: js property
date: 2017-02-21 16:03:37
categories: javascript #文章文类
tags: [javascript] #文章标签，多于一项时用这种格式
---

There are several answers here how to check if a property exists in an object.

I was always using

if(myObj.hasOwnProperty('propName'))
but I wonder if there is any difference from
if('propName' in myObj){

<!--more-->

An example

``` js
var test = function() {}

test.prototype.newProp = function() {}

var instance = new test();

instance.hasOwnProperty('newProp'); // false
'newProp' in instance // true

```
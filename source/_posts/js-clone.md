---
title: JavaScript Object Clone
date: 2017-05-04 10:03:37
categories: JavaScript #文章文类
tags: [prototype] #文章标签，多于一项时用这种格式
---

To do clone for any object in JavaScript will not be simple or straightforward. You will run into the problem of erroneously picking up attributes from the object's prototype that should be left in the prototype and not copied to the new instance. If, for instance, you are adding a clone method to Object.prototype, as someone depict, you will need to explicitly skip that attribute. But what if there are other additional methods added to Object.prototype, or other intermediate prototypes, that you don't know about? In that case, you will copy attributes you shouldn't, so you need to detect unforeseen, non-local attributes with the hasOwnProperty method.

<!-- more -->
# the very simple way

``` js
var cloneObj = JSON.parse(JSON.stringify(obj));
```
In this way, it will discard constructor.


# think deep 
``` js
Object.prototype.clone = function () {
    var Constructor = this.constructor;
    var obj = new Constructor();

    for (var attr in this) {
        if (this.hasOwnProperty(attr)) {
            if (typeof(this[attr]) !== "function") {
                if (this[attr] === null) {
                    obj[attr] = null;
                }
                else {
                    obj[attr] = this[attr].clone();
                }
            }
        }
    }
    return obj;
};
```

## some interior object clone

``` js
/* Method of Array */
Array.prototype.clone = function () {
    var thisArr = this.valueOf();
    var newArr = [];
    for (var i=0; i<thisArr.length; i++) {
        newArr.push(thisArr[i].clone());
    }
    return newArr;
};

/* Method of Boolean, Number, String*/
Boolean.prototype.clone = function() { return this.valueOf(); };
Number.prototype.clone = function() { return this.valueOf(); };
String.prototype.clone = function() { return this.valueOf(); };

/* Method of Date*/
Date.prototype.clone = function() { return new Date(this.valueOf()); };

/* Method of RegExp*/
RegExp.prototype.clone = function() {
    var pattern = this.valueOf();
    var flags = '';
    flags += pattern.global ? 'g' : '';
    flags += pattern.ignoreCase ? 'i' : '';
    flags += pattern.multiline ? 'm' : '';
    return new RegExp(pattern.source, flags);
};
```

# some test

``` js
[1]==[1]
false
[1]===[1]
false

var j = new Object();
j.a = 11
j.__proto__.print = function(){
	for(var i in j){
		console.log(i, j[i]);
	}
}

j.print()
//输出
a 11
print function (){
	for(var i in j){
		console.log(i, j[i]);
	}
}

for(var ii in j){
	if(j.hasOwnProperty(ii)){
		console.log(ii, j[ii]);
	}
}
//输出
a 11

```
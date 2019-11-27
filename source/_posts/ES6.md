---
title: ES6
date: 2016-11-24 14:40:02
categories: JavaScript #文章文类
tags: [ES6] #文章标签，多于一项时用这种格式
---
# 函数参数的默认值 
在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法。
``` js
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello World
```
<!--more-->
缺点在于，如果参数y为false，则该赋值不起作用。
为了避免这个问题，通常需要先判断一下参数y是否被赋值，如果没有，再等于默认值。
``` js
if (typeof y === 'undefined') {
  y = 'World';
}
```

ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。
``` js
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

# rest参数
rest参数中的变量代表一个数组，所以数组特有的方法都可以用于这个变量。下面是一个利用rest参数改写数组push方法的例子。
``` js
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
```
注意，rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
函数的length属性，不包括rest参数。
``` js
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```


# 扩展运算符
扩展运算符（spread）是三个点（...）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。
``` js
function add(x, y) {
  return x + y;
}

var numbers = [4, 38];
add(...numbers) // 42
```

# 替代数组的apply方法 
``` js
// ES5的写法
Math.max.apply(null, [14, 3, 77])

// ES6的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

``` js
> var a = [1,2]
undefined
> var b = [3,4]
undefined
> a.push(b)
3
> a
[ 1, 2, [ 3, 4 ] ]
> a.push(...b)
5
> a
[ 1, 2, [ 3, 4 ], 3, 4 ]


// ES5的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);
```
# 扩展运算符的应用
##（1）合并数组
``` js
// ES5
[1, 2].concat(more)
// ES6
[1, 2, ...more]
```

##（2）字符串
扩展运算符还可以将字符串转为真正的数组。
``` js
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

# 其他
```js
foo::bar;
// 等同于
bar.bind(foo);
```
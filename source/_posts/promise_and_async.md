---
title: promise and async
date: 2016-11-24 15:20:02
categories: javascript #文章文类
tags: [javascript, es6] #文章标签，多于一项时用这种格式
---
# Promise对象的简单例子。
```js
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```
<!--more-->

``` js
async muiltiPromise(){
  var ret = await AsyncStorage.getItem('access_token')
  .then(token=>{
    console.warn(token);
    var config = {
      token:token
    };
    config.add = 'add something';
    return config;
  })
  .then(v=>{
    console.warn(JSON.stringify(v));
    v.addMore = 'add another thing';
    return v;
  });
  console.warn(JSON.stringify(ret));
}

muiltiPromise2(){
  var ret = AsyncStorage.getItem('access_token')
  .then(token=>{
    console.warn(token);
    var config = {
      token:token
    };
    config.add = 'add something';
    return config;
  })
  .then(v=>{
    console.warn(JSON.stringify(v));
    v.addMore = 'add another thing';
    return v;
  });
  
  ret.then(v => console.warn(JSON.stringify(v))).catch();
}
```
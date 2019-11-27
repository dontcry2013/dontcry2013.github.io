---
title: Promise and Async
date: 2016-11-24 15:20:02
categories: JavaScript #文章文类
tags: [ES6, ES2017] #文章标签，多于一项时用这种格式
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

multiPromise2(){
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

# Promise的一个错误的例子。
``` js
resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

testPromise(){
  var x = this.resolveAfter2Seconds(1)
          .then(v=>{
            console.log("内部内部的testPromise是：", v);         
            return v;
          }); 
  console.log("内部的testPromise是：", x);  //此时是promise对象
  return x;
}

var tPromise = this.testPromise();
console.log("tPromise是：", tPromise);  //此时依然是promise对象

```


# 正确写法
``` js
class Test{
  constructor(props) {
    super(props);
    this.testPromise(60).then(v => {
      console.log("tPromise是：", tPromise);  // prints 60 after 2 seconds.
    });

    console.log("another thing happens");
  } 

  resolveAfter2Seconds(x) {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(x);
      }, 2000);
    });
  }

  testPromise(arg){
    var x = this.resolveAfter2Seconds(arg)
            .then(v=>{
              console.log("内部内部的testPromise是：", v);         
              return v;
            }); 
    console.log("内部的testPromise是：", x);  //此时是promise对象
    return x;
  }
}

```
你可以无限制的then下去，但是最终结果的依然是在then中处理。
"another thing happens"是最先输出的，promise没有阻塞代码块的顺序执行。

# async的一些不同
``` js
resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async testPromise(arg){
  var x = await this.resolveAfter2Seconds(arg);
  console.log("内部的testPromise是：", x);  //此时x的值确是arg
  return x;  //此时返回的是promise对象
}

this.testPromise(60).then(v => {
  console.log("tPromise是：", tPromise);  // 依然要这样处理返回值
});

```
总结：处理返回值要在then中。
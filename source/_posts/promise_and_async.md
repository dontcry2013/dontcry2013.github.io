---
title: Promise and Async
date: 2016-11-24 15:20:02
categories: JavaScript
tags: [ES6, ES2017]
---
# Promise Simple Example
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

# A **WRONG** Promise Example
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
    console.log("1:", v);         
    return v;
  }); 
  console.log("11:", x);  // it is a promise object
  return x;
}

var tPromise = this.testPromise();
console.log("tPromise is:", tPromise);  // still is a promise object
```


# Correct Example
``` js
class Test{
  constructor(props) {
    super(props);
    this.testPromise(60).then(v => {
      console.log("tPromise is:", tPromise);  // prints 60 after 2 seconds.
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
      console.log("1:", v);         
      return v;
    }); 
    console.log("11:", x);  // promise object
    return x;
  }
}

```
You can write infinitive then functions, but in the end, the final result need to be solved in then function.

"another thing happens" is printed firstly, promise does not block the function to execute in sequence.

# async
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
  console.log("testPromise is:", x);
  return x;  // promise object
}

this.testPromise(60).then(v => {
  console.log("tPromise is:", tPromise);
});

```

# Conclusion
In order to handle the return value, we still need to do it in then function.
---
title: Sequential Execution Queue
date: 2018-05-01 09:48:28
categories: JavaScript #文章文类
tags: [Node.js, ES6, ES2017] #文章标签，多于一项时用这种格式
---
There are a lot of scenarios that we want a queue executed sequentially, like we have a list of video url, and we want to download all of the video, after finish one we have to check the integrity, start next job if it is OK. Normally, we have three methods to do that.
<!--more-->
# Recursion
``` js
var taskList = [1,2,3,4]
var downloadList = function(){
  if(taskList.length > 0){
    var task = taskList.shift();
    console.log(`dealing with ${task}`);
    (function(fun){
    	setTimeout(()=>{
	    	fun();
	    }, 1000)	
    }(downloadList));
  } else{
	console.log(`taskList is empty`);
  }
}
downloadList();
```

# While
``` js
var events = require('events');
var eventEmitter = new events.EventEmitter();
let bool = true;
var taskList = [1,2,3,4,5,6]
while(taskList.length){
	if(bool){
		console.log(1, bool)
		bool = false;
		var concur1 = taskList.shift();
		var concur2 = taskList.shift();
		console.log(`dealing with ${concur1} and ${concur2}`);
		(function(param){
			setTimeout(()=>{
				console.log(2, param)
				param = true;
		    }, 1000)
		}(bool))

		while(Date.now() - last> 1000 && taskList.length){
			eventEmitter.emit("start");
		}
	} else{
		console.log(3, bool)
	}
}


```

# Event
``` js
var events = require('events');
var eventEmitter = new events.EventEmitter();
var taskList = [1,2,3,4,5,6]

eventEmitter.emit("start");

eventEmitter.on("start", function(){
	var concur1 = taskList.shift();
	var concur2 = taskList.shift();
	console.log(`dealing with ${concur1} and ${concur2}`);
	while(Date.now() - last> 1000 && taskList.length){
		eventEmitter.emit("start");
	}
})
```

# ES2017 async
``` js
var sleep = function (time) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve('ok');
        }, time);
    })
};

(async function () {
    for (var i = 1; i <= 10; i++) {
        console.log(`dealing with ${i}`);
        await sleep(1000);
    }
}())
```


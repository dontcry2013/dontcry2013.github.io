---
title: Jsonp Tricky Trap
date: 2016-11-28 17:10:12
categories: JavaScript #文章文类
tags: [jQuery, Issue] #文章标签，多于一项时用这种格式
---
在使用polyv的web上传插件时遇到语言问题，官方插件是中文没有切换英文的借口。想到保存到本地修改后使用。
幸好提供了jsonp借口，但是在调用上踩了一下坑。
<!--more-->
![polyv api](/img/polyvapi.png)
# 问题描述

Uncaught ReferenceError: jsonp is not defined

网上的解决办法：[jQuery .ajax() calls failing](http://stackoverflow.com/questions/8673050/jquery-ajax-calls-failing)
然而并没有什么卵用。

# 真正的解决办法
当没有指定jsonpCallback，默认是执行名为callback的函数，然而服务器返回的是形如：jsonp([{}])的命名（其实也是我指定的命名）。
所以jsonpCallback字段要对应。

``` js
	$.ajax({
	    url: "http://" + polyvVideo.video_domain + "/uc/services/rest",
	    data: {
			"method": "getNewList",
			"readtoken":polyvVideo.readToken,
			"pageNum": num,
			"jsonp":"jsonp"
		},
	    dataType: "jsonp",
	    jsonpCallback: 'jsonp',
	    success: function(json) {
	        console.log(json);
	        if(!json.data){
				$("[data-pagenum='next']").hide();
			}else{
				$("[data-pagenum='next']").show();
			}
			polyvVideo.showVideo(json);
	    }
	});
```

# 官方解读
http://api.jquery.com/jquery.ajax/

If jsonp is specified, \$.ajax() will automatically append a query string parameter of (by default) callback=? to the URL. The jsonp and jsonpCallback properties of the settings passed to \$.ajax() can be used to specify, respectively, the name of the query string parameter and the name of the JSONP callback function. The server should return valid JavaScript that passes the JSON response into the callback function. \$.ajax() will execute the returned JavaScript, calling the JSONP callback function, before passing the JSON object contained in the response to the \$.ajax() success handler.
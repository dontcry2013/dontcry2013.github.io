---
title: Self-Executing Anonymous Functions
date: 2016-11-28 17:10:12
categories: JavaScript #文章文类
tags: [ES5, TODO] #文章标签，多于一项时用这种格式
---
自执行函数写法很诡异，mark一下，需要学习。
<!--more-->
# jquery 的自执行函数

``` js
(function($) {
  'use strict';

  $(function() {
    var $fullText = $('.admin-fullText');
    $('#admin-fullscreen').on('click', function() {
      $.AMUI.fullscreen.toggle();
    });

    $(document).on($.AMUI.fullscreen.raw.fullscreenchange, function() {
      $.AMUI.fullscreen.isFullscreen ? $fullText.text('关闭全屏') : $fullText.text('开启全屏');
    });
  });
})(jQuery);
```
[self executing function jquery vs JavaScript difference](http://stackoverflow.com/questions/19491650/self-executing-function-jquery-vs-javascript-difference)


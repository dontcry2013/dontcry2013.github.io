---
title: Tampermonkey Script
date: 2017-03-31 11:48:34
categories: [Tools, JavaScript] #文章文类
tags: [jQuery] #文章标签，多于一项时用这种格式
---

Using tampermonkey to fill studylink automatically which used a lot of jquery api, this is a good backup to remind the usage of  a practical scenario.
<!--more-->

``` js

// ==UserScript==
// @name         Studylink Userscript
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://customer.studylink.com/*
// @require     http://ss.aemg.com.au/theme/Officer/js/chosen.jquery.js
// @grant       GM_xmlhttpRequest
// @grant       GM_addStyle
// @grant       GM_getResourceText
// @run-at      document-end
// ==/UserScript==


//var jqChosen_CssSrc = GM_getResourceText ("jqChosen_CSS");
GM_addStyle ('#myFixedDiv {position:fixed;height:auto;width:250px;background-color: khaki;bottom:0;right:0;padding:10px;transition: transform 2s;}.tips-trans {transform: translate(80%);}.chosen-container{position:relative;display:inline-block;vertical-align:middle;font-size:13px;zoom:1;*display:inline;-webkit-user-select:none;-moz-user-select:none;user-select:none}.chosen-container .chosen-drop{position:absolute;top:100%;left:-9999px;z-index:1010;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;width:100%;border:1px solid #aaa;border-top:0;background:#fff;box-shadow:0 4px 5px rgba(0,0,0,.15)}.chosen-container.chosen-with-drop .chosen-drop{left:0}.chosen-container a{cursor:pointer}.chosen-container-single .chosen-single{position:relative;display:block;overflow:hidden;padding:0 0 0 8px;height:23px;border:1px solid #aaa;border-radius:5px;background-color:#fff;background:-webkit-gradient(linear,50% 0,50% 100%,color-stop(20%,#fff),color-stop(50%,#f6f6f6),color-stop(52%,#eee),color-stop(100%,#f4f4f4));background:-webkit-linear-gradient(top,#fff 20%,#f6f6f6 50%,#eee 52%,#f4f4f4 100%);background:-moz-linear-gradient(top,#fff 20%,#f6f6f6 50%,#eee 52%,#f4f4f4 100%);background:-o-linear-gradient(top,#fff 20%,#f6f6f6 50%,#eee 52%,#f4f4f4 100%);background:linear-gradient(top,#fff 20%,#f6f6f6 50%,#eee 52%,#f4f4f4 100%);background-clip:padding-box;box-shadow:0 0 3px #fff inset,0 1px 1px rgba(0,0,0,.1);color:#444;text-decoration:none;white-space:nowrap;line-height:24px}.chosen-container-single .chosen-default{color:#999}.chosen-container-single .chosen-single span{display:block;overflow:hidden;margin-right:26px;text-overflow:ellipsis;white-space:nowrap}.chosen-container-single .chosen-single-with-deselect span{margin-right:38px}.chosen-container-single .chosen-single abbr{position:absolute;top:6px;right:26px;display:block;width:12px;height:12px;background:url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) -42px 1px no-repeat;font-size:1px}.chosen-container-single .chosen-single abbr:hover{background-position:-42px -10px}.chosen-container-single.chosen-disabled .chosen-single abbr:hover{background-position:-42px -10px}.chosen-container-single .chosen-single div{position:absolute;top:0;right:0;display:block;width:18px;height:100%}.chosen-container-single .chosen-single div b{display:block;width:100%;height:100%;background:url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) no-repeat 0 2px}.chosen-container-single .chosen-search{position:relative;z-index:1010;margin:0;padding:3px 4px;white-space:nowrap}.chosen-container-single .chosen-search input[type=text]{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;margin:1px 0;padding:4px 20px 4px 5px;width:100%;height:auto;outline:0;border:1px solid #aaa;background:#fff url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) no-repeat 100% -20px;background:url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) no-repeat 100% -20px;font-size:1em;font-family:sans-serif;line-height:normal;border-radius:0}.chosen-container-single .chosen-drop{margin-top:-1px;border-radius:0 0 4px 4px;background-clip:padding-box}.chosen-container-single.chosen-container-single-nosearch .chosen-search{position:absolute;left:-9999px}.chosen-container .chosen-results{position:relative;overflow-x:hidden;overflow-y:auto;margin:0 4px 4px 0;padding:0 0 0 4px;max-height:240px;-webkit-overflow-scrolling:touch}.chosen-container .chosen-results li{display:none;margin:0;padding:5px 6px;list-style:none;line-height:15px;-webkit-touch-callout:none}.chosen-container .chosen-results li.active-result{display:list-item;cursor:pointer}.chosen-container .chosen-results li.disabled-result{display:list-item;color:#ccc;cursor:default}.chosen-container .chosen-results li.highlighted{background-color:#3875d7;background-image:-webkit-gradient(linear,50% 0,50% 100%,color-stop(20%,#3875d7),color-stop(90%,#2a62bc));background-image:-webkit-linear-gradient(#3875d7 20%,#2a62bc 90%);background-image:-moz-linear-gradient(#3875d7 20%,#2a62bc 90%);background-image:-o-linear-gradient(#3875d7 20%,#2a62bc 90%);background-image:linear-gradient(#3875d7 20%,#2a62bc 90%);color:#fff}.chosen-container .chosen-results li.no-results{display:list-item;background:#f4f4f4}.chosen-container .chosen-results li.group-result{display:list-item;font-weight:700;cursor:default}.chosen-container .chosen-results li.group-option{padding-left:15px}.chosen-container .chosen-results li em{font-style:normal;text-decoration:underline}.chosen-container-multi .chosen-choices{position:relative;overflow:hidden;-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;margin:0;padding:0;width:100%;height:auto!important;height:1%;border:1px solid #aaa;background-color:#fff;background-image:-webkit-gradient(linear,50% 0,50% 100%,color-stop(1%,#eee),color-stop(15%,#fff));background-image:-webkit-linear-gradient(#eee 1%,#fff 15%);background-image:-moz-linear-gradient(#eee 1%,#fff 15%);background-image:-o-linear-gradient(#eee 1%,#fff 15%);background-image:linear-gradient(#eee 1%,#fff 15%);cursor:text}.chosen-container-multi .chosen-choices li{float:left;list-style:none}.chosen-container-multi .chosen-choices li.search-field{margin:0;padding:0;white-space:nowrap}.chosen-container-multi .chosen-choices li.search-field input[type=text]{margin:1px 0;padding:5px;height:15px;outline:0;border:0!important;background:transparent!important;box-shadow:none;color:#666;font-size:100%;font-family:sans-serif;line-height:normal;border-radius:0}.chosen-container-multi .chosen-choices li.search-field .default{color:#999}.chosen-container-multi .chosen-choices li.search-choice{position:relative;margin:3px 0 3px 5px;padding:3px 20px 3px 5px;border:1px solid #aaa;border-radius:3px;background-color:#e4e4e4;background-image:-webkit-gradient(linear,50% 0,50% 100%,color-stop(20%,#f4f4f4),color-stop(50%,#f0f0f0),color-stop(52%,#e8e8e8),color-stop(100%,#eee));background-image:-webkit-linear-gradient(#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:-moz-linear-gradient(#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:-o-linear-gradient(#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:linear-gradient(#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-clip:padding-box;box-shadow:0 0 2px #fff inset,0 1px 0 rgba(0,0,0,.05);color:#333;line-height:13px;cursor:default}.chosen-container-multi .chosen-choices li.search-choice .search-choice-close{position:absolute;top:4px;right:3px;display:block;width:12px;height:12px;background:url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) -42px 1px no-repeat;font-size:1px}.chosen-container-multi .chosen-choices li.search-choice .search-choice-close:hover{background-position:-42px -10px}.chosen-container-multi .chosen-choices li.search-choice-disabled{padding-right:5px;border:1px solid #ccc;background-color:#e4e4e4;background-image:-webkit-gradient(linear,50% 0,50% 100%,color-stop(20%,#f4f4f4),color-stop(50%,#f0f0f0),color-stop(52%,#e8e8e8),color-stop(100%,#eee));background-image:-webkit-linear-gradient(top,#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:-moz-linear-gradient(top,#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:-o-linear-gradient(top,#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);background-image:linear-gradient(top,#f4f4f4 20%,#f0f0f0 50%,#e8e8e8 52%,#eee 100%);color:#666}.chosen-container-multi .chosen-choices li.search-choice-focus{background:#d4d4d4}.chosen-container-multi .chosen-choices li.search-choice-focus .search-choice-close{background-position:-42px -10px}.chosen-container-multi .chosen-results{margin:0;padding:0}.chosen-container-multi .chosen-drop .result-selected{display:list-item;color:#ccc;cursor:default}.chosen-container-active .chosen-single{border:1px solid #5897fb;box-shadow:0 0 5px rgba(0,0,0,.3)}.chosen-container-active.chosen-with-drop .chosen-single{border:1px solid #aaa;-moz-border-radius-bottomright:0;border-bottom-right-radius:0;-moz-border-radius-bottomleft:0;border-bottom-left-radius:0;background-image:-webkit-gradient(linear,50% 0,50% 100%,color-stop(20%,#eee),color-stop(80%,#fff));background-image:-webkit-linear-gradient(#eee 20%,#fff 80%);background-image:-moz-linear-gradient(#eee 20%,#fff 80%);background-image:-o-linear-gradient(#eee 20%,#fff 80%);background-image:linear-gradient(#eee 20%,#fff 80%);box-shadow:0 1px 0 #fff inset}.chosen-container-active.chosen-with-drop .chosen-single div{border-left:0;background:transparent}.chosen-container-active.chosen-with-drop .chosen-single div b{background-position:-18px 2px}.chosen-container-active .chosen-choices{border:1px solid #5897fb;box-shadow:0 0 5px rgba(0,0,0,.3)}.chosen-container-active .chosen-choices li.search-field input[type=text]{color:#111!important}.chosen-disabled{opacity:.5!important;cursor:default}.chosen-disabled .chosen-single{cursor:default}.chosen-disabled .chosen-choices .search-choice .search-choice-close{cursor:default}.chosen-rtl{text-align:right}.chosen-rtl .chosen-single{overflow:visible;padding:0 8px 0 0}.chosen-rtl .chosen-single span{margin-right:0;margin-left:26px;direction:rtl}.chosen-rtl .chosen-single-with-deselect span{margin-left:38px}.chosen-rtl .chosen-single div{right:auto;left:3px}.chosen-rtl .chosen-single abbr{right:auto;left:26px}.chosen-rtl .chosen-choices li{float:right}.chosen-rtl .chosen-choices li.search-field input[type=text]{direction:rtl}.chosen-rtl .chosen-choices li.search-choice{margin:3px 5px 3px 0;padding:3px 5px 3px 19px}.chosen-rtl .chosen-choices li.search-choice .search-choice-close{right:auto;left:4px}.chosen-rtl.chosen-container-single-nosearch .chosen-search,.chosen-rtl .chosen-drop{left:9999px}.chosen-rtl.chosen-container-single .chosen-results{margin:0 0 4px 4px;padding:0 4px 0 0}.chosen-rtl .chosen-results li.group-option{padding-right:15px;padding-left:0}.chosen-rtl.chosen-container-active.chosen-with-drop .chosen-single div{border-right:0}.chosen-rtl .chosen-search input[type=text]{padding:4px 5px 4px 20px;background:#fff url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) no-repeat -30px -20px;background:url(http://ss.aemg.com.au/theme/Officer/css/chosen-sprite.png) no-repeat -30px -20px;direction:rtl}.chosen-rtl.chosen-container-single .chosen-single div b{background-position:6px 2px}.chosen-rtl.chosen-container-single.chosen-with-drop .chosen-single div b{background-position:-12px 2px}@media only screen and (-webkit-min-device-pixel-ratio:2),only screen and (min-resolution:144dpi){.chosen-rtl .chosen-search input[type=text],.chosen-container-single .chosen-single abbr,.chosen-container-single .chosen-single div b,.chosen-container-single .chosen-search input[type=text],.chosen-container-multi .chosen-choices .search-choice .search-choice-close,.chosen-container .chosen-results-scroll-down span,.chosen-container .chosen-results-scroll-up span{background-image:url(chosen-sprite@2x.png)!important;background-size:52px 37px!important;background-repeat:no-repeat!important}}');

var hex_chr = "0123456789abcdef";
function rhex(num)
{
  str = "";
  for(j = 0; j <= 3; j++)
    str += hex_chr.charAt((num >> (j * 8 + 4)) & 0x0F) +
           hex_chr.charAt((num >> (j * 8)) & 0x0F);
  return str;
}

/*
 * Convert a string to a sequence of 16-word blocks, stored as an array.
 * Append padding bits and the length, as described in the MD5 standard.
 */
function str2blks_MD5(str)
{
  nblk = ((str.length + 8) >> 6) + 1;
  blks = new Array(nblk * 16);
  for(i = 0; i < nblk * 16; i++) blks[i] = 0;
  for(i = 0; i < str.length; i++)
    blks[i >> 2] |= str.charCodeAt(i) << ((i % 4) * 8);
  blks[i >> 2] |= 0x80 << ((i % 4) * 8);
  blks[nblk * 16 - 2] = str.length * 8;
  return blks;
}

/*
 * Add integers, wrapping at 2^32. This uses 16-bit operations internally
 * to work around bugs in some JS interpreters.
 */
function add(x, y)
{
  var lsw = (x & 0xFFFF) + (y & 0xFFFF);
  var msw = (x >> 16) + (y >> 16) + (lsw >> 16);
  return (msw << 16) | (lsw & 0xFFFF);
}

/*
 * Bitwise rotate a 32-bit number to the left
 */
function rol(num, cnt)
{
  return (num << cnt) | (num >>> (32 - cnt));
}

/*
 * These functions implement the basic operation for each round of the
 * algorithm.
 */
function cmn(q, a, b, x, s, t)
{
  return add(rol(add(add(a, q), add(x, t)), s), b);
}
function ff(a, b, c, d, x, s, t)
{
  return cmn((b & c) | ((~b) & d), a, b, x, s, t);
}
function gg(a, b, c, d, x, s, t)
{
  return cmn((b & d) | (c & (~d)), a, b, x, s, t);
}
function hh(a, b, c, d, x, s, t)
{
  return cmn(b ^ c ^ d, a, b, x, s, t);
}
function ii(a, b, c, d, x, s, t)
{
  return cmn(c ^ (b | (~d)), a, b, x, s, t);
}

/*
 * Take a string and return the hex representation of its MD5.
 */
function calcMD5(str)
{
  x = str2blks_MD5(str);
  a =  1732584193;
  b = -271733879;
  c = -1732584194;
  d =  271733878;

  for(i = 0; i < x.length; i += 16)
  {
    olda = a;
    oldb = b;
    oldc = c;
    oldd = d;

    a = ff(a, b, c, d, x[i+ 0], 7 , -680876936);
    d = ff(d, a, b, c, x[i+ 1], 12, -389564586);
    c = ff(c, d, a, b, x[i+ 2], 17,  606105819);
    b = ff(b, c, d, a, x[i+ 3], 22, -1044525330);
    a = ff(a, b, c, d, x[i+ 4], 7 , -176418897);
    d = ff(d, a, b, c, x[i+ 5], 12,  1200080426);
    c = ff(c, d, a, b, x[i+ 6], 17, -1473231341);
    b = ff(b, c, d, a, x[i+ 7], 22, -45705983);
    a = ff(a, b, c, d, x[i+ 8], 7 ,  1770035416);
    d = ff(d, a, b, c, x[i+ 9], 12, -1958414417);
    c = ff(c, d, a, b, x[i+10], 17, -42063);
    b = ff(b, c, d, a, x[i+11], 22, -1990404162);
    a = ff(a, b, c, d, x[i+12], 7 ,  1804603682);
    d = ff(d, a, b, c, x[i+13], 12, -40341101);
    c = ff(c, d, a, b, x[i+14], 17, -1502002290);
    b = ff(b, c, d, a, x[i+15], 22,  1236535329);

    a = gg(a, b, c, d, x[i+ 1], 5 , -165796510);
    d = gg(d, a, b, c, x[i+ 6], 9 , -1069501632);
    c = gg(c, d, a, b, x[i+11], 14,  643717713);
    b = gg(b, c, d, a, x[i+ 0], 20, -373897302);
    a = gg(a, b, c, d, x[i+ 5], 5 , -701558691);
    d = gg(d, a, b, c, x[i+10], 9 ,  38016083);
    c = gg(c, d, a, b, x[i+15], 14, -660478335);
    b = gg(b, c, d, a, x[i+ 4], 20, -405537848);
    a = gg(a, b, c, d, x[i+ 9], 5 ,  568446438);
    d = gg(d, a, b, c, x[i+14], 9 , -1019803690);
    c = gg(c, d, a, b, x[i+ 3], 14, -187363961);
    b = gg(b, c, d, a, x[i+ 8], 20,  1163531501);
    a = gg(a, b, c, d, x[i+13], 5 , -1444681467);
    d = gg(d, a, b, c, x[i+ 2], 9 , -51403784);
    c = gg(c, d, a, b, x[i+ 7], 14,  1735328473);
    b = gg(b, c, d, a, x[i+12], 20, -1926607734);

    a = hh(a, b, c, d, x[i+ 5], 4 , -378558);
    d = hh(d, a, b, c, x[i+ 8], 11, -2022574463);
    c = hh(c, d, a, b, x[i+11], 16,  1839030562);
    b = hh(b, c, d, a, x[i+14], 23, -35309556);
    a = hh(a, b, c, d, x[i+ 1], 4 , -1530992060);
    d = hh(d, a, b, c, x[i+ 4], 11,  1272893353);
    c = hh(c, d, a, b, x[i+ 7], 16, -155497632);
    b = hh(b, c, d, a, x[i+10], 23, -1094730640);
    a = hh(a, b, c, d, x[i+13], 4 ,  681279174);
    d = hh(d, a, b, c, x[i+ 0], 11, -358537222);
    c = hh(c, d, a, b, x[i+ 3], 16, -722521979);
    b = hh(b, c, d, a, x[i+ 6], 23,  76029189);
    a = hh(a, b, c, d, x[i+ 9], 4 , -640364487);
    d = hh(d, a, b, c, x[i+12], 11, -421815835);
    c = hh(c, d, a, b, x[i+15], 16,  530742520);
    b = hh(b, c, d, a, x[i+ 2], 23, -995338651);

    a = ii(a, b, c, d, x[i+ 0], 6 , -198630844);
    d = ii(d, a, b, c, x[i+ 7], 10,  1126891415);
    c = ii(c, d, a, b, x[i+14], 15, -1416354905);
    b = ii(b, c, d, a, x[i+ 5], 21, -57434055);
    a = ii(a, b, c, d, x[i+12], 6 ,  1700485571);
    d = ii(d, a, b, c, x[i+ 3], 10, -1894986606);
    c = ii(c, d, a, b, x[i+10], 15, -1051523);
    b = ii(b, c, d, a, x[i+ 1], 21, -2054922799);
    a = ii(a, b, c, d, x[i+ 8], 6 ,  1873313359);
    d = ii(d, a, b, c, x[i+15], 10, -30611744);
    c = ii(c, d, a, b, x[i+ 6], 15, -1560198380);
    b = ii(b, c, d, a, x[i+13], 21,  1309151649);
    a = ii(a, b, c, d, x[i+ 4], 6 , -145523070);
    d = ii(d, a, b, c, x[i+11], 10, -1120210379);
    c = ii(c, d, a, b, x[i+ 2], 15,  718787259);
    b = ii(b, c, d, a, x[i+ 9], 21, -343485551);

    a = add(a, olda);
    b = add(b, oldb);
    c = add(c, oldc);
    d = add(d, oldd);
  }
  return rhex(a) + rhex(b) + rhex(c) + rhex(d);
}

function strencode(string) {
    var key = calcMD5('aemgmelbourne');
    var str = atob(string);
    var len = key.length;
    var code = '';
    for (var i = 0; i < str.length; i++) {
        var k = i % len;
        code += String.fromCharCode(str.charCodeAt(i) ^ key.charCodeAt(k));
    }
    return atob(code);
}

function addTips(){
    $("body").after("<div id='myFixedDiv'>\
                <div id='myTips' style='text-align: center; font-weight:bold'>Script Tips</div>\
            </div>");
    $("#myFixedDiv").bind("click", function(){
      $(this).toggleClass("tips-trans");
    })
}
function fillInTips(myInfo){
     //提示信息
    if(myInfo){
        console.log("print tips");
        $("#myFixedDiv").append("<div id='myAddedDiv0' style='border-bottom: 1px solid pink'></div>");
        $("#myFixedDiv").append("<div id='myAddedDiv1' />");
        $("#myFixedDiv").append("<div id='myAddedDiv2' style='border-bottom: 1px solid pink'></div>");
        $("#myFixedDiv").append("<div id='myAddedDiv3' />");
        $("#myFixedDiv").append("<div id='myAddedDiv4' />");
        $("#myAddedDiv0").html('<b>'+myInfo.PersonalDetails.first_name + " " + myInfo.PersonalDetails.last_name + "</b> apply for " + myInfo.CourseApplicationMain.university);
        $("#myAddedDiv1").html("<b>course: </b>" + myInfo.CourseApplicationLanguage.course_name);
        $("#myAddedDiv2").html("<b>start date: </b>" + myInfo.CourseApplicationLanguage.start_date);
        $("#myAddedDiv3").html("<b>course: </b>" + myInfo.CourseApplicationMain.course_name);
        $("#myAddedDiv4").html("<b>start date: </b>" + myInfo.CourseApplicationMain.start_date);
    } else{
       console.log("will not print tips");
    }
}
(function($) {
    $(function(){
        function getURLParameter(name) {
            return (RegExp(name + '=' + '(.+?)(&|$)').exec(location.search)||[,null])[1];
        }
        function getCharMonth(i){
            var monthNames = ["January", "February", "March", "April", "May", "June",
              "July", "August", "September", "October", "November", "December"
            ];
            return monthNames[i];
        }
        function queryApplication(opt){
            //var url = 'http://10.0.0.40/ss/Students/course_application_list';
            var url = 'http://cloudservice.au-edu.com/service_staging/Students/course_application_list';
            GM_xmlhttpRequest({
                "method": 'GET',
                "url": url,
                "headers": {"Content-Type": "application/x-www-form-urlencoded"},
                "onload": function(response) {
//                    console.log("response:", response);
                    var html=response.responseText;
//                    console.log("html:", html);
                    if(html){
                        var ret = strencode(html);
//                        console.log("ret:", ret);
                        try{
                            var mJson = JSON.parse(ret);
                            $(".chosen-select option").remove();
                            $(".chosen-select").append("<option value=''> -- select --</option>");
                            for(var i = 0; i < mJson.length; i++){
                                var mItem = mJson[i];
                                var mOption = $("<option>", { value: i, text : mItem.name + " " + mItem.email + " " + mItem.first_name + " " + mItem.last_name + " " + mItem.university});
                                $(".chosen-select").append(mOption);
                            }
                            $(".chosen-select").chosen({width:"100%"});
                            $(".chosen-select").change(function(event){
                                var mId = this.value;
                                var ii = mJson[mId];
                                if(ii){
                                    var dob = ii.date_of_birth.split("-");
                                    var mon = parseInt(dob[1])-1;
                                    var dobMonth = getCharMonth(mon);
                                    $("table input[name=st_email]").val(ii.email);
                                    $("table input[name=st_firstname]").val(ii.first_name);
                                    $("table input[name=st_lastname]").val(ii.last_name);
                                    $("table select[name=st_country] option:contains(CHINA)").prop("selected", true);
                                    $("table select[name=st_dob_day]").val(parseInt(dob[2]));
                                    $("table select[name=st_dob_month]").val(dobMonth);
                                    $("table select[name=st_dob_year]").val(dob[0]);
                                    sessionStorage.setItem(ii.email, JSON.stringify(ii));
                                    $("#simplemodal-overlay, #simplemodal-container").hide();
                                } else{
                                    console.log(mId, "not exist");
                                }
                            });

                            console.log(mJson);
                        } catch(e){
                            console.error(e);
                        }
                    }
                }
            });
        }
        function get_course_application_info(email, cb){
//             var url = "http://10.0.0.40/ss/Students/get_course_application_info/" + email;
             var url = "http://cloudservice.au-edu.com/service_staging/Students/get_course_application_info/" + email;
             console.log("url", url);
             GM_xmlhttpRequest({
                "method": 'GET',
                "url": url,
                "headers": {"Content-Type": "application/x-www-form-urlencoded"},
                "onload": function(response) {
                    var html=response.responseText;
                    if(html){
                        var ret = strencode(html);
                        try{
                            var myInfo = JSON.parse(ret);
                            console.log(myInfo);
                            cb(myInfo);
                        } catch(e){
                            console.error(e);
                        }
                    }
                },
                onerror: function(response) {
                    console.error("onerror", response);
                    $("#myTips").html("Load data failed");
                },
                ontimeout: function(response) {
                    console.error("ontimeout", response);
                    $("#myTips").html("Load data timeout");
                }
            });
        }
        function checkRadio(id, value, click){
            value = value || "Yes";
            click = click || true;
            if(click){
                $("#"+id+"[value='"+value+"']").prop("checked", true).trigger("click");
            } else {
                $("#"+id+"[value='"+value+"']").prop("checked", true);
            }
        }
        function checkSelect(id, value, change){
            change = change || true;
            if(change){
                $("#" + id + " option:contains(" + value + ")").prop("selected", true).trigger("change");
            } else {
                $("#" + id + " option:contains(" + value + ")").prop("selected", true);
            }
        }
        function changeSelectTrue(x){
            x.prop("selected", true).trigger("change");
        }
        function clickRadio(x){
            x.prop("checked", true).trigger("click");
        }

        var href = window.location.href;
        var pathname = window.location.pathname;
        var search = window.location.search;
        console.log(href, pathname, search);
        
        var param1 = getURLParameter("newapplication");
        var param2 = getURLParameter("event");
        var attemptEvent = "security.attemptLogin";
        var selectEvent = "course.select";
        var searchEvent = "course.search.process";
        
        if(param1 || param2 == attemptEvent){
            addTips();
            $(".formHead").append("<button id='selectStudent' type='button' style='float:right'>Select student</button>");

            $("body").append('<div id="simplemodal-overlay" class="simplemodal-overlay" style="opacity: 0.5; height: 100%; width: 100%; position: fixed; left: 0px; top: 0px; z-index: 1001;"></div>\
                <div id="simplemodal-container" class="simplemodal-container" style="position: fixed; z-index: 1002; height: 360px; width: 600px; left: 50%; top: 50%; margin-left: -300px; margin-top: -180px;"><div>\
                  <em>Select Student</em>\
                  <select data-placeholder="Choose a Student..." class="chosen-select" style="width:350px;">\
                    <option value=""></option>\
                  </select>\
                </div></div>');
            $("#simplemodal-overlay, #simplemodal-container").hide();
            $("#selectStudent").click(function(){
                $("#simplemodal-overlay, #simplemodal-container").show();
                queryApplication();
            });
            $("#simplemodal-overlay").click(function(){$("#simplemodal-overlay, #simplemodal-container").hide();});
        } else if(pathname == "/index.cfm" && !search && !$(".standardList").length){
            addTips();
            var mMark = $("#core_f10").val();
            if(!mMark){
                console.log("leave");
                return;
            }
            var mEmail = mMark.trim();
            get_course_application_info(mEmail, function(mJson){
                $("#myTips").html(mJson.PersonalDetails.first_name + " " + mJson.PersonalDetails.last_name+"'s Data has been loaded");
                var mDetails = mJson.PersonalDetails;
                var mCurrentContactDetails = mJson.CurrentContactDetails;
                var mPermanentContactDetails = mJson.PermanentContactDetails;
                var mLanguageProficiency = mJson.LanguageProficiency;
                if(mDetails.gender == "1"){
                    $("label:contains('Male')").trigger("click");
                } else{
                    $("label:contains('Female')").trigger("click");
                }
                var cob = mDetails["country_of_brith"];
                if(cob){
                    $("#core_f1103 option:contains("+cob+")").prop("selected", true);
                }
                //签证
                $("#core_f142").val(mDetails.passport_number);
                //项目点联系方式
                $("#core_f13").val(mCurrentContactDetails.address);
                $("#core_f14").val(mCurrentContactDetails.city);
                $("#core_f139").val(mCurrentContactDetails.mobile_number);
                if(mCurrentContactDetails.province){
                    $("#core_f15 option:contains("+mCurrentContactDetails.province+")").prop("selected", true);
                }
                //家庭联系方式
                $("#core_f20").val(mPermanentContactDetails.address);
                $("#core_f21").val(mPermanentContactDetails.city);
                $("#core_f141").val(mPermanentContactDetails.mobile_number);
                if(mPermanentContactDetails.province){
                    $("#core_f22 option:contains("+mPermanentContactDetails.province+")").prop("selected", true);
                }
                //语言熟练程度
                $("#core_f118_name").val(mLanguageProficiency.english_exam_test_name);
                var mDate = mLanguageProficiency.date_English_exam_taken.split("-");
                $("[name=core_f119_d]").val(parseInt(mDate[2]));
                $("[name=core_f119_m]").val(parseInt(mDate[1]));
                $("[name=core_f119_y]").val(mDate[0]);
                $("#core_f1092").val(mLanguageProficiency.overall_score);
                $("#core_f1093").val(mLanguageProficiency.reading_score);
                $("#core_f1094").val(mLanguageProficiency.writing_score);
                $("#core_f1095").val(mLanguageProficiency.speaking_score);
                $("#core_f1096").val(mLanguageProficiency.listening_score);

                var applicantid = $("[name=applicantid]").val();
                sessionStorage.setItem(mEmail, JSON.stringify(mJson));
                sessionStorage.setItem(applicantid, JSON.stringify(mJson));
            });

        } else if(selectEvent == param2){
//            https://customer.studylink.com/index.cfm?event=course.select&applicantid=2471372
            var applicantid = getURLParameter("applicantid");
            var myInfo = sessionStorage.getItem(applicantid);
            if(myInfo){
                addTips();
                myInfo = JSON.parse(myInfo);
                fillInTips(myInfo);
            }
        } else if(searchEvent == param2){
//            https://customer.studylink.com/index.cfm?event=course.search.process
            var applicantid = $("input[name=applicantid]").val();
            var myInfo = sessionStorage.getItem(applicantid);
            if(myInfo){
                addTips();
                myInfo = JSON.parse(myInfo);
                fillInTips(myInfo);
            }
        } else if(pathname == "/index.cfm" && (param2 == "application.create" || param2 == "application.quickadd.continue")){
            addTips();
            //大学申请页面
            var mImg = $("form table:first td:last img"); //        var mImg = $("form table tr:eq(0) td:nth-child(3)");
            var mUni = mImg.attr("title");
            console.log("大学是", mUni);
            var mEmail, myInfo;
            try{
                if(mUni.indexOf("Griffith University") >= 0){
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Griffith University Start $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                    mEmail = $("#fieldInput_19").text().trim();
                    console.log("邮箱", mEmail);
                    myInfo = JSON.parse(sessionStorage.getItem(mEmail));
                    console.log(myInfo);
                    if(!myInfo){
                        console.log("nothing to do, return");
                        return;
                    }
    //****************************** Introduction******************************
                    //Have you applied to Griffith before
                    //$("#fieldID_261333_dt1[value=No]").prop("checked", true).trigger("click");
                    $("div.questionWrapper:contains('Have you applied to Griffith before')").next().find("input[value=No]").prop("checked", true).trigger("click");

    //****************************** Personal details******************************
                    //Title
                    var mTitleSelect = $("div.fieldWrapper:contains('Title')").filter(function( index ) {
                                            return $(this).text().trim() == 'Title:';
                                          }).parent().find("select");
                    if(myInfo.PersonalDetails.title == "Mr"){
                        //$("#fieldID_261336_dt1").val("Mister=Mr").trigger("change");
                        mTitleSelect.val("Mister=Mr").trigger("change");
                    } else if(myInfo.PersonalDetails.title == "Miss"){
                        //$("#fieldID_261336_dt1").val("Miss=Miss").trigger("change");
                        mTitleSelect.val("Miss=Miss").trigger("change");
                    }

                    //Country of birth
                    var mCountry = $("div.fieldWrapper:contains('Country of birth')").parent().find("select option:contains("+myInfo.PersonalDetails.citizenship+")");
                    changeSelectTrue(mCountry);

    //****************************** Address for correspondence******************************
                    var mAddressParent = $("legend:contains('Address for correspondence')").parent();
                    //Address Line 1
                    var mAddress1 = mAddressParent.find("div.fieldWrapper:contains('Address Line 1')").next().find("input");
                    mAddress1.val(myInfo.CurrentContactDetails.address).trigger("change");
                    //City
                    var mCity = mAddressParent.find("div.fieldWrapper:contains('City')").next().find("input");
                    mCity.val(myInfo.CurrentContactDetails.city).trigger("change");
                    //Phone Number
                    var mPhone = mAddressParent.find("div.questionWrapper:contains('Phone Number')").next().find("input");
                    mPhone.first().val("86").trigger("change");
                    mPhone.last().val(myInfo.CurrentContactDetails.mobile_number).trigger("change");

    //****************************** Permanent address in home country******************************
                    var mPermanentParent = $("legend:contains('Permanent address in home country')").parent();
                    var mPmPhone = mPermanentParent.find("div.questionWrapper:contains('Phone Number')").next().find("input");
                    mPmPhone.eq(0).val("86").trigger("change");
                    mPmPhone.eq(1).val(myInfo.PermanentContactDetails.mobile_number).trigger("change");

    //****************************** English proficiency******************************
                    var mLang = $("div.questionWrapper:contains('I have taken')").parent().find("select option:contains("+myInfo.LanguageProficiency.english_exam_test_name+")");
                    changeSelectTrue(mLang);
                    var takenExamTime = myInfo.LanguageProficiency.date_English_exam_taken.split("-");
                    var mLangSelect = $("div.fieldWrapper:contains('Date of Test')").parent().find("select");
                    mLangSelect.eq(0).val(takenExamTime[2]).trigger("change");
                    mLangSelect.eq(1).val(takenExamTime[1]).trigger("change");
                    mLangSelect.eq(2).val(takenExamTime[0]).trigger("change");

    //****************************** Educational background******************************
                    //$("div.questionWrapper:contains('Qualification 1')").parent().find("div.fieldWrapper:contains(' Degree/award:')").parent().find("input");
                    var lenQual = myInfo.Qualification.length;
                    if(lenQual > 0){
                        var last = lenQual-1;
                        var qualification1 = myInfo.Qualification[last];
                        var mQ1 = $("div.questionWrapper:contains('Qualification 1')").parent();
                        var mQ1Text = mQ1.find("input:text");
                        mQ1Text.eq(0).val(qualification1.degree).trigger("change");
                        mQ1Text.eq(1).val(qualification1.Institution).trigger("change");
                        var mQ1Select = mQ1.find("select");
                        var mCountry = mQ1Select.eq(0).find("option:contains("+qualification1.country+")");
                        changeSelectTrue(mCountry);
                        var mStartDate = qualification1.start_date.split("-");
                        mQ1Select.eq(1).val(mStartDate[1]).trigger("change");
                        mQ1Select.eq(2).val(mStartDate[0]).trigger("change");
                        var mEndDate = qualification1.end_date.split("-");
                        mQ1Select.eq(3).val(mEndDate[1]).trigger("change");
                        mQ1Select.eq(4).val(mEndDate[0]).trigger("change");
                    }
                    if(lenQual && lenQual-1 > 0){
                        var lastButTwo = lenQual-2;
                        var qualification2 = myInfo.Qualification[lastButTwo];
                        var mQ2 = $("div.questionWrapper:contains('Qualification 2')").parent();
                        var mQ2Text = mQ2.find("input:text");
                        mQ2Text.eq(0).val(qualification2.degree).trigger("change");
                        mQ2Text.eq(1).val(qualification2.Institution).trigger("change");
                        var mQ2Select = mQ2.find("select");
                        var mCountry = mQ2Select.eq(0).find("option:contains("+qualification2.country+")");
                        changeSelectTrue(mCountry);
                        var mStartDate = qualification2.start_date.split("-");
                        mQ2Select.eq(1).val(mStartDate[1]).trigger("change");
                        mQ2Select.eq(2).val(mStartDate[0]).trigger("change");
                        var mEndDate = qualification2.end_date.split("-");
                        mQ2Select.eq(3).val(mEndDate[1]).trigger("change");
                        mQ2Select.eq(4).val(mEndDate[0]).trigger("change");
                    }
                    var mExcludedTertiary = $("div.fieldWrapper:contains('excluded from previous')").parent().find("input:radio[value=No]");
                    clickRadio(mExcludedTertiary);
    //****************************** Credit for previous study******************************
                    var mExemption = $("div.questionWrapper:contains('credit exemption')").parent().find("input:radio[value=Yes]");
                    clickRadio(mExemption);

    //****************************** Request for disability support******************************
                    var mDisability = $("div.questionWrapper:contains('disability support')").parent().find("input:radio[value=No]");
                    clickRadio(mDisability);

    //****************************** Visa Application History******************************
                    var mVisaRejected = $("div.questionWrapper:contains('visa application rejected')").parent().find("input:radio[value=No]");
                    clickRadio(mVisaRejected);

    //****************************** Overseas Student Health Cover******************************
                    var mHealthCover = $("div.questionWrapper:contains('the type of cover')").parent();
                    mHealthCover.find("select option:contains("+myInfo.OverseasStudentHealthCover.health_cover+")").prop("selected", true).trigger("change");

    //****************************** Financial support******************************
                    var mFinancial = $("div.questionWrapper:contains('financial support')").parent().find("input:radio");
                    if(myInfo.Financialsupport.is_have_scholarship == "0"){
                        clickRadio(mFinancial.eq(0));
                    } else{
                        clickRadio(mFinancial.eq(1));
                    }

    //****************************** Agent Declaration and signature******************************
                    $("span:contains(I agree)").find("input:checkbox").each(function(){
                        var mChecked = $(this).is(":checked");
                        mChecked?"":$(this).trigger("click");
                    });
                    var mAgentName = $("div.fieldWrapper:contains(Agent's Name)").parent().find("input:text");
                    mAgentName.val("Australia Education Management Group").trigger("change");

    //****************************** Declaration and signature******************************
                    var mUnder18 = $("div.fieldWrapper:contains('under 18 years')").parent().find("input:radio[value='No']");
                    clickRadio(mUnder18);
                    var mSignature = myInfo.PersonalDetails.first_name + " " + myInfo.PersonalDetails.last_name;
                    $("div.fieldWrapper:contains('Applicant\'s')").parent().find("input:text").val(mSignature).trigger("change");
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Griffith University END $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                } else if(mUni.indexOf("Tasmania") >= 0){
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Tasmania University $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                    mEmail = $("#fieldInput_22").text().trim();
                    console.log("邮箱", mEmail);
                    myInfo = JSON.parse(sessionStorage.getItem(mEmail));
                    console.log(myInfo);
                    if(!myInfo){
                        console.log("nothing to do, return");
                        return;
                    }
                    if(myInfo.PersonalDetails.title == "Mr"){
                        $("#fieldID_302301_dt1").val("Mr=MR").trigger("change");
                    } else if(myInfo.PersonalDetails.title == "Miss"){
                        $("#fieldID_302301_dt1").val("Miss=MISS").trigger("change");
                    }
                    $("#fieldID_302308_dt1").val(myInfo.PermanentContactDetails.city).trigger("change"); //birth city
                    $("#fieldID_302310_dt1 option:contains("+myInfo.PersonalDetails.citizenship+")").prop("selected", true).trigger("change");
                    $("#fieldID_302313_dt1[value=No]").prop("checked", true).trigger("click"); //student ID in Tasmania
                     //disability support
                    if(myInfo.RequestForDisabilitySupport.is_need_disability_support == "1"){
                        $("#fieldID_302316_dt1[value=Yes]").prop("checked", true).trigger("click");
                    }else{
                        $("#fieldID_302316_dt1[value=No]").prop("checked", true).trigger("click");
                    }

                    $("#fieldID_302319_dt1").val(myInfo.PermanentContactDetails.address).trigger("change");
                    $("#fieldID_302321_dt1").val(myInfo.PermanentContactDetails.city).trigger("change");
                    $("#fieldID_302324_dt1").val(myInfo.PermanentContactDetails.mobile_number).trigger("change");
                    $("#fieldID_302418_dt1[value=Yes]").prop("checked", true).trigger("click");
                    $("#fieldID_302430_dt1[value=No]").prop("checked", true).trigger("click");

                    $("#fieldID_302426_dt1 option:contains("+myInfo.LanguageProficiency.english_exam_test_name+")").prop("selected", true);
                    $("#fieldID_302420_dt1").val(myInfo.LanguageProficiency.overall_score).trigger("change");
                    $("#fieldID_302421_dt1").val(myInfo.LanguageProficiency.writing_score).trigger("change");
                    $("#fieldID_302422_dt1").val(myInfo.LanguageProficiency.reading_score).trigger("change");
                    $("#fieldID_302423_dt1").val(myInfo.LanguageProficiency.listening_score).trigger("change");
                    $("#fieldID_302424_dt1").val(myInfo.LanguageProficiency.speaking_score).trigger("change");
                    
                    var lenQual = myInfo.Qualification.length;
                    //secondary school
                    if(lenQual > 0){
                        var last = lenQual-1;
                        $("#fieldID_302432_dt1").val(myInfo.Qualification[last].Institution).trigger("change");
                        $("#fieldID_302433_dt1").val(myInfo.Qualification[last].degree).trigger("change");
                        $("#fieldID_302434_dt1 option:contains("+myInfo.Qualification[last].country+")").prop("selected", true).trigger("change");
                        var mStartDate = myInfo.Qualification[last].start_date.split("-");
                        $("#date_fieldID_302437_dt1_mm").val(mStartDate[1]).trigger("change");
                        $("#date_fieldID_302437_dt1_yyyy").val(mStartDate[0]).trigger("change");
                        var mEndDate = myInfo.Qualification[last].end_date.split("-");
                        $("#date_fieldID_302438_dt1_mm").val(mEndDate[1]).trigger("change");
                        $("#date_fieldID_302438_dt1_yyyy").val(mEndDate[0]).trigger("change");
                        $("#fieldID_302440_dt1[value=Yes]").prop("checked", true).trigger("click");
                    }
                    

                    //post secondary school
                    if(lenQual && lenQual-1 > 0){
                        var lastButTwo = lenQual-2;
                        $("#fieldID_302444_dt1").val(myInfo.Qualification[lastButTwo].Institution).trigger("change");
                        $("#fieldID_302445_dt1").val(myInfo.Qualification[lastButTwo].degree).trigger("change");
                        $("#fieldID_302448_dt1 option:contains("+myInfo.Qualification[lastButTwo].country+")").prop("selected", true).trigger("change");
                        var mPostSecondaryStartDate = myInfo.Qualification[lastButTwo].start_date.split("-");
                        $("#date_fieldID_302450_dt1_mm").val(mPostSecondaryStartDate[1]).trigger("change");
                        $("#date_fieldID_302450_dt1_yyyy").val(mPostSecondaryStartDate[0]).trigger("change");
                        var mPostSecondaryEndDate = myInfo.Qualification[lastButTwo].end_date.split("-");
                        $("#date_fieldID_302451_dt1_mm").val(mPostSecondaryEndDate[1]).trigger("change");
                        $("#date_fieldID_302451_dt1_yyyy").val(mPostSecondaryEndDate[0]).trigger("change");
                    }

                    //VISA AND OTHER INFORMATION
                    $("#fieldID_302446_dt1 option:contains("+ myInfo.Qualification[0].degree +")").prop("selected", true).trigger("change");
                    $("#fieldID_302452_dt1 option[value=Yes]").prop("selected", true).trigger("change");
                    if(myInfo.CreditForPreviousStudy.is_credit_exemption == "1"){
                        $("#fieldID_302452_dt1 option[value=Yes]").prop("selected", true);
                    }
                    $("#fieldID_302503_dt1[value=No]").prop("checked", true).trigger("click"); //excluded from university
                    $("#fieldID_302507_dt1[value=No]").prop("checked", true).trigger("click"); //compelted course more than 6 months
                    $("#fieldID_302509_dt1[value=Yes]").prop("checked", true).trigger("click"); //passport
                    $("#fieldID_302510_dt1").val(myInfo.PassportAndVisaDetails.passport_number).trigger("change");
    //                checkRadio("fieldID_302514_dt1", "No"); //valid visa
                    var passportExpiry = myInfo.PassportAndVisaDetails.passport_expiry_date.split("-");
                    $("#date_fieldID_302511_dt6_dd").val(passportExpiry[2]).trigger("change");
                    $("#date_fieldID_302511_dt6_mm").val(passportExpiry[1]).trigger("change");
                    $("#date_fieldID_302511_dt6_yyyy").val(passportExpiry[0]).trigger("change");

                    checkRadio("fieldID_302524_dt1", "No");

                    $("#fieldID_302519_dt1[value=Yes]").prop("checked", true).trigger("click"); //student visa
                    $("#fieldID_302598_dt1[value='Family Support']").trigger("click"); //financial
                    $("#fieldID_302600_dt1[value=Yes]").prop("checked", true).trigger("click"); //transfer funds
                    $("#fieldID_302601_dt1[value=Yes]").prop("checked", true).trigger("click"); //sufficient funds
                    $("#fieldID_302602_dt1[value=No]").prop("checked", true).trigger("click"); //other Award
                    $("#fieldID_302605_dt1[value=No]").prop("checked", true).trigger("click"); //brother enrolled
                    checkRadio("fieldID_302626_dt1", "No"); //authorize permission
                    if(!$("#checked302632").is(":checked")){
                        $("#checked302632").trigger("click");
                    }
                    var signature = myInfo.PersonalDetails.first_name + " " + myInfo.PersonalDetails.last_name;
                    $("#fieldID_302633_dt1").val(signature).trigger("change");
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Tasmania University END $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                } else if(mUni.indexOf("Australian National University") >= 0){
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Australian National University $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                    mEmail = $("#fieldInput_21").text().trim();
                    console.log("邮箱", mEmail);
                    myInfo = JSON.parse(sessionStorage.getItem(mEmail));
                    console.log(myInfo);
                    if(!myInfo){
                        console.log("nothing to do, return");
                        return;
                    }
                    // title
                    if(myInfo.PersonalDetails.title == "Mr"){
                        $("#fieldID_299666_dt1 option[value='Mr=Mr']").prop("selected", true).trigger("change");
                    } else{
                        checkSelect("fieldID_299666_dt1", myInfo.PersonalDetails.title);
                    }
                    // study in ANU before
                    checkRadio("fieldID_299668_dt1", "No");
                    // disability
                    if(myInfo.RequestForDisabilitySupport.is_need_disability_support == "0"){
                        checkRadio("fieldID_299675_dt1", "No");
                    } else {
                        checkRadio("fieldID_299675_dt1");
                    }
                    // Postal Address
                    checkSelect("fieldID_299678_dt1", myInfo.CurrentContactDetails.country);
                    // permanent home address
                    checkSelect("fieldID_299686_dt1", myInfo.PermanentContactDetails.country);
                    $("#fieldID_299687_dt1").val(myInfo.PermanentContactDetails.address).trigger("change");
                    $("#fieldID_299690_dt1").val(myInfo.PermanentContactDetails.city).trigger("change");
                    //humanitarian visa
                    checkRadio("fieldID_299697_dt1", "No");
                    //citizenship status
                    checkSelect("fieldID_299698_dt1", "other than");
                    //Australian Aboriginal
                    checkSelect("fieldID_299699_dt1", "No");
                    //6 months
                    checkRadio("fieldID_299704_dt1");
                    //expiry date
                    var mExpiry = myInfo.PassportAndVisaDetails.passport_expiry_date.split("-");
                    $("#date_fieldID_299706_dt6_dd").val(mExpiry[2]).trigger("change");
                    $("#date_fieldID_299706_dt6_mm").val(mExpiry[1]).trigger("change");
                    $("#date_fieldID_299706_dt6_yyyy").val(mExpiry[0]).trigger("change");
                    //breached
                    checkRadio("fieldID_299713_dt1", "No");
                    //convicted
                    checkRadio("fieldID_299715_dt1", "No");
                    //English Language Proficiency
                    checkSelect("fieldID_299744_dt1", "English language test");
                    checkRadio("fieldID_299746_dt1");
                    checkSelect("fieldID_299748_dt1", myInfo.LanguageProficiency.english_exam_test_name);
                    var takenDate = myInfo.LanguageProficiency.date_English_exam_taken.split("-");
                    $("#date_fieldID_299749_dt6_dd option[value="+takenDate[2]+"]").prop("selected", true).trigger("change");
                    $("#date_fieldID_299749_dt6_mm option[value="+takenDate[1]+"]").prop("selected", true).trigger("change");
                    $("#date_fieldID_299749_dt6_yyyy option[value="+takenDate[0]+"]").prop("selected", true).trigger("change");
                    $("#fieldID_299750_dt1").val(myInfo.LanguageProficiency.overall_score).trigger("change");
                    $("#fieldID_299751_dt1").val(myInfo.LanguageProficiency.reading_score).trigger("change");
                    $("#fieldID_299752_dt1").val(myInfo.LanguageProficiency.writing_score).trigger("change");
                    $("#fieldID_299753_dt1").val(myInfo.LanguageProficiency.speaking_score).trigger("change");
                    $("#fieldID_299754_dt1").val(myInfo.LanguageProficiency.listening_score).trigger("change");
                    //degree
                    checkSelect("fieldID_299780_dt1", myInfo.Qualification[0].degree.toUpperCase());
                    var mStartDate = myInfo.Qualification[0].start_date.split("-");
                    $("#date_fieldID_299774_dt1_mm").val(mStartDate[1]).trigger("change");
                    $("#date_fieldID_299774_dt1_yyyy").val(mStartDate[0]).trigger("change");
                    var mEndDate = myInfo.Qualification[0].end_date.split("-");
                    $("#date_fieldID_299775_dt1_mm").val(mEndDate[1]).trigger("change");
                    $("#date_fieldID_299775_dt1_yyyy").val(mEndDate[0]).trigger("change");

                    if(myInfo.RequestForDisabilitySupport.is_have_visa_reject == "0"){
                        checkRadio("fieldID_299836_dt1", "No");
                    } else{
                        checkRadio("fieldID_299836_dt1");
                    }
                    if(myInfo.Financialsupport.is_have_scholarship == "0"){
                        checkRadio("fieldID_299741_dt1", "Self Funded");
                    }
                    //certify
                    var isDeclareChecked = $("#checked299924").is(":checked");
                    if(!isDeclareChecked){
                        $("#checked299924").trigger("click");
                    }
                    isDeclareChecked = $("#checked299925").is(":checked");
                    if(!isDeclareChecked){
                        $("#checked299925").trigger("click");
                    }
                    isDeclareChecked = $("#checked299926").is(":checked");
                    if(!isDeclareChecked){
                        $("#checked299926").trigger("click");
                    }
                    isDeclareChecked = $("#checked299928").is(":checked");
                    if(!isDeclareChecked){
                        $("#checked299928").trigger("click");
                    }
                    if(!$("#checked299929").is(":checked")){
                        $("#checked299929").trigger("click");
                    }
                    if(!$("#checked299930").is(":checked")){
                        $("#checked299930").trigger("click");
                    }
                    if(!$("#checked299931").is(":checked")){
                        $("#checked299931").trigger("click");
                    }
                    if(!$("#checked299932").is(":checked")){
                        $("#checked299932").trigger("click");
                    }
                    if(!$("#checked302231").is(":checked")){
                        $("#checked302231").trigger("click");
                    }

                    var mDate = new Date();
                    var mDay = mDate.getDate();
                    if(mDay < 10){
                        mDay = "0" + mDay;
                    }
                    var mMonth = mDate.getMonth();
                    if(mMonth < 9){
                        mMonth += 1;
                        mMonth = "0" + mMonth;
                    } else{
                        mMonth += 1;
                    }
                    $("#date_fieldID_299935_dt6_dd option[value="+mDay+"]").prop("selected", true).trigger("change");
                    $("#date_fieldID_299935_dt6_mm option[value="+mMonth+"]").prop("selected", true).trigger("change");
                    $("#date_fieldID_299935_dt6_yyyy option[value="+mDate.getFullYear()+"]").prop("selected", true).trigger("change");
    //$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$ Australian National University END $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                }
                //提示信息
                fillInTips(myInfo);

            }catch(e){
                console.error(e);
            }


        }
    });

})(jQuery);


//window.onbeforeunload = function(e) {
//  return ("onbeforeunload");
//};
```
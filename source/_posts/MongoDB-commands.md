---
title: MongoDB Commands
date: 2016-04-13 09:51:09
categories: Database
tags: [Command, MongoDB]
---

一些常用的mongo命令
<!--more-->

``` bash
./mongodb/bin/mongo  
use admin;
db.auth('username','XXXXXX');
show dbs;
show collections;
db.pushbind.find().pretty(); #格式化
db.courierlocation.findOne();
db.mobilesession.find({username:'13692173844'});
db.mobilesession.find({versionCode:{$ne:null}}) #不等于空
db.special.find({com:"debangwuliu"}).sort({_id:-1}); #倒序
db.special.find({com:'ems',_id:/^110100000000/}).count() #以XXX开头的记录个数
db.special.find().limit(10).skip(5)
db.special.find({ct: {$gte: 1447084800000, $lte: 1447167600000}}) #大于等于，小于等于
db.special.find({ct:{$gt:new Date().getTime()-24*60*60*1000}}).count() #一天前记录个数

# 记录的增删改
db.special.remove({appid:/23/})    #删除包含23的appid
db.special.insert({"name" : "yiibai"})
db.special.save({"_id" : ObjectId(5983548781331adf45ec7), "title":"Yiibai New Topic", "by":"Yiibai"}
db.special.update({_id:'chongwu'}, {$set:{url:'http://mp.weixin.qq.com'}}, false, true);

# 集合的增删 
db.createCollection('special')  #新建collection，不常用，save会自动新建
db.COLLECTION_NAME.drop();

#联合操作
db.foo.remove({_id:{ $lt : db.foo.find().skip(5).limit(1)[0]._id}}) 
db.blackkeywords.remove({_id:{$ne:db.blackkeywords.find().skip(1257).limit(1)[0]._id}})

```
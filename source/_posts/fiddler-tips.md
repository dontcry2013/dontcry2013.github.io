---
title: Fiddler Tips
date: 2016-03-15 14:57:37
categories: Tools
tags: [Fiddler, Proxy]
---

Fiddler is coded by C# and it's a free http proxy debugging tool. It has two version which are .net 2 and .net 4. Fiddler can record all the messages that transmitted between your computer and internet. Basically, it's really easy to use, but it doesn't means it's simple designed but, on the contrary, it can set breakpoints, run script before/after a http request to tunning and examine the data which is really powerful.
<!-- more -->

## use fiddler to debugging mobile app

### Test the connection
your mobile phone and the computer which runs fiddler should in the same ethernet. To validate, run a server on the computer and use the phone to access the server to test if the connection is alright. 

#### Fiddler configuration
![configuration](/img/fiddler-config.png)
set Fiddler->Tools->Fiddler Option like above.

#### Usage
![configuration](/img/fiddler-panel.png)
* Left panel is the whole records of the access history, we can observe the load process of a webpage.
* Right click of the record, Filter Now->Hide Url, to avoid some annoying message.
* TextWizard is a set of handy tools.
* Right panel, composer can build a http request which you can modify the head and body.




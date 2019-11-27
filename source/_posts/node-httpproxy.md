---
title: Use Node.js To Implement HTTP Proxy
date: 2016-03-06 22:23:11
categories: Front-end
tags: [HTTP, Node.js]
---
## Demand
When I have username and password to simulate a login action on a website, how can I pass the verification algorithm or validation routines, sometimes this kind of program is really complicated which is the biggest obstacle to hack in. For instance, when you have plenty [Taobao](https://login.m.taobao.com/login.htm) usernames and passwords how can you figure out what the customer bought, as this b2c mogul uses an algorithm to check all http request if they are legal.
<!-- more -->

## Analysis
We need that signature program to disguise our http request.

before we dig it out from the website we really need a workstation to do some experiments as a handy tool may save us loads of time. The longer answer is that we could use http proxy to falsify a normal http request and to verify our speculation.


## Tools & Implementation
Chrome
SwitchyOmega chrome plugin
[requirejs](https://github.com/jrburke/requirejs).

With switchyOmega we can get a proxy server interface locally, use 127.0.0.1 and a specific port, let's say 8000.

Now, all the requests will pass through this port before rendered by the browser.

Download requirejs, we need it to load our js in local.

What we need then is to run a http server instance to do our own secret business, it is time to shine a spotlight on the nodejs server and some scripts. 

``` js
/**
*	proxy.js
**/
var sys = require("sys");
var http = require("http");
var fs = require('fs');
/**
 *  User defined module containing the special handlers
 **/
var special_handler = require("./handlers");
// var special_handler = require("C:/Users/pc/Desktop/handlers");

/**
 *  ignore common connection errors like broken pipes.
 **/
process.on('uncaughtException', function (err) {});

/**
 *  Default handler. Proxy the requests that don't match the patterns
 **/
var proxy_handler = function(response, data) {
    response.write(data, 'binary');
}

/**
 *  This is the Request Handler function for the server.
 **/
var requestHandler = function(clientRequest, clientResponse) {

    sys.puts("Request: " + clientRequest.url);
	
	if(clientRequest.url.indexOf('lib-yocto')>0){
		fs.readFile('./taobaof.js', function (err, data) {
			if (err) throw err;
			console.log('just here\n\n\n\n\n\n');
			clientResponse.setHeader('content-type','application/x-JavaScript');
			clientResponse.write(data);
			clientResponse.end();
		});
		return;
	}

    /* Create the client */
    var proxy = http.createClient(80, clientRequest.headers['host'])
    /* Do the request */
    var client = proxy.request(clientRequest.method, clientRequest.url, clientRequest.headers);

    /* Handle on response event */
    client.on("response", function(response) {

        sys.puts("Status Code: " + response.statusCode);

        clientResponse.writeHeader(response.statusCode, response.headers);

        /* By Default the handler is proxy_handler */
        handler = proxy_handler;

        /**
         * Process the user defined patterns and handlers
         * */
        for (hand in special_handler.handlers) {

            if (clientRequest.url.search(special_handler.handlers[hand].pattern) != -1)
            {
                handler = special_handler.handlers[hand].action
                break;
            }
        }

        /* Write the response data while receive */
        response.on('data', function(data) {
            handler(clientResponse, data);
        });

        /* Close the connection */
        response.on("end", function() {
            clientResponse.end();
        })
    });

    /* Write the request data while receive */
    clientRequest.on("data", function (chunk) {
        client.write(chunk, 'binary');
    });

    /* Close the connection */
    clientRequest.on("end", function () {
        client.end();
    });
}

/**
 *  Create and run the server!
 **/
var server = http.createServer(requestHandler);
server.listen(8000);
sys.puts("Server running at http://127.0.0.1:8000/")

```


``` js
/**
*	handlers.js
**/

exports.handlers = [{
    pattern: 'facebook',

    action: function(response) {
        response.writeHead(200, {
            'Content-Type': 'text/html'
        });
        response.end("<h1>Ups, Sorry. <br/><br/> I Like Facebook but I Block it... <br/><br/> Please Contact the Administrator </h1>");
    }
}];

```
As above scripts block facebook and show a message to manifest our workstation is built up .

Finally, it's time to use chrome to open a login web page of the website - https://login.m.taobao.com/login.htm. Listen all the http & https requests and analyse & verify the signature program.


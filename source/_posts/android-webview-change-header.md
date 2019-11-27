---
title: How To Modify Android Webview Request Header
date: 2016-03-11 09:20:36
categories: Android
tags: [Webview]
---

Let's say something about android webview, when we open webpage in an android application, the http header would some how add a field named x-requested-with to show your app's package name. For some websites may prohibit any further access according to this field and their webpages are only available for their own app and browsers. To tackle this problem and make our app to access these webpages we need to modify webview client.

<!-- more -->

## Analysis
From Android API 11 (3.0)，WebView supply a new API in WebViewClient, like below:

``` java
public WebResourceResponse shouldInterceptRequest(WebView view, String url)  
```
we can use this by call setWebViewClient.

but, in API21, a change occurs in this function, 

``` java
public WebResourceResponse shouldInterceptRequest(WebView view, final WebResourceRequest request)  
```

## Implement

In order to adapt all the devices, both function should be implemented.

``` java
webView.setWebViewClient(new WebViewClient() {
  @SuppressLint("NewApi")
  @Override
  public WebResourceResponse shouldInterceptRequest(WebView view, WebResourceRequest request) {
    if (request != null && request.getUrl() != null && request.getMethod().equalsIgnoreCase("get")) {
      String scheme = request.getUrl().getScheme().trim();
      if (scheme.equalsIgnoreCase("http") || scheme.equalsIgnoreCase("https")) {
        try {
          URL url = new URL(injectIsParams(request.getUrl().toString()));
          URLConnection connection = url.openConnection();
          return new WebResourceResponse(connection.getContentType(), connection.getHeaderField("encoding"), connection.getInputStream());
        } catch (MalformedURLException e) {
          e.printStackTrace();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
    return null;
  }
  @Override
  public WebResourceResponse shouldInterceptRequest(WebView view, String url) {
    if (!TextUtils.isEmpty(url) && Uri.parse(url).getScheme() != null) {
      String scheme = Uri.parse(url).getScheme().trim();
      if (scheme.equalsIgnoreCase("http") || scheme.equalsIgnoreCase("https")) {
        try {
          URL url = new URL(injectIsParams(request.getUrl().toString()));
          URLConnection connection = url.openConnection();
          return new WebResourceResponse(connection.getContentType(), connection.getHeaderField("encoding"), connection.getInputStream());
        } catch (MalformedURLException e) {
          e.printStackTrace();
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }
    return null;
  }
});
```

For our demand, we need to change all the url which requested and add a param as a plus to mark.

``` java
public static String injectIsParams(String url) {  
  if (url != null && !url.contains("xxx=") {
    if (url.contains("?")) {
      return url + "&xxx=1";
    } else {
      return url + "?xxx=1";
    }
  } else {
    return url;
  }
}
```

Attention: to do different operation by identify http and https.

## More

* construct WebResourceResponse a intercept the http request and use local image to change the image from web.

``` java
WebResourceResponse response = null;  
if (url.contains("logo")) {  
  try {
    InputStream is = getAssets().open("test.png");
    response = new WebResourceResponse("image/png", "UTF-8", is);
  } catch (IOException e) {
    e.printStackTrace();
  }		
}
return response;  
```

* for the version above API 21 (5.0), use WebResourceRequest interface to modify header
``` java
@Override
public Map<String, String> getRequestHeaders() {  
    return request.getRequestHeaders();
}
```

* distinguish the different request from Get to Post by use this function and could be used to identify some differences about AJAX .


## Problems

the webpage which I tended to modify actually did not rendered by the android browser, they are just some html raw characters, which is not a successfully experiment apparently, and I will keep looking for the reasons and if success I'll refresh this blog.





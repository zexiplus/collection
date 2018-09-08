# chrome.runtime

>  chrome浏览器接口



### catalogue

[TOC]

### chrome.runtime summary     

> [链接](https://developer.chrome.com/apps/runtime)

> Methods & properties
>
> connect, id, getURL, getMainifest, onMessage, sendMessage, onConnect

```js
// properties
chrome.runtime.id  // the id of the extension/app

// methods
// connect
chrome.runtime.connect(string id, object connectInfo)

// sendMessage
chrome.runtime.sendMessage(string id, any message, object options, function reponseCallback)

// onMessage
chrome.runtime.onMessage.addListener(function callback)

```



### 插件和页面消息传递

> [链接](https://crxdoc-zh.appspot.com/apps/messaging#connect)

> content.js

```js
// 从页面js发送消息到chrome插件
chrome.runtime.sendMessage({greeting: "您好"}, function(response) {
  console.log(response.farewell);
});

// 页面监听浏览器插件发送的消息
chrome.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    console.log(sender.tab ?
                "来自内容脚本：" + sender.tab.url :
                "来自扩展程序");
    if (request.greeting == "您好")
      sendResponse({farewell: "再见"});
  });

```

> plugin.js

```js
// 在扩展程序中与页面通讯
chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {
  chrome.tabs.sendMessage(tabs[0].id, {greeting: "您好"}, function(response) {
    console.log(response.farewell);
  });
});
```


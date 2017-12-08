---
layout: post
title: Chrome插件关闭窗口
category: chrome
tags: [chrome,close]
---

写插件时，某些情况下Chrome通过window.close关不了窗口，用插件的功能来关闭。

# 主要通过chrome tabs remove来完成

window页面向插件background通信，background里面可以对Chrome页签进行关闭操作。

## manifest.json打开对tab操作的准许

```js
"permissions": [
    "tabs"
  ]
```

## widnow页面Js代码

```js
chrome.runtime.sendMessage({});
```

## background.js代码

```js
chrome.runtime.onMessage.addListener(function (request, sender, sendResponse) {
    chrome.tabs.remove(sender.tab.id);
});
```
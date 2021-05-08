---
title: 在小程序webview中H5页面判断是否是小程序环境
date: 2018-09-05 10:12:01
tags:
---

在实际业务代码中，我经常会和小程序来做交互，查阅了小程序开发文档，提供的方法总是会有各种奇怪的bug，ios和安卓总会有奇奇怪怪的场景bug，总结了一个函数

```javascript
function checkMiniProgram(){
    var miniProgram= false;
    //安卓可以用UA
    if(window.navigator.userAgent.indexOf("miniProgram") !== -1){
        miniProgram = true;
      }
    function isWXXCX(){
      var isWXX=false;
      if (!window.WeixinJSBridge || !WeixinJSBridge.invoke) {
        document.addEventListener('WeixinJSBridgeReady', ready, false)
      } else {
        ready()
      }
      function ready(){
        isWXX = (window.__wxjs_environment === 'miniprogram');
        return isWXX;
      }
      //小程序文档上说最好用回调形式，其实在实际场景直接调用没有出现过判断失败的情况。
      return ready();
    }
    miniProgram =miniProgram? miniProgram: isWXXCX();
    return miniProgram
}
```
---
title: '微信扫一扫，扫条码去掉code_128 '
date: 2019-08-28 11:29:24
categories: 
  - 微信
  - work
tags: 
  - 微信
  - work
---
#### 背景
在微信里面调用js扫一扫，但是扫条形码的时候，扫出来的结果都会带上CODE_128,
 <!--more-->
#### 解决：
使用js，把扫描返回的结果进行加工，去掉CODE_128:
```
wx.scanQRCode({
    needResult: 0, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
    scanType: ["qrCode","barCode"], // 可以指定扫二维码还是一维码，默认二者都有
    success: function (res) {
        var result = res.resultStr; // 当needResult 为 1 时，扫码返回的结果
        var barCode = getBarCode(result);
    }
});
function getBarCode(resultStr){
        var serial = resultStr.split(",");
        var barCode = serial[serial.length-1];
        return barCode;
} 
```
---
title: Web Audio (三) 
date: 2019-12-09 11:00:00
tags: ['AudioContext','JavaScript']
categories: WebAPI
---

AudioContext提供了设配音频源的支持，配合MediaDevices提供的相关接口，我们可以获取麦克风音源，并进行相关操作。
<!-- more -->

## 获取麦克风音源
我们可以使用**MediaDevices.getUserMedia()** 来来请求获取用户的媒体设备（摄像头，麦克风）输入，媒体输入会产生一个**MediaStream**，里面包含了请求的媒体类型的轨道。此流可以包含一个视频轨道（来自硬件或者虚拟视频源，比如相机、视频采集设备和屏幕共享服务等等）、一个音频轨道（同样来自硬件或虚拟音频源，比如麦克风、A/D转换器等等），也可能是其它轨道类型。[MDN参考文档](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices/getUserMedia)

1. constraints配置参数参考（[MediaStreamConstraints](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamConstraints)）

```js
// 同时请求不带任何参数的音频和视频
{ audio: true, video: true }

// 指定摄像头分辨率（注意：用户可能没有这么高分辨率的摄像头，那么浏览器可能会提供一个其它分辨率的视频）
{
  audio: true,
  video: { width: 1280, height: 720 }
}

// 强制要求视频的最低尺寸（注意：如果不能满足，那么将会获取失败，可以在Promise的catch中获取这个错误）
{
  audio: true,
  video: {
    width: { min: 1280 },
    height: { min: 720 }
  }
}

// min是最小值，ideal是理想值，max为最大值 
{
  audio: true,
  video: {
    width: { min: 1024, ideal: 1280, max: 1920 },
    height: { min: 776, ideal: 720, max: 1080 }
  }
}

// 前置摄像头
{ audio: true, video: { facingMode: "user" } }

// 后置摄像头
{ audio: true, video: { facingMode: { exact: "environment" } } }
```

2. 获取用户麦克风

```JavaScript
navigator.mediaDevices
.getUserMedia({audio: true, video: false})
.then(stream => console.log(stream))
.catch(e => console.log(e));
```

3. 通过stream创建音频源
```JavaScript
function createAudioSource(stream) {
    const AudioContext = window.AudioContext || window.webkitAudioContext;
    let audioContext = new AudioContext();
    let audioSource = audioContext.createMediaStreamSource(stream);
}
```


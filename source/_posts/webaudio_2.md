---
title: Web Audio (二) 
date: 2019-07-24 19:21:07
tags: ['AudioContext','JavaScript', 'WebGL']
categories: WebAPI
---

音频分析节点**(AnalyserNode)**可以用来实时频率分析与时域分析，本文记录了使用分析节点构建一个音频可视化Demo的过程。
<!-- more -->


# 音频可视化实现

## 创建音频源
我们可以使用`audio`标签来创建我们的音频源，本例子将使用网络音频作为演示素材。**AudioContext**提供了`createMediaElementSource`接口，它可以通过`audio`的DOM节点来创建音频源。
```html
// 创建AudioSourceNode

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <audio id="audioSource" src="https://wavesurfer-js.org/example/media/demo.wav" controls crossorigin="anonymous"></audio>
</body>
<script>
    const AudioContext = window.AudioContext || window.webkitAudioContext;
    const audioElement = document.getElementById('audioSource');
    const audioContext = new AudioContext();
    const audioSourceNode = audioContext.createMediaElementSource(audioElement);
</script>

</html>
```

## 创建音频分析节点
**AudioContext**提供了`createAnalyser`接口用于创建分析器。
```JavaScript
const analyser = audioContext.createAnalyser();
```

## 连接音频源，分析节点，输出
**AudioNode**提供了`connect`方法来连接其它节点，我们可以调用`audioSourceNode`的`connect`方法来连接`analyser`,最终我们把节点连接到默认输出节点`audioContext.destination`
```JavaScript
audioSourceNode.connect(analyser).connect(audioContext.destination);
```

## 查看分析节点的音频数据
**AnalyseNode**的成员变量`fftSize`一个无符号长整形的值，代表了用于计算频域信号时使用的 FFT (快速傅里叶变换) 的窗口大小。frequencyBinCount是一个无符号长整形的值, 值为fftSize的一半，通常等于将要用于可视化的数据值的数量。
```JavaScript
analyser.fftSize = 2048;
anaylserData = new Uint8Array(this.analyser.frequencyBinCount);
function showAnalyseData() {
    analyser.getByteTimeDomainData(anaylserData);
    console.log(anaylserData);
}
// 1000ms获取一次当前缓冲区数据
setTimeout(showAnalyseData, 1000);
```

## 绘制音频图像
getByteTimeDomainData方法把音频可视化数据(波形)导入到anaylserData数组中了，我们使用canvas画布把每一个点描绘出来即可。
```JavaScript
function displayAnalysData() {
    
}
```

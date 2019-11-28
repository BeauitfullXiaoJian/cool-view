---
title: WebRTC笔记-服务端环境搭建
date: 2019-01-25 15:11:11
tags: ['WebRTC']
categories: Web
---

开源的TURN&STUN服务器实现，支持 P2P 穿透防火墙。主要用于WebRTC点对点视频音频通话.
<!-- more -->

# 服务端搭建

## WebRTC 连接顺序
1. 直接连接（两台设备直接可以访问对方，同一内网中）
2. 通过stun服务器进行穿透
3. 无法穿透则通过turn服务器中转

## 安装coturn
1. Ubuntu apt可以直接安装
`apt-get install coturn`
2. 编译安装
* [GitHub地址](https://github.com/coturn/coturn)

## 简单运行
```cmd
turnserver -o -a -f -v --mobility -m 2 --max-bps=100000 --min-port=32355 --max-port=65535 --user=账号:密码 -r demo -L公网ip
```

## 使用时配置参数
```javascript
const peerConnectionConfig = {
    iceServers: [
        {
            urls: "stun:139.129.161.216:3478",
            username: '账号',
            credential: '密码'
        },
        {
            urls: "turn:139.129.161.216:3478",
            username: '账号',
            credential: '密码'
        }
    ]
};

const pc = new RTCPeerConnection(peerConnectionConfig);
```

## 服务器测试
测试网址：https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/
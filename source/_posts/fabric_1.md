---
title: Hyperledger Fabric 节点介绍
date: 2019-12-16 11:29:04
tags: ["Fabric"]
categories: 区块链
---

Hyperledger Fabric 是一个开源且具有企业级许可的分布式记账技术 （DLT） 平台，专为在企业环境中使用而设计。其本身和其它流行的分布式记账及区块链平台有一定差异，fabric引入了成员管理服务，每个参与者必须提供对应证书证明才能允许访问Fabric系统。
<!-- more -->

# 什么是区块链
区块链网络核心是一个分布式账本，记录所有在网络上发生的交易。

In general terms, a blockchain is an immutable transaction ledger, maintained within a distributed network of peer nodes. These nodes each maintain a copy of the ledger by applying transactions that have been validated by a consensus protocol, grouped into blocks that include a hash that bind each block to the preceding block.

一般来说，区块链就是一个一成不变的

# Fabric特性
1. 身份管理
2. 隐私和保密
3. 高性能
4. 函数式合约代码编程
5. 模块化设计

# 相关概念速览

## Channel
A channel is a private blockchain overlay which allows for data isolation and confidentiality. A channel-specific ledger is shared across the peers in the channel, and transacting parties must be properly authenticated to a channel in order to interact with it. Channels are defined by a Configuration-Block.

## Channel
在Fabric网络中的一个私有独立链，只有加入这个Channel的节点才能对Channel中的账本进行操作。

## 

## 锚点 Anchor Peer
通道上的每个成员都有一个锚点（或多个锚点，以防止单点故障），从而允许属于不同成员的对等点发现通道上的所有现有对等点

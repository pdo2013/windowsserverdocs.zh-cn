---
title: 性能的网络适配器的历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239234"
---
# 性能的网络适配器的历史记录

> 适用于： Windows Server Insider Preview

性能历史记录收集的网络适配器的详细介绍了[为存储空间直通的性能历史记录](performance-history.md)此子主题。 网络适配器性能历史记录仅适用于在群集中的每个服务器中的每个物理网络适配器。 启用了 RDMA 是适用于每个物理网络适配器远程直接内存访问 (RDMA) 性能历史记录。

   > [!NOTE]
   > 不能在关闭的服务器的网络适配器收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## 系列名称和单位

这些系列将收集有关每个符合条件的网络适配器：

| 系列                               | 单位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒的速度的位 |
| `netadapter.bandwidth.outbound`      | 每秒的速度的位 |
| `netadapter.bandwidth.total`         | 每秒的速度的位 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒的速度的位 |
| `netadapter.bandwidth.rdma.outbound` | 每秒的速度的位 |
| `netadapter.bandwidth.rdma.total`    | 每秒的速度的位 |

   > [!NOTE]
   > 网络适配器性能历史记录在每秒的速度，不字节每秒的**位**。 一个 10 GbE 网络适配器可以发送和接收大约 1000000000 位 = 125,000,000 字节 = 1.25 每个 GB 第二个在其理论上的最大值。

## 如何解释

| 系列                               | 如何解释                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 收到的网络适配器的数据的速率。                         |
| `netadapter.bandwidth.outbound`      | 通过网络适配器发送的数据的速率。                             |
| `netadapter.bandwidth.total`         | 数据的总速率接收或由网络适配器发送。           |
| `netadapter.bandwidth.rdma.inbound`  | 通过 RDMA 的网络适配器收到的数据的速率。               |
| `netadapter.bandwidth.rdma.outbound` | 通过 RDMA 网络适配器发送的数据的速率。                   |
| `netadapter.bandwidth.rdma.total`    | 数据的总速率接收或发送 RDMA 网络适配器。 |

## 它们来自

`bytes.*`从收集系列`Network Adapter`性能计数器集在服务器上安装的网络适配器的位置，每个网络适配器的一个实例。

| 系列                           | 源计数器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

`rdma.*`从收集系列`RDMA Activity`性能计数器集在服务器上安装的网络适配器的位置，每个使用启用了 RDMA 的网络适配器的一个实例。

| 系列                               | 源计数器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 ×*以上的总和*   |

   > [!NOTE]
   > 计数器测量通过不采样的整个间隔。 例如，如果网络适配器的空闲时间 9 秒传输 200 位截至第二个，其`netadapter.bandwidth.total`将记录为每秒的速度 20 位平均此 10 秒间隔。 这将确保其性能历史捕获所有活动并就可以防止噪音。

## 在 PowerShell 中的使用情况

使用[Get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## 另请参阅

- [存储空间直通的性能历史记录](performance-history.md)

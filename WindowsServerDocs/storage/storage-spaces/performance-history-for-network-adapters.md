---
title: 适用于网络适配器性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849978"
---
# <a name="performance-history-for-network-adapters"></a>适用于网络适配器性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)详细地介绍了收集的网络适配器的性能历史记录。 网络适配器性能历史记录是可用于在群集中的每个服务器中的每个物理网络适配器。 远程直接内存访问 (RDMA) 性能历史记录是可用于每个物理网络适配器与启用了 RDMA。

   > [!NOTE]
   > 不能为网络适配器已关闭的服务器中收集性能历史记录。 当服务器重新启动后，集合将自动恢复。

## <a name="series-names-and-units"></a>序列名称和单位

这些系列将收集有关每个符合条件的网络适配器：

| 系列                               | 单位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒位数 |
| `netadapter.bandwidth.outbound`      | 每秒位数 |
| `netadapter.bandwidth.total`         | 每秒位数 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒位数 |
| `netadapter.bandwidth.rdma.outbound` | 每秒位数 |
| `netadapter.bandwidth.rdma.total`    | 每秒位数 |

   > [!NOTE]
   > 网络适配器性能历史记录中记录**bits**每秒，而非每秒字节。 一个 10 GbE 网络适配器可以发送和接收大约 1000000000 位 = 125,000,000 字节 = 1.25 GB，每秒最其理论最大值。

## <a name="how-to-interpret"></a>如何解释

| 系列                               | 如何解释                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 网络适配器接收到的数据的速率。                         |
| `netadapter.bandwidth.outbound`      | 网络适配器发送的数据的速率。                             |
| `netadapter.bandwidth.total`         | 数据的总速率接收或发送的网络适配器。           |
| `netadapter.bandwidth.rdma.inbound`  | 收到的网络适配器 RDMA 上运行的数据的速率。               |
| `netadapter.bandwidth.rdma.outbound` | 通过 RDMA 网络适配器发送的数据的速率。                   |
| `netadapter.bandwidth.rdma.total`    | 接收或通过 RDMA 网络适配器发送的数据的总速率。 |

## <a name="where-they-come-from"></a>这些来自

`bytes.*`系列收集从`Network Adapter`设置在服务器上安装的网络适配器的性能计数器，每个网络适配器的一个实例。

| 系列                           | 源计数器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

`rdma.*`系列收集从`RDMA Activity`设置在服务器上安装的网络适配器的性能计数器，每个与启用了 RDMA 的网络适配器的一个实例。

| 系列                               | 源计数器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 ×*之和更高版本*   |

   > [!NOTE]
   > 计数器来度量在整个间隔内，不会被取样。 例如，如果网络适配器处于空闲状态 9 秒，但传输 200 位在 10 秒，其`netadapter.bandwidth.total`将记录为 20 位每秒平均此 10 秒间隔内。 这可确保其性能历史记录捕获所有活动也很可靠到干扰。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[Get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)

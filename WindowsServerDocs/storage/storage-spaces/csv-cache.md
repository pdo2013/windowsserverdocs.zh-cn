---
title: 存储空间直通内存中读取缓存
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122050"
---
# 存储空间直通使用 CSV 内存中读取缓存
> 适用于： Windows Server 2016、 Windows Server 2019

本主题介绍了如何使用系统内存来提高性能的[存储空间直通](storage-spaces-direct-overview.md)。

存储空间直通是与群集共享卷 (CSV) 内存中读取缓存兼容。 使用为缓存读取的系统内存可以提高如 HYPER-V、 使用无缓冲的 I/O 访问 VHD 或 VHDX 文件的应用程序的性能。 （无缓冲的 IOs 都不会缓存通过 Windows 缓存管理器中的任何操作。）

由于内存中缓存是服务器本地，它可提高超聚合存储空间直通部署的数据位置： 缓存最近读取在其中运行虚拟机的相同主机上的内存，减少频率读取转通过网络。 这会导致更低的延迟和更好的存储性能。

## 规划注意事项

在内存中读取缓存是对于读取密集型工作负荷，如虚拟桌面基础结构 (VDI) 的最有效。 相反，如果工作负荷是非常大量的写入操作，缓存可能会引入更多的开销比值，并且应该处于禁用状态。

你可以使用 CSV 内存中读取缓存 80%的总物理内存。

  > [!TIP]
  > 对于超聚合部署，其中计算并存储在相同服务器上运行仔细地保持足够用于虚拟机的内存。 对于聚合的横向扩展文件服务器 (SoFS) 部署，使用较少争夺内存，这不会应用。

  > [!NOTE]
  > 某些 microbenchmarking 工具像 DISKSPD 和[VM 五星](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)可能会产生更糟的是结果使用 CSV 内存中读取缓存比启用而无需它。 默认情况下 VM 五星创建一个 10 钩每个虚拟机 – 大约 1 TiB VHDX 100 虚拟机 – 的总然后执行*统一随机*读取和写入到它们。 与真实的工作负荷，不同读取不遵循任何可预测性或重复模式，因此内存中缓存不有效，只需也会产生开销。

## 配置内存中读取缓存

CSV 内存中读取缓存是 Windows Server 2016 和 Windows Server 2019 中适用于相同的功能。 在 Windows Server 2016 中，它默认是关闭的。 在 Windows Server 2019，它是在默认情况下使用分配的 1 GB。

| 操作系统版本          | 默认 CSV 缓存大小 |
|---------------------|------------------------|
| Windows Server 2016 | 0 （禁用）           |
| Windows Server 2019 | 1 钩                   |

若要查看使用 PowerShell 分配的内存量，请运行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

返回的值是在每个服务器 mebibytes (MiB)。 例如，`1024`表示 1 gibibyte （钩）。

若要更改分配的内存量，修改使用 PowerShell 此值。 例如，若要分配的每个服务器 2 钩，运行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

更改立即生效，暂停，然后恢复 CSV 卷，或服务器之间移动它们。 例如，使用此 PowerShell 代码段将每个 CSV 移动到另一个服务器节点，然后再返回：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## 另请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)

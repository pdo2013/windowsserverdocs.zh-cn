---
title: 存储空间直通内存中读取缓存
ms.prod: windows-server
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 83fc923f505531f955fc0131d7dcc1ce98974daa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394087"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>将存储空间直通与 CSV 内存中读取缓存一起使用
> 适用于：Windows Server 2016、Windows Server 2019

本主题介绍如何使用系统内存来提高[存储空间直通](storage-spaces-direct-overview.md)性能。

存储空间直通与群集共享卷（CSV）内存中读取缓存兼容。 使用系统内存来缓存读取可以提高应用程序的性能，例如 Hyper-v，它使用无缓冲 i/o 来访问 VHD 或 VHDX 文件。 （未缓冲 Io 是 Windows 缓存管理器未缓存的任何操作。）

由于内存中缓存是服务器本地的，因此它会提高超聚合存储空间直通部署的数据位置：最近的读取缓存在虚拟机运行的同一主机上的内存中，从而降低了读取网络的频率。 这会降低延迟和更好的存储性能。

## <a name="planning-considerations"></a>规划注意事项

内存中读取缓存最适用于读取密集型工作负荷，如虚拟桌面基础结构（VDI）。 相反，如果工作负荷非常耗费写入，则缓存可能会引入比值更多的开销，因此应将其禁用。

对于 CSV 内存中读取缓存，最多可以使用 80% 的总物理内存。

  > [!TIP]
  > 对于超聚合部署（其中计算和存储在相同的服务器上运行），请注意为虚拟机留出足够的内存。 对于聚合横向扩展文件服务器（SoFS）部署（内存争夺更少），这不适用。

  > [!NOTE]
  > 某些 microbenchmarking 工具（如 DISKSPD 和[VM 汽油](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)）可能会产生更糟糕的结果，其中启用了 CSV 内存中读取缓存。 默认情况下，VM 汽油每个虚拟机创建 1 10 GiB VHDX – 100 Vm 约有 1 TiB 的总数，然后对它们执行*统一的随机*读取和写入。 与实际工作负载不同，读取不遵循任何可预测或重复的模式，因此内存中缓存不起作用，且只会造成开销。

## <a name="configuring-the-in-memory-read-cache"></a>配置内存中读取缓存

CSV 内存中读取缓存在 Windows Server 2016 和 Windows Server 2019 中提供，具有相同的功能。 在 Windows Server 2016 中，它在默认情况下处于关闭状态。 在 Windows Server 2019 中，默认情况下，它默认为 1 GB。

| 操作系统版本          | 默认 CSV 缓存大小 |
|---------------------|------------------------|
| Windows Server 2016 | 0（已禁用）           |
| Windows Server 2019 | 1 GiB                   |

若要查看使用 PowerShell 分配的内存量，请运行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

返回的值在每个服务器的 mebibytes （MiB）中。 例如，`1024` 表示 1 gibibyte （GiB）。

若要更改分配的内存量，请使用 PowerShell 修改此值。 例如，若要为每台服务器分配2个 GiB，请运行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

要使更改立即生效，请暂停并恢复 CSV 卷，或在服务器之间移动它们。 例如，使用此 PowerShell 片段将每个 CSV 移到另一个服务器节点，并再次返回：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)

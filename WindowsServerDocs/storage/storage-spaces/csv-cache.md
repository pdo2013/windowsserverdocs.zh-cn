---
title: 存储空间直通的内存中读取缓存
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850548"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>使用存储空间直通中内存 CSV 读取缓存
> 适用于：Windows Server 2016 中，Windows Server 2019

本主题介绍如何使用系统内存来提高的性能[存储空间直通](storage-spaces-direct-overview.md)。

存储空间直通适用于群集共享卷 (CSV) 的内存中读取缓存。 使用系统内存缓存读取到可以提高应用程序，如 HYPER-V，使用访问 VHD 或 VHDX 文件的形式无缓冲的 I/O 的性能。 （无缓冲的 IOs 是不会缓存由 Windows 缓存管理器的任何操作。）

由于内存中缓存是服务器本地，它提高了超聚合存储空间直通部署的数据局部性： 最新的读取进行缓存，在其中运行虚拟机在同一主机上的内存，减少了何种频率读取绕过网络。 这会导致较低的延迟和提高存储性能。

## <a name="planning-considerations"></a>规划注意事项

在内存中读取缓存是最有效的是读取密集型工作负荷，例如虚拟桌面基础结构 (VDI)。 相反，如果工作负荷非常大量写入操作，则缓存将可能会带来更多的开销比值，应禁用。

您可以使用最多 80%的总物理内存中内存 CSV 读取缓存。

  > [!TIP]
  > 对于超聚合部署，其中计算并存储在同一服务器上运行仔细地保留足够的内存用于虚拟机。 对于聚合的横向扩展文件服务器 (SoFS) 部署，使用内存，减少争用，这不适用于。

  > [!NOTE]
  > 等 DISKSPD 某些 microbenchmarking 工具和[VM Fleet](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)可能会产生更糟糕的结果与 CSV 内存中已启用读缓存比没有它。 默认情况下 VM Fleet 将创建一个 10 GiB 每个虚拟机 – 大约 1 TiB 的 VHDX 总计为 100 Vm，然后执行*均匀随机*读取和写入到它们。 与不同的实际工作负荷，读取操作不遵循任何可预测性或重复的模式，因此内存中缓存不有效，就可能会产生开销。

## <a name="configuring-the-in-memory-read-cache"></a>配置中内存的读取缓存

在内存 CSV 读取缓存已在 Windows Server 2016 和 Windows Server 2019 具有相同的功能。 在 Windows Server 2016 中，它默认处于关闭状态。 在 Windows Server 2019，它是默认情况下分配 1 gb。

| 操作系统版本          | 默认 CSV 缓存大小 |
|---------------------|------------------------|
| Windows Server 2016 | 0 （禁用）           |
| Windows Server 2019 | 1 GiB                   |

若要查看使用 PowerShell 分配内存量，请运行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

返回的值是在每个服务器 mebibytes (MiB)。 例如，`1024`表示 1 gibibyte (GiB)。

若要更改分配的内存量，请修改此值使用 PowerShell。 例如，若要分配 2 GiB 每台服务器，请运行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

更改会立即生效，暂停然后恢复你的 CSV 卷，或服务器之间移动它们。 例如，使用此 PowerShell 代码段将每个 CSV 移到另一个服务器节点，然后再返回：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)

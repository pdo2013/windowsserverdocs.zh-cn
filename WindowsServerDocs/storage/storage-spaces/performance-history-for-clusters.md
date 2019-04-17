---
title: 群集的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894316"
---
# <a name="performance-history-for-clusters"></a>群集的性能历史记录

> 应用于： Windows Server 内幕预览

[性能历史记录存储空格直接](performance-history.md)此子主题介绍收集的群集性能历史记录。

有源自在群集级别没有系列。 而是 server 系列，如`clusternode.cpu.usage`，群集中的所有服务器的聚合。 卷系列，如`volume.iops.total`，群集中的所有卷的聚合。 和驱动器系列，如`physicaldisk.size.total`，群集中的所有驱动器聚合。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用情况

使用[Get-群集](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster)cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>另请参阅

- [性能的存储空间直接的历史记录](performance-history.md)

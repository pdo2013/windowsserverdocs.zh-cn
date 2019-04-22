---
title: 对于群集的性能历史记录
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 存储空间直通
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818768"
---
# <a name="performance-history-for-clusters"></a>对于群集的性能历史记录

> 适用于：Windows Server Insider Preview

此子主题[的存储空间直通的性能历史记录](performance-history.md)介绍为群集收集的性能历史记录。

没有序列在群集级别发出的。 相反，server 系列，如`clusternode.cpu.usage`，聚合的群集中的所有服务器。 卷序列，如`volume.iops.total`，聚合了在群集中的所有卷。 和推动系列，如`physicaldisk.size.total`，群集中的所有驱动器的聚合。

## <a name="usage-in-powershell"></a>在 PowerShell 中的用法

使用[Get 群集](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster)cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>请参阅

- [有关存储空间直通的性能历史记录](performance-history.md)

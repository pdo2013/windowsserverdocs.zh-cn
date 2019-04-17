---
title: "运行状况服务操作"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>运行状况服务操作

> 适用于 Windows Server 2016

运行状况服务是 Windows Server 2016，以便改进的日常监视和运营群集运行存储空间直通体验中的新增功能。

## <a name="actions"></a>操作  

下一步部分介绍工作流，它将自动通过运行状况服务。 若要检查，实际上正在自主，采取措施或跟踪它的进度或结果，运行状况服务生成"操作"。 与日志，不同操作消失不久后，完成后主要用于提供深入日常活动，其中可能会影响性能或容量 （例如还原复原或重新平衡数据）。  

### <a name="usage"></a>使用情况  

一个新的 PowerShell cmdlet 显示所有操作：  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>覆盖率  

在 Windows Server 2016，**获取 StorageHealthAction** cmdlet 可以返回任何以下信息：  

-   即将停用失败的、 利润连接或响应物理磁盘  

-   切换为使用更换物理磁盘的存储池中  

-   还原完整复原数据  

-   重新平衡存储池中  

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)
- [开发人员的文档、 示例代码和 MSDN 上的 API 参考](https://msdn.microsoft.com/windowshealthservice)

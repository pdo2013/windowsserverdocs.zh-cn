---
title: 运行状况服务操作
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843018"
---
# <a name="health-service-actions"></a>运行状况服务操作

> 适用于 Windows Server 2016

运行状况服务是一项新功能在 Windows Server 2016，可提高日常监视和运行存储空间直通的群集的操作体验。

## <a name="actions"></a>操作  

下一部分介绍运行状况服务自动化的工作流。 要验证确实正在自主执行一项操作，或者要跟踪其进度或结果，运行状况服务会生成“操作”。 与日志不同，完成后不久操作会消失，并且主要用于深入了解正在进行的活动，这些活动可能会影响性能或容量（例如还原复原或重新平衡数据）。  

### <a name="usage"></a>用法  

一个新的 PowerShell cmdlet 会显示所有操作：  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>覆盖范围  

在 Windows Server 2016 **Get-storagehealthaction** cmdlet 可以返回任何以下信息：  

-   停用失败、断开连接或未响应的物理磁盘  

-   切换存储池以使用替代物理磁盘  

-   还原数据的完全复原能力  

-   重新平衡存储池  

## <a name="see-also"></a>请参阅

- [Windows Server 2016 中的运行状况服务](health-service-overview.md)
- [开发人员文档、 示例代码和 MSDN 上的 API 参考](https://msdn.microsoft.com/windowshealthservice)

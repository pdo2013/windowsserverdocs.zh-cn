---
title: "配置服务器的存储"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>配置服务器的存储

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>示例硬盘配置  
 下表建议配置示例硬盘。 评估基于典型用法和功能，但它们不能解决问题的影响获得最佳性能。 你可以使用任何类型的受支持硬盘针对这些配置 （如 SATA 或 SCSI） 基于首选项和客户的需求。  
  
> [!IMPORTANT]
>   Windows Server Essentials 必须安装作为 c： 音量和卷的大小必须至少 60 GB。 建议你在您的操作系统磁盘上创建两个分区不使用 c: （系统分区） 存储业务的任何数据。  
  
|服务器级别|磁盘配置|  
|------------------|------------------------|  
|输入|-两个物理磁盘<br /><br /> -作为包含以下镜像 RAID 1 集配置：<br /><br /> -C： 音量？ 60 GB<br /><br /> -D： 音量？ 1000 GB|  
|中等|-三个物理磁盘<br /><br /> -配置包含以下 RAID 5 设置：<br /><br /> -C： 音量？ 60 GB<br /><br /> -D： 音量？ 1500 GB|  
|高|-5 个或多个总物理磁盘<br /><br /> 的 RAID 1 中两个磁盘镜像集，其中包含 c： 音量？ 100 GB<br /><br /> 的包含以下 RAID 5 组中所有剩余磁盘：<br /><br /> -D： 音量？ 1500 GB<br /><br /> -E： 音量？ 1500 GB|  
  
 这些建议考虑在内，已安装的操作系统的大小，平均服务器使用的数据存储和预期的数据存储增长，通过整个使用期限内服务器的大小。 卷可能在单个物理磁盘分区或将它们放在单独的物理磁盘上。 因为服务器上存储的重要数据为您的客户，建议你使用多个物理磁盘和使用帮助保护你的客户的数据 RAID 或存储空间的硬件。  
  
## <a name="configuring-your-server-backup"></a>配置服务器备份  
 在服务器上内部硬盘，除了客户应该考虑使用外部 USB 硬盘进行备份。 理想的情况下，的客户必须具有足够备份在服务器上的所有数据的能力至少两个外部硬盘。 如果使用外部硬盘，的客户可能需要一个磁盘离线每个晚上，若要进一步保护数据。  
  
## <a name="partition-configuration"></a>分区配置  
 在初始配置为服务器中，一套包含共享的文件夹和客户端计算机备份文件夹中的默认服务器文件夹磁盘 0 上最大的数据分区中创建。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)


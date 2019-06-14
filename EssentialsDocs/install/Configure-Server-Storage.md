---
title: 配置服务器存储
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433743"
---
# <a name="configure-server-storage"></a>配置服务器存储

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>示例硬盘配置  
 下表给出了示例硬盘配置的建议。 估计值基于典型用途和功能，但不说明影响最佳性能的问题。 你可以根据客户的喜好和需要对这些配置（如 SATA 或 SCSI）使用任意类型的受支持硬盘。  
  
> [!IMPORTANT]
>   必须在 Windows Server Essentials 安装为 c： 卷和卷大小必须至少为 60 GB。 建议在操作系统磁盘上创建两个分区，并且不使用 C:（系统分区）存储任何业务数据。  
  
|服务器级别|磁盘配置|  
|------------------|------------------------|  
|条目|-两个物理磁盘<br /><br /> -配置为 RAID 1 镜像集，其中包含以下各项：<br /><br /> -C： 卷？ 60 GB<br /><br /> -D： 卷？ 1000 GB|  
|中等|-三个物理磁盘<br /><br /> -配置为 RAID 5 集，其中包含以下各项：<br /><br /> -C： 卷？ 60 GB<br /><br /> -D： 卷？ 1500 GB|  
|高|-五个或多个总的物理磁盘<br /><br /> -两个磁盘位于 RAID 1 镜像集，其中包含 c： 卷？ 100 GB<br /><br /> 的包含以下的 RAID 5 集中所有剩余磁盘：<br /><br /> -D： 卷？ 1500 GB<br /><br /> -E： 卷？ 1500 GB|  
  
 这些建议考虑到已安装的操作系统的大小、服务器使用的数据存储的平均大小，以及在服务器的整个生命周期内预期的数据存储增长情况。 卷可以是单个物理磁盘上的分区，也可以置于单独的物理磁盘上。 由于服务器上为您的客户存储重要数据，建议你使用多个物理磁盘，并帮助保护你的客户的数据通过使用硬件 RAID 或存储空间。  
  
## <a name="configuring-your-server-backup"></a>配置服务器备份  
 除了服务器上的内部硬盘以外，客户还应考虑使用 USB 移动硬盘进行备份。 理想情况下，客户应至少有两个移动硬盘，其容量足以备份服务器上的所有数据。 如果使用移动硬盘，则客户可以每晚将一个硬盘带离现场，以进一步保护数据。  
  
## <a name="partition-configuration"></a>分区配置  
 在服务器的初始配置过程中，会在磁盘 0 上最大的数据分区中创建包含共享文件夹和客户端计算机备份文件夹的默认服务器文件夹集。  
  
## <a name="see-also"></a>请参阅  

 [Windows Server Essentials ADK 入门](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [Windows Server Essentials ADK 入门](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [部署准备的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)


---
title: "添加健康通知"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>添加健康通知

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

健康加载项提供定义警报、健康检查和修复网络问题。 健康加载项包含为图像添加批注的代码或用于评估健康信息特定功能的数据的 xml 文件。 健康外接程序创建由开发人员和管理员服务器和客户端计算机上安装。  
  
 请参考[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)有关创建的健康加载项的详细信息。  
  
## <a name="installing-health-add-in-files"></a>安装健康外接程序文件  
 开发人员创建了 xml 文件后，你必须在相应的服务器和客户端计算机上的位置将文件的副本。  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>若要安装的服务器上的 xml 文件  
  
1.  在**%ProgramFiles%\ Windows Server \Bin\Feature 定义**文件夹，创建一个名为新文件夹**MyHealthAddIn**。 还可以赋予此文件夹任何名称。 我们建议文件夹的名称将功能名称相同。  
  
2.  将 Definition.xml 和 Definition.xml.config 文件复制到新文件夹。  
  
3.  如果创建条件或多个操作为二进制文件时，你还应该复制这些文件来**%ProgramFiles%\ Windows Server \Bin**。  
  
 客户端计算机运行到适当位置提取原色 XML 文件了计划的任务每 6 小时。 你可以手动运行该任务强制服务器客户端计算机之间同步。  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>若要安装 xml 文件的客户端计算机上  
  
1.  打开任务计划程序。  
  
2.  运行**HealthDefintionUpdateTask**任务计划程序中。  
  
    > [!NOTE]
    >  此任务中不安装二进制文件。 你必须手动复制到二进制文件**%ProgramFiles%\ Windows Server \Bin**客户端计算机上的文件夹。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)
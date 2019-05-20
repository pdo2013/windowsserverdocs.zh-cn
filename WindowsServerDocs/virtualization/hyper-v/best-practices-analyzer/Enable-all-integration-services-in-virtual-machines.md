---
title: 启用虚拟机中的所有集成服务
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829428"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>启用虚拟机中的所有集成服务

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*一个或多个集成服务已禁用或虚拟机中无法正常工作。*  
  
## <a name="impact"></a>影响  
  
*对以下虚拟机，可能无法正常运行的服务或集成功能：*  
  
\<虚拟机名称的列表 >  
  
## <a name="resolution"></a>分辨率  
  
*使用服务管理单元或 sc config 命令行工具来验证服务配置为自动启动，并且不会停止运行。*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>配置服务应该如何启动使用服务管理单元  
  
1.  使用远程桌面服务或虚拟机连接到来宾操作系统上连接到虚拟机并登录。  
  
2.  打开“服务”。 (单击**启动**，在单击**开始搜索**框中，键入**services.msc**，然后按 ENTER。)  
  
3.  在详细信息窗格中，右键单击你想要配置，然后单击该服务**属性**。  
  
4.  上**常规**选项卡上，在**启动**键入，单击**自动**。  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>若要配置服务应该如何使用启动 SC Config  
  
1.  打开 Windows PowerShell。 (从桌面上，单击**启动**，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  将 < 服务名称 > 替换为与服务的名称，然后键入：  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  



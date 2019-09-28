---
title: 应将 Hyper-v 虚拟机管理服务配置为自动启动
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f35f94a815e9f895f7f7690737b6b8fb2bed82e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393416"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>应将 Hyper-v 虚拟机管理服务配置为自动启动

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  

在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。

## <a name="issue"></a>问题  
  
*Hyper-v 虚拟机管理服务未配置为自动启动。*  
  
## <a name="impact"></a>影响  
  
*启动服务之前，不能管理虚拟机。*  
  
运行的虚拟机将继续运行。 但是，在服务运行之前，你将无法管理虚拟机，也无法创建或删除它们。  
  
## <a name="resolution"></a>分辨率  
  
*使用 "服务" 管理单元或 sc config 命令行工具将服务重新配置为自动启动。*  
  
> [!TIP]  
> 如果在桌面应用中找不到该服务，或命令行工具报告该服务不存在，则可能未安装 Hyper-v 管理工具。 若要安装它们：  
>   
> - 在 Windows Server 上，打开服务器管理器，并使用 "添加角色和功能向导"。 有关更多详细信息，请参阅[在 Windows Server 2016 上安装 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
> - 在 Windows 上，从桌面开始键入**程序**，单击 "**程序和功能**" （控制面板） >**打开或关闭 Windows 功能** > **hyper-v** > **hyper-v 管理工具**。 然后单击 **"确定"** 。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>将服务重新配置为使用 "服务" 桌面应用程序自动启动  
  
1.  打开 "服务" 桌面应用。 （单击 "**开始**"，在搜索框中单击，开始键入 "**服务**"，然后在结果列表中单击 "服务"。  
  
2.  在详细信息窗格中，右键单击 " **Hyper-v 虚拟机管理**"，然后单击 "**属性**"。  
  
3.  在 "**常规**" 选项卡上，在 "**启动**类型" 中单击 "**自动**"。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>将服务重新配置为使用 SC Config 命令自动启动  
  
1.  打开 Windows PowerShell。  
  
2.  键入：  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  如果该服务尚未运行，请键入：  
  
    ```  
    start-service -name vmms  
    ```  
  



---
title: HYPER-V 虚拟机管理服务应配置为自动启动
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c33f81678d7fdc71e81834a002fd3d7917a6f632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833248"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>HYPER-V 虚拟机管理服务应配置为自动启动

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
  
*HYPER-V 虚拟机管理服务未配置为自动启动。*  
  
## <a name="impact"></a>影响  
  
*不能管理虚拟机，直到该服务已启动。*  
  
正在运行的虚拟机将继续运行。 但是，你将无法管理虚拟机，或创建或删除它们，直到该服务正在运行。  
  
## <a name="resolution"></a>分辨率  
  
*使用服务管理单元或 sc config 命令行工具重新配置为自动启动该服务。*  
  
> [!TIP]  
> 如果找不到该服务在桌面应用中或命令行工具将报告，该服务不存在，HYPER-V 管理工具可能不会安装。 若要安装它们：  
>   
> - Windows Server 上打开服务器管理器并使用添加角色和功能向导。 有关更多详细信息，请参阅[Windows Server 2016 上安装 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
> - 在 Windows 桌面上，从上开始键入**程序**，单击**程序和功能**（控制面板） >**打开或关闭打开的 Windows 功能** >  **Hyper V** > **HYPER-V 管理工具**。 然后，单击**确定**。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要重新配置服务以开始自动使用服务的桌面应用程序  
  
1.  打开服务桌面应用程序。 (单击**启动**，单击搜索框中，键入**服务**，然后单击在结果列表中的服务。  
  
2.  在细节窗格中，右键单击**HYPER-V 虚拟机管理**，然后单击**属性**。  
  
3.  上**常规**选项卡上，在**启动**键入，单击**自动**。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>若要重新配置服务以开始自动使用 SC Config 命令  
  
1.  打开 Windows PowerShell。  
  
2.  键入：  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  如果该服务未运行，请键入：  
  
    ```  
    start-service -name vmms  
    ```  
  



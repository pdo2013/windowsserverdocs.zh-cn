---
title: 必须运行的 HYPER-V 虚拟机管理服务
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826108"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>必须运行的 HYPER-V 虚拟机管理服务

>适用于：Windows Server 2016
  
有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|先决条件|  

在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。

## <a name="issue"></a>问题  
  
*管理虚拟机所需的服务未运行。*  
  
## <a name="impact"></a>影响  
  
*可以不执行任何虚拟机管理操作。*  
  
正在运行的虚拟机将继续运行。 但是，你将无法管理虚拟机，或创建或删除它们，直到该服务正在运行。  
  
## <a name="resolution"></a>分辨率  
  
*使用服务管理单元中或 Sc config 命令行工具重新配置为自动启动该服务。*  
  
> [!TIP]  
> 如果找不到该服务在桌面应用中或命令行工具将报告，该服务不存在，HYPER-V 管理工具可能不会安装。 并且，如果您不能看到从开始菜单上的 HYPER-V MMC 控制台，则应安装 HYPER-V 管理工具。

若要安装 HYPER-V 管理工具：  
>   
> - Windows Server 上打开服务器管理器并使用添加角色和功能向导。 有关更多详细信息，请参阅[Windows Server 2016 上安装 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  此外可以使用 PowerShell 来安装这些工具 (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - 在 Windows 桌面上，从上开始键入**程序**，单击**程序和功能**（控制面板） >**打开或关闭打开的 Windows 功能** >  **Hyper V** > **HYPER-V 管理工具**。 然后，单击**确定**。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要重新配置服务以开始自动使用服务的桌面应用程序  
  
1.  打开服务桌面应用程序。 (单击**启动**，在单击**开始搜索**框中，键入**services.msc**，然后按 ENTER。)  
  
2.  在细节窗格中，右键单击**HYPER-V 虚拟机管理**，然后单击**属性**。  
  
3.  上**常规**选项卡上，在**启动**键入，单击**自动**。  
  
4.  若要启动服务，请单击**启动**。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>若要重新配置服务以开始自动使用 SC Config  
  
1.  打开 Windows PowerShell。 (从桌面上，单击**启动**，然后开始键入**Windows PowerShell**。)  
  
2.  右键单击**Windows PowerShell**然后单击**以管理员身份运行**。  
  
3.  若要重新配置该服务，请键入：  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  若要启动该服务，请键入：  
  
    ```  
    sc start vmms  
    ```  
  
如果该服务已配置为自动启动，并且只需重新启动服务，可以执行的操作，从 HYPER-V 管理器，或从上面显示的"sc start vmms"命令。  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>若要重新启动 Hyper-v 管理器中的服务  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”**，指向 **“管理工具”**，然后单击 **“Hyper-V 管理器”**。  
  
2.  在导航窗格中，单击服务器的名称，如果没有选中它。  
  
3.  在中**操作**窗格中，单击**启动服务**。  
  



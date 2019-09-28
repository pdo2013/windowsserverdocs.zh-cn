---
title: Hyper-v 虚拟机管理服务必须正在运行
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: de1e2ed9fc24afe7d1ccc12bc11eb94a846f0664
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364677"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Hyper-v 虚拟机管理服务必须正在运行

>适用于：Windows Server 2016
  
有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|Error|  
|**类别**|先决条件|  

在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。

## <a name="issue"></a>问题  
  
*管理虚拟机所需的服务未运行。*  
  
## <a name="impact"></a>影响  
  
*不能执行虚拟机管理操作。*  
  
运行的虚拟机将继续运行。 但是，在服务运行之前，你将无法管理虚拟机，也无法创建或删除它们。  
  
## <a name="resolution"></a>分辨率  
  
*使用 "服务" 管理单元或 Sc config 命令行工具将服务重新配置为自动启动。*  
  
> [!TIP]  
> 如果在桌面应用中找不到该服务，或命令行工具报告该服务不存在，则可能未安装 Hyper-v 管理工具。 如果在 "开始" 菜单中看不到 Hyper-v MMC 控制台，则应安装 Hyper-v 管理工具。

若要安装 Hyper-v 管理工具：  
>   
> - 在 Windows Server 上，打开服务器管理器，并使用 "添加角色和功能向导"。 有关更多详细信息，请参阅[在 Windows Server 2016 上安装 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  你还可以使用 PowerShell 安装工具（`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`） 
> - 在 Windows 上，从桌面开始键入**程序**，单击 "**程序和功能**" （控制面板） >**打开或关闭 Windows 功能** > **hyper-v** > **hyper-v 管理工具**。 然后单击 **"确定"** 。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>将服务重新配置为使用 "服务" 桌面应用程序自动启动  
  
1.  打开 "服务" 桌面应用。 （单击 "**开始**"，在 "**开始搜索**" 框中单击，键入**services.msc**，然后按 enter。）  
  
2.  在详细信息窗格中，右键单击 " **Hyper-v 虚拟机管理**"，然后单击 "**属性**"。  
  
3.  在 "**常规**" 选项卡上，在 "**启动**类型" 中单击 "**自动**"。  
  
4.  若要启动该服务，请单击 "**启动**"。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>将服务重新配置为使用 SC Config 自动启动  
  
1.  打开 Windows PowerShell。 （在桌面上，单击 "**开始**"，然后开始键入**Windows PowerShell**。）  
  
2.  右键单击 " **Windows PowerShell** "，然后单击 "**以管理员身份运行**"。  
  
3.  若要重新配置服务，请键入：  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  若要启动该服务，请键入：  
  
    ```  
    sc start vmms  
    ```  
  
如果该服务已配置为自动启动，而你只需要重新启动该服务，则可以从 Hyper-v 管理器或上面所示的 "sc start vmms" 命令执行该操作。  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>从 Hyper-v 管理器重新启动服务  
  
1.  打开 Hyper-V 管理器。 单击 **“开始”** ，指向 **“管理工具”** ，然后单击 **“Hyper-V 管理器”** 。  
  
2.  在导航窗格中，单击服务器的名称（如果尚未选择）。  
  
3.  在 "**操作**" 窗格中，单击 "**启动服务**"。  
  



---
title: 识别并解决远程访问服务器操作问题
description: 本主题是 Windows Server 2016 中的远程访问监视和记帐指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: db10f784f383938edb29b18d7e8febf869378abc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404571"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>识别并解决远程访问服务器操作问题

>适用于：Windows Server（半年频道）、Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
你可以使用以下过程来确定远程访问服务器操作问题、其根本原因和解决问题所需的解决方法。  
  
> [!NOTE]  
> 您必须以 Domain Admins 组成员或每台计算机上 Administrators 组的成员身份登录才能完成本主题中所述的任务。 如果你在使用作为 Administrators 组成员的帐户登录时无法完成任务，请尝试在使用域管理员组的成员帐户登录时执行此任务。  
  
本主题包括有关执行以下任务的信息：  
  
- 模拟操作问题  
  
- 确定操作问题并采取纠正措施  
  
- 还原 IP Helper 服务  
  
### <a name="BKMK_Simulate"></a>模拟操作问题  
  
> [!CAUTION]  
> 由于你的远程访问服务器可能已正确配置且没有遇到任何问题，因此你可以使用以下过程来模拟操作问题。 如果你的服务器当前在生产环境中为客户端提供服务，则你可能不想在此时执行这些操作。 您可以通过阅读这些步骤来了解如何处理将来可能在远程访问服务器上出现的问题。  
  
IP Helper 服务（IPHlpSvc）托管 IPv6 转换技术（如 IP-HTTPS、6to4 或 Teredo），并且 DirectAccess 服务器正常工作需要它。 若要演示远程访问服务器上的模拟操作问题，必须停止（IPHlpSvc）网络服务。  
  
##### <a name="to-stop-the-ip-helper-service"></a>停止 IP Helper 服务  
  
1.  在远程访问服务器的 "**开始**" 屏幕上，单击 "**管理工具**"，然后双击 "**服务**"。  
  
2.  在**服务**列表中，向下滚动并右键单击 " **IP 帮助**程序"，然后单击 "**停止**"。  
  
### <a name="BKMK_Identify"></a>确定操作问题并采取纠正措施  
关闭 IP Helper 服务将导致远程访问服务器上出现严重错误。 监视仪表板将显示服务器的操作状态和问题的详细信息。  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>确定详细信息并采取纠正措施  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击“仪表板”以导航到“远程访问管理控制台”中的“远程访问仪表板”。  
  
3.  请确保已在左窗格中选择远程访问服务器，然后在中间窗格中单击 "**操作状态**"。  
  
4.  你将看到带有绿色或红色图标的组件列表，指示其操作状态。 在列表中单击 " **ip-https" 行**。 选择某一行后，**详细**信息窗格中会显示该操作的详细信息，如下所示：  
  
    **错误**  
  
    IP Helper 服务（IPHlpSvc）已停止。 DirectAccess 可能无法按预期方式工作。 IP Helper 服务使用连接平台、IPv6 转换技术和 ip-https 提供隧道连接。  
  
    **导致**  
  
    1.  IP Helper 服务已停止。  
  
    2.  IP Helper 服务未响应。  
  
    **解决方法**  
  
    1.  若要确保服务正在运行，请在 Windows PowerShell 提示符下键入**get-help iphlpsc** 。  
  
    2.  若要启用该服务，请在提升的 Windows PowerShell 提示符下键入**Start-service iphlpsvc** 。  
  
    3.  若要重新启动该服务，请在提升的 Windows PowerShell 提示符下键入**restart-service iphlpsvc** 。  
  
### <a name="BKMK_Restart"></a>还原 IP Helper 服务  
若要在远程访问服务器上还原 IP Helper 服务，可以按照上述解决方法步骤来启动或重新启动该服务，或者可以使用以下过程反转用于模拟 IP Helper 服务故障的过程。  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>在远程访问服务器上重新启动 IP Helper 服务  
  
1.  在 "**开始**" 屏幕上，单击 "**管理工具**"，然后双击 "**服务**"。  
  
2.  在**服务**列表中，向下滚动并右键单击 " **IP 帮助**程序"，然后单击 "**启动**"。  
  
@no__t 0Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  



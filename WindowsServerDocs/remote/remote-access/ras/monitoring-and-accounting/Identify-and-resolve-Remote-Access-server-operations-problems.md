---
title: 识别并解决远程访问服务器操作问题
description: 本主题是指南的适用于远程访问监视和记帐 Windows Server 2016 中的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c58ff97e286f91610a321801d177a2977349fa78
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840458"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>识别并解决远程访问服务器操作问题

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 与路由及远程访问服务 (RRAS) 合并到了单个远程访问角色中。  
  
你可以使用以下过程来确定远程访问服务器操作问题、 其根本原因和修复问题所需的分辨率。  
  
> [!NOTE]  
> 您必须登录以 Domain Admins 组的成员或每台计算机上 Administrators 组的成员才能完成本主题中所述的任务。 如果已使用该帐户是 Administrators 组的成员登录时，无法完成任务，请尝试执行该任务，而是 Domain Admins 组的成员帐户登录。  
  
本主题包含有关执行以下任务的信息：  
  
- 模拟操作问题  
  
- 确定操作问题并采取纠正措施  
  
- 还原 IP 帮助程序服务  
  
### <a name="BKMK_Simulate"></a>模拟操作问题  
  
> [!CAUTION]  
> 由于远程访问服务器可能是配置不正确并遇到任何问题，可以使用以下过程以模拟操作问题。 如果你的服务器当前为生产环境中的客户端提供服务，可能不希望这一次执行以下操作。 相反，你可以读取完成的步骤了解如何解决可能出现在远程访问服务器在将来的问题。  
  
IP 帮助程序服务 (IPHlpSvc) 的主机 IPv6 转换技术 （如 IP-HTTPS、 6to4 或 Teredo），并且它是必需的 DirectAccess 服务器可以正常工作。 若要演示远程访问服务器上的模拟的操作问题，必须停止 (IPHlpSvc) 网络服务。  
  
##### <a name="to-stop-the-ip-helper-service"></a>若要停止 IP 帮助程序服务  
  
1.  上**启动**屏幕的远程访问服务器中，单击**管理工具**，然后双击**Services**。  
  
2.  在列表中**Services**，向下滚动并右键单击**IP 帮助程序**，然后单击**停止**。  
  
### <a name="BKMK_Identify"></a>确定操作问题并采取纠正措施  
远程访问服务器上，关闭 IP 帮助程序服务将导致一个严重的错误。 在监视仪表板将显示在服务器的操作状态和问题的详细信息。  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>若要标识详细信息并采取纠正措施  
  
1.  在“服务器管理器”中，单击“工具”，然后单击“远程访问管理”。  
  
2.  单击“仪表板”以导航到“远程访问管理控制台”中的“远程访问仪表板”。  
  
3.  请确保在左侧窗格中，选择您的远程访问服务器，然后在中间窗格中，单击**操作状态**。  
  
4.  您将看到带有绿色或红色图标，指示其操作状态的组件的列表。 单击**IP-HTTPS**列表中的行。 当选中某行时，该操作的详细信息所示**详细信息**窗格中的，如下所示：  
  
    **错误**  
  
    IP 帮助程序服务 (IPHlpSvc) 已停止。 DirectAccess 可能无法按预期运行。 IP 帮助程序服务提供通过使用连接平台、 IPv6 转换技术和 IP-HTTPS 隧道连接。  
  
    **原因**  
  
    1.  IP 帮助程序服务已停止。  
  
    2.  IP 帮助程序服务未响应。  
  
    **解决方法**  
  
    1.  若要确保服务正在运行，请键入**Get-service iphlpsc**在 Windows PowerShell 提示符下。  
  
    2.  若要启用该服务，请键入**Start-service iphlpsvc**从提升的 Windows PowerShell 提示符。  
  
    3.  若要重新启动该服务，请键入**重新启动服务 iphlpsvc**从提升的 Windows PowerShell 提示符。  
  
### <a name="BKMK_Restart"></a>还原 IP 帮助程序服务  
若要还原远程访问服务器上的 IP 帮助程序服务，您可以执行上述启动或重新启动该服务，解决方法步骤，或者可以使用以下过程来执行相反的过程，用于模拟 IP 帮助程序服务失败。  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>若要重新启动远程访问服务器上的 IP 帮助程序服务  
  
1.  上**启动**屏幕上，单击**管理工具**，然后双击**Services**。  
  
2.  在列表中**Services**，向下滚动并右键单击**IP 帮助程序**，然后单击**启动**。  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  



---
title: "解决了连接到服务器 Windows Server Essentials 中的计算机"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>解决了连接到服务器 Windows Server Essentials 中的计算机

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
 本主题包含疑难解答指南将计算机连接到运行的 Windows Server Essentials 或 Windows Server Essentials 服务器时可能遇到的问题。  
  
> [!NOTE]
>  最新的疑难解答信息的 Windows Server Essentials 和 Windows Server Essentials 从社区，我们建议你访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials)。 Windows Server Essentials 论坛是一个不错的进行搜索以获取帮助，或者提出问题。  
  
 本主题提供的以下问题的解决方案：  
  

-   问题 1:[发出 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   2 的问题：[发出 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   3 平板电脑的问题：[发出 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   问题 4:[发出 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   问题 5:[发出 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   问题 6:[发出 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   问题 7:[发出 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   问题 8:[发出 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   问题 9:[发出 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   10 的问题：[发出 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   问题 11:[发出 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   问题 1:[发出 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   2 的问题：[发出 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   3 平板电脑的问题：[发出 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   问题 4:[发出 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   问题 5:[发出 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   问题 6:[发出 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   问题 7:[发出 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   问题 8:[发出 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   问题 9:[发出 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   10 的问题：[发出 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   问题 11:[发出 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>1 问题  
 **问题**  
  
 我收到安装未成功一个包。 请尝试重新安装 Windows Server Essentials 接头。 如果问题仍然存在，请联系你的网络管理员错误计算机连接到服务器时  
  
 **描述**  
  
 如果计算机连接到服务器而其他 Windows 更新或应用程序安装处于挂起状态并取消接头安装运行 Windows Server Essentials，则可能会发生此问题。  
  
 **解决方案**  
  
 其他所有更新或应用程序安装都完成。 如果出现提示，请重启计算机。  
  
##  <a name="BKMK_ConnectorIssue2"></a>2 的问题  
 **问题**  
  
 不能加入 Windows Server Essentials 到一台计算机  
  
 **描述**  
  
 在计算机名称有非 ASCII 字符的计算机不能加入 Windows Server Essentials。 如果计算机名称包括非 ASCII 字符，你会收到错误消息"出现错误。"  
  
 **解决方案**  
  
 重命名客户端计算机名称中包含 ASCII 字符仅，并尝试重新添加到 Windows Server Essentials 计算机。  
  
##  <a name="BKMK_ConnectorIssue2a"></a>3 平板电脑的问题  
 **问题**  
  
 安装软件接头已取消时收到错误计算机连接到服务器  
  
 **描述**  
  
 能够将计算机连接到服务器，系统帐户必须具有服务器文件夹显示在 Windows Server Essentials 仪表板上的完整控制权限。 如果未被授予所需的权限，你会收到"取消接头软件安装"的错误消息。  
  
 **解决方案**  
  
 系统帐户授予**完全控制**权限的每个 server 文件夹。  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>授予系统帐户服务器文件夹完全控制权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击**存储**，然后单击**服务器文件夹**。  
  
3.  服务器文件夹中，右键单击，然后单击**打开该文件夹**。 (如果你没有看到**打开该文件夹**选项，则你不需要的文件夹设置权限。)  
  
4.  在网络路径顶部的 Windows 资源管理器中，单击要共享的文件夹显示服务器上的服务器共享。 例如，如果路径**网络 > Server01 > 文件历史记录备份**，单击**Server01**。  
  
5.  一个服务器文件夹中，右键单击，然后单击**属性**。  
  
6.  单击**安全**选项卡。  
  
7.  如果**完全控制**是不得系统帐户，请单击**编辑**，然后单击**系统**。 下**系统的权限**、选择**允许**旁边的复选框**完全控制**。  
  
8.  单击**确定**两次，更新应用的权限，并关闭**属性**。  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>4 问题  
 **问题**  
  
 获取到运行此应用程序，你必须安装一个以下版本的.NET Framework: V4.5.50709"错误在将计算机连接到服务器时  
  
 **描述**  
  
 当将计算机连接到运行的 Windows Server Essentials 或 Windows Server Essentials 服务器时，该向导将尝试安装在计算机上的.NET Framework 版本 4.5.50709。 但是，如果早期版本的.NET Framework 4.5，则无法安装更新的发布，并尝试连接将失败并这个错误消息：运行该应用程序，必须安装一个以下版本的.NET Framework: V4.5.50709。 有关如何获取适当版本的.NET Framework 的说明，请联系你分配发布者。  
  
 **解决方案**  
  
 从计算机中，卸载.NET Framework 4.5，然后将计算机连接到服务器。  
  
#### <a name="to-uninstall-net-framework-45"></a>若要卸载.NET Framework 4.5  
  
1.  在**开始**打开的客户端计算机页面**“控制面板”**。  
  
2.  在控制面板，在下**程序**，单击**卸载程序**。  
  
3.  右键单击**Microsoft .NET Framework 4.5**，然后单击**卸载**。  
  
4.  .NET Framework 4.5 卸载成功后，将计算机连接到服务器。 安装正确版本的.NET Framework 4.5 以及接头软件。  
  
##  <a name="BKMK_Time"></a>5 的问题  
 **问题**  
  
 获取服务器不可用。 若要解决此问题，请联系负责你的网络的人员。 一台计算机连接到服务器时出错  
  
 **描述**  
  
 如果未同步的日期和时间服务器上的日期和时间连接的计算机上发生这种情况。  Windows Server 软件包和 Windows Server Essentials 使用时间同步服务要同步的日期和时间的计算机运行在 Windows Server Essentials 或 Windows Server Essentials 网络。 同步的时间非常重要，因为默认身份验证协议使用服务器时间身份验证过程的一部分。 例如，如果客户端计算机上的时钟不会同步到正确的日期和时间，Windows Server Essentials 或 Windows Server Essentials 身份验证错误地可能解释入侵尝试的登录请求和拒绝用户访问。  
  
 如果服务器 s 可用内存小于 5%发生这种情况。  
  
 如果你已经拥有的与 Windows 软件包服务器的 VPN 连接，并且你尝试使用域名地址配置接头软件关闭后端发生这种情况。  
  
 **解决方案**  
  
1.  与服务器同步的日期和时间客户端计算机上。 然后将计算机连接到服务器。  
  
2.  关闭服务器端，某些应用，然后将计算机连接到服务器。  
  
3.  关闭此 VPN 连接，然后再将计算机连接到服务器。  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>若要更改的日期和时间客户端计算机上  
  
1.  在客户端计算机的开始页面上，打开**“控制面板”**。  
  
2.  在控制面板中，单击**时钟、语言和区域**，然后单击**日期和时间**。  
  
3.  单击**更改日期和时间**到正确的日期和时间设置日期和时间，然后单击**确定**。  
  
4.  单击**确定**，然后关闭控制面板。  
  
5.  再次尝试连接到服务器的客户端计算机。 有关说明，请参阅连接到服务器的计算机。  
  
 如果你仍然无法连接到服务器的客户端计算机，请确保的日期和时间服务器上的正确无误。 如果的日期和时间不正确，它们进行更改。  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>若要更改的日期和时间服务器上  
  
1.  登录到使用你在设置期间 Windows Server Essentials 或 Windows Server Essentials 密码服务器安装和配置。  
  
    > [!NOTE]
    >  如果该服务器管理远程，您必须登录到服务器使用远程桌面连接。  
  
2.  在**开始**页上，打开**“控制面板”**。  
  
3.  在控制面板中，单击**时钟、语言和区域**，然后单击**日期和时间**。  
  
4.  单击**更改日期和时间**到正确的日期和时间设置日期和时间，然后单击**确定**。  
  
5.  单击**确定**，然后关闭控制面板。  
  
6.  在客户端计算机上，再次尝试连接到服务器的客户端计算机。 有关说明，请参阅连接到服务器的计算机。  
  
##  <a name="BKMK_ServiceStopped"></a>6 的问题  
 **问题**  
  
 获取出现错误。 若要解决此问题，请联系负责你的网络的人员。 一台计算机连接到服务器时出错  
  
 **描述**  
  
 如果你收到这个错误消息，可能无法运行 WSS 证书 Web 服务。  
  
 **解决方案**  
  
 开始 WSS 证书 Web 服务。  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>若要开始 WSS 证书 Web 服务  
  
1.  在服务器上**开始**页上，打开**管理工具**。  
  
     在**管理工具**，右键单击**Internet 信息服务 (IIS) 管理器**，然后单击**打开**。  
  
2.  在导航窗格中，单击**WSS 证书 Web 服务**。  
  
3.  在**操作**窗格中，单击**开始**。  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>7 问题  
 **问题**  
  
 在尝试试图连接成功后，再次将计算机连接到服务器时，我收到的警告同名的计算机已连接到服务器  
  
 **描述**  
  
 如果之前尝试连接到服务器的计算机已取消，或者中断，你可能会收到以下警告，当你尝试重新连接：同名的计算机已连接到服务器。 这种情况下，由于证书颁发当你尝试连接到服务器的第一次。  
  
 **解决方案**如果你是确保没有其他计算机具有相同名称已连接到服务器，请单击**下一步**，然后按照说明来完成**我的计算机连接到服务器**向导。  
  
##  <a name="BKMK_JoinWin7"></a>8 问题  
 **问题**  
  
 当尝试连接到服务器运行 Windows 7 家庭版的客户端计算机时，打开运行接头软件网页，但无法连接到服务器的客户端计算机  
  
 **描述**  
  
 如果你的网络上的路由器已启用多路广播，服务器和客户端计算机运行的 Windows 7 家庭普通版或 Windows 7 家庭高级版之间进行通信无法正常工作。  
  
 **解决方案**  
  
 禁用多路由器上的路广播。 在某些路由器，这可能包括禁用翻录-2 M 路由协议。 有关详细信息，请参阅路由器制造商提供的文档。  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>9 的问题  
 **问题**  
  
 自动登录停止工作后在已将计算机连接到服务器  
  
 **描述**  
  
 如果计算机连接到 Windows Server Essentials 时自动登录设置的用户帐户，接头软件安装在计算机上时覆盖设置。  
  
 **解决方案**若要解决此问题，，当你将计算机连接到服务器，请记下用于用户帐户的密码。 安装接头软件后，将配置为自动登录使用该帐户。  
  
> [!NOTE]
>  Windows Server Essentials 域帐户需要满足默认密码策略要求的密码。  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>10 的问题  
 **问题**  
  
 卸载接头软件预发布版本不会删除现有日志  
  
 **描述**  
  
 预发行（beta 版或 RC）版本的 Windows Server Essentials 中更新到的已发布版本后，你必须删除已连接到服务器，每个计算机上的接头软件，然后将重新安装接头软件的已发布的版本的计算机连接。  
  
 但是，当你从网络计算机中删除接头软件的现有日志文件中 %ProgramData%\ Microsoft \ 不会删除该计算机上的 Windows Server \Logs\ 文件夹。 如果你不会删除日志文件夹，将计算机连接到的已发布版本的 Windows Server Essentials 中可能会损坏的日志文件。  
  
 **解决方案**以免的日志文件损坏，手动删除日志文件夹之前重新连接到更新服务器客户端计算机。  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>若要重新连接的计算机后，而不会损坏日志更新服务器文件  
  
1.  卸载来自客户端计算机接头软件。  
  
2.  删除日志文件夹从 %ProgramData%\ Microsoft \ Windows Server \ 文件夹。  
  
3.  重新连接到服务器的计算机。 这会安装接头软件，创建一个新的日志文件夹和日志文件中的已发布的版本。  
  
##  <a name="BKMK_UpgradeClientOS"></a>11 的问题  
 **问题**  
  
 我想要升级的客户端计算机上的操作系统  
  
 **描述**  
  
 在安装期间接头软件，将针对客户端操作系统为确保客户端符合所有接头先决条件执行许多检查。 如果客户端操作系统升级后安装接头软件时，某些先决条件可能不会出现，并客户端连接器可能失败。  
  
 **解决方案**  
  
 你升级到了不同版本的客户端操作系统之前（例如，你升级 Windows XP 到 Windows Vista 或 Windows 7 到 Windows Vista），您应该卸载接头软件。 使用**添加或删除程序**控制面板中。 客户端操作系统升级完成后，你可以重新安装客户端连接器打开 http://<*服务器*> / 在 Web 浏览器连接了 <*服务器*> 是 Windows Server Essentials 服务器的名称。  
  
 如果你已经安装了接头软件与升级客户端，使用**添加/删除程序**或**程序和功能**卸载接头软件。 然后重新安装接头软件。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials ConnectComputer 疑难解答 (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)

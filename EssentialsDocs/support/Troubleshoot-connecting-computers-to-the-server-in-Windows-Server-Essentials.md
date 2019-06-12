---
title: 将计算机连接到 Windows Server Essentials 中的服务器疑难解答
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: 52ec9bf1caa4cb4c7ed661eec1448f3f2a4be980
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436057"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>将计算机连接到 Windows Server Essentials 中的服务器疑难解答

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
 本主题包含有关将计算机连接到运行 Windows Server Essentials 的服务器或 Windows Server Essentials 时可能遇到的问题故障排除指南。  
  
> [!NOTE]
>  对于 Windows Server Essentials 和 Windows Server Essentials 社区的最新疑难解答信息，我们建议您访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials)。 Windows Server Essentials 论坛是寻求帮助或提出问题的好地方。  
  
 本主题提供以下问题的解决方案：  
  

-   问题 1:[问题 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   问题 2:[问题 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   问题 3:[问题 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   问题 4:[问题 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   问题 5:[问题 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   问题 6:[问题 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   问题 7:[问题 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   问题 8:[问题 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   问题 9:[问题 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   问题 10:[问题 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   问题 11:[问题 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   问题 1:[问题 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   问题 2:[问题 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   问题 3:[问题 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   问题 4:[问题 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   问题 5:[问题 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   问题 6:[问题 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   问题 7:[问题 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   问题 8:[问题 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   问题 9:[问题 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   问题 10:[问题 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   问题 11:[问题 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a> 问题 1  
 **问题**  
  
 获取的包安装未成功。 请重新尝试安装 Windows Server Essentials 连接器。 如果问题仍然存在，请联系网络管理员错误将计算机连接到服务器时  
  
 **说明**  
  
 如果将计算机连接到其他 Windows 更新或应用程序安装处于挂起状态且连接器安装已取消时正在运行的 Windows Server Essentials 的服务器，则可能会出现此问题。  
  
 **解决方案**  
  
 完成其他所有更新或应用程序的安装。 如果出现提示，则重新启动计算机。  
  
##  <a name="BKMK_ConnectorIssue2"></a> 问题 2  
 **问题**  
  
 不能将计算机加入到 Windows Server Essentials  
  
 **说明**  
  
 计算机名称中包含非 ASCII 字符的计算机无法加入 Windows Server Essentials。 如果计算机名包含非 ASCII 字符，你将收到错误消息“出现意外错误”。  
  
 **解决方案**  
  
 重命名计算机客户端仅，包含 ASCII 字符的名称，然后重试将计算机添加到 Windows Server Essentials。  
  
##  <a name="BKMK_ConnectorIssue2a"></a> 问题 3  
 **问题**  
  
 连接器软件安装已取消时收到错误将计算机连接到服务器  
  
 **说明**  
  
 为了能够将计算机连接到服务器，系统帐户必须具有 Windows Server Essentials 仪表板上显示的服务器文件夹的完全控制权限。 如果未授予所需权限，你将收到“连接器软件安装已取消”错误消息。  
  
 **解决方案**  
  
 向系统帐户授予对每个服务器文件夹的 **完全控制** 权限。  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>向系统帐户授予对服务器文件夹的完全控制权限  
  
1.  打开 Windows Server Essentials 仪表板。  
  
2.  单击“存储”  ，然后单击“服务器文件夹”  。  
  
3.  右键单击服务器文件夹，然后单击“打开该文件夹”  。 （如果你没有看到“打开该文件夹”选项  ，则无需设置文件夹权限。）  
  
4.  在 Windows 资源管理器顶部的网络路径中，单击服务器共享以显示服务器上的共享文件夹。 例如，如果路径为**网络 > Server01 > 文件历史记录备份**，则单击 **Server01**。  
  
5.  右键单击某个服务器文件夹，然后单击 **“属性”** 。  
  
6.  单击 **“安全”** 选项卡。  
  
7.  如果系统帐户不允许进行**完全控制**，则单击“编辑”  ，然后单击“系统”  。 在“系统权限”下  ，选中“完全控制”  旁边的“允许”复选框  。  
  
8.  单击“确定”  两次，更新这些权限并关闭“属性”  。  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a> 问题 4  
 **问题**  
  
 获取运行此应用程序收件人，则必须安装.NET Framework 以下版本之一：V4.5.50709”错误  
  
 **说明**  
  
 当将计算机连接到运行 Windows Server Essentials 的服务器或 Windows Server Essentials 后时，向导会尝试在计算机上安装.NET Framework 版本 4.5.50709。 但是，如果存在早期版本的.NET Framework 版本 4.5，则不能安装更新的版本，并且连接尝试失败，出现此错误消息：若要运行此应用程序，必须安装.NET Framework 以下版本之一：V4.5.50709。 有关获取.NET Framework 的适当版本的说明，请与你的分配发行商联系。  
  
 **解决方案**  
  
 从计算机上卸载 .NET Framework 4.5，然后将计算机连接到服务器。  
  
#### <a name="to-uninstall-net-framework-45"></a>卸载 .NET Framework 4.5  
  
1.  在客户端计算机的“开始”  页面上，打开“控制面板”  。  
  
2.  在“控制面板”的“程序”  下，单击“卸载程序”  。  
  
3.  右键单击“Microsoft .NET Framework 4.5”  ，然后单击“卸载”  。  
  
4.  成功卸载 .NET Framework 4.5 后，将计算机连接到服务器。 .NET Framework 4.5 的正确版本是随连接器软件一起安装的。  
  
##  <a name="BKMK_Time"></a> 问题 5  
 **问题**  
  
 获取服务器不可用。 若要解决此问题，请联系负责你的网络的人员。 错误  
  
 **说明**  
  
 如果连接的计算机上的日期和时间没有与服务器上的日期和时间同步，则可能发生这种情况。  Windows Server Essentials 和 Windows Server Essentials 使用时间同步服务同步的日期和时间的运行 Windows Server Essentials 或 Windows Server Essentials 网络中的计算机。 因为默认的身份验证协议会在身份验证过程中使用服务器时间，所以同步时间非常重要。 例如，如果客户端计算机上的时钟不同步到正确的日期和时间，Windows Server Essentials 或 Windows Server Essentials 身份验证可能会错误地解释为入侵尝试登录请求并拒绝用户访问权限。  
  
 如果服务器的可用内存小于 5%，则可以发生这种情况。  
  
 如果你已通过 VPN 连接到 Windows Server Essentials，而你尝试通过使用域地址从外部配置连接器软件，则可能发生这种情况。  
  
 **解决方案**  
  
1.  使客户端计算机上的日期和时间与服务器上的日期和时间同步。 然后将计算机连接到服务器。  
  
2.  关闭服务器端上的某些应用程序，然后将计算机连接到服务器。  
  
3.  关闭 VPN 连接，然后将计算机连接到服务器。  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>更改客户端计算机上的日期和时间  
  
1. 在客户端计算机的“开始”页面上，打开“控制面板”  。  
  
2. 在“控制面板”中，单击  “时钟、语言和区域”，然后单击  “日期和时间”。  
  
3. 单击“更改日期和时间”  、将日期和时间设置为正确的日期和时间，然后单击“确定”  。  
  
4. 单击“确定”  ，然后关闭“控制面板”。  
  
5. 重新尝试将客户端计算机连接到服务器。 有关说明，请参阅“将计算机连接到服务器”。  
  
   如果仍无法将客户端计算机连接到服务器，请确保服务器上的日期和时间是正确的。 如果日期和时间不正确，请更改。  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>更改服务器上的日期和时间  
  
1.  登录到使用在 Windows Server Essentials 或 Windows Server Essentials 安装和配置过程中设置的密码的服务器上。  
  
    > [!NOTE]
    >  如果你从远程管理服务器，则必须使用远程桌面连接登录服务器。  
  
2.  在“开始”  页上，打开“控制面板”  。  
  
3.  在“控制面板”中，单击  “时钟、语言和区域”，然后单击  “日期和时间”。  
  
4.  单击“更改日期和时间”  、将日期和时间设置为正确的日期和时间，然后单击“确定”  。  
  
5.  单击“确定”  ，然后关闭“控制面板”。  
  
6.  在客户端计算机上，再次尝试将客户端计算机连接到服务器。 有关说明，请参阅“将计算机连接到服务器”。  
  
##  <a name="BKMK_ServiceStopped"></a> 问题 6  
 **问题**  
  
 获取 An 发生意外的错误。 若要解决此问题，请联系负责你的网络的人员。 错误  
  
 **说明**  
  
 如果收到此错误消息，WSS 证书 Web 服务可能没有在运行。  
  
 **解决方案**  
  
 启动 WSS 证书 Web 服务。  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>启动 WSS 证书 Web 服务  
  
1.  在服务器“开始”  页面上，单击“管理工具”  。  
  
     在“管理工具”  中，右键单击“Internet 信息服务 (IIS) 管理器”  ，然后单击“打开”  。  
  
2.  在导航窗格中，单击“WSS 证书 Web 服务”  。  
  
3.  在“操作”  窗格中，单击“启动”  。  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a> 问题 7  
 **问题**  
  
 当我尝试连接尝试失败后再次连接到服务器的计算机时，我收到的警告： 具有此名称的计算机已连接到服务器  
  
 **说明**  
  
 如果之前将计算机连接到服务器的尝试已取消或中断，在再次尝试连接时，你可能会收到以下警告：具有此名称的计算机已连接到服务器。 因为当你首次尝试连接到服务器时发布了一张证书，所以会发生这种情况。  
  
 **解决方案**如果你确定没有任何其他同名计算机已连接到服务器，请单击“下一步”  ，然后按照说明完成“将我的计算机连接到服务器”  向导。  
  
##  <a name="BKMK_JoinWin7"></a> 问题 8  
 **问题**  
  
 当我试图将运行 Windows 7 家庭版的客户端计算机连接到服务器时，运行连接器软件的网页打开了，但客户端计算机无法连接到服务器  
  
 **说明**  
  
 如果网络上的路由器启用了多播，则服务器和运行 Windows 7 家庭普通版或 Windows 7 家庭高级版的客户端计算机之间将无法正常通信。  
  
 **解决方案**  
  
 禁用路由器上的多播。 在某些路由器上，这可能包括禁用 RIP-2M 路由协议。 有关详细信息，请参考路由器制造商提供的文档。  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a> 问题 9  
 **问题**  
  
 将计算机连接到服务器后自动登录停止运行  
  
 **说明**  
  
 如果自动登录设置的用户帐户将计算机连接到 Windows Server Essentials 时，设置将覆盖在计算机上安装连接器软件。  
  
 **解决方案** 若要解决此问题，在将计算机连接到服务器时，请记下用于用户帐户的密码。 安装连接器软件后，将自动登录配置为使用该帐户。  
  
> [!NOTE]
>  Windows Server Essentials 域帐户要求满足默认密码策略要求的密码。  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a> 问题 10  
 **问题**  
  
 卸载预发布版本的连接器软件不会删除现有日志  
  
 **说明**  
  
 从 Windows Server Essentials 的预发布版本 （Beta 或 RC） 版本更新到发行版本后，必须从已连接到服务器，每台计算机中删除连接器软件，然后程序以安装发布将计算机连接连接器软件版本。  
  
 然而，当你从网络计算机上删除连接器软件时，不会删除该计算机上 %ProgramData%\Microsoft\Windows Server\Logs\ 文件夹中的现有日志文件。 如果不删除日志文件夹，将计算机连接到发行版本的 Windows Server Essentials 中可能会损坏的日志文件。  
  
 **解决方案**若要避免日志文件损坏，请在将客户端计算机重新连接到更新的服务器之前，手动删除日志文件夹。  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>在更新服务器后重新连接计算机而不损坏日志文件  
  
1.  从客户端计算机中卸载连接器软件。  
  
2.  删除 %ProgramData%\Microsoft\Windows Server\ 下的日志文件夹。  
  
3.  重新将计算机连接到服务器。 这将安装已发布的连接器软件版本，创建新的日志文件夹和日志文件。  
  
##  <a name="BKMK_UpgradeClientOS"></a> 问题 11  
 **问题**  
  
 我希望在客户端计算机上升级操作系统  
  
 **说明**  
  
 在连接器软件安装期间，将针对客户端操作系统执行数次检查，以确保该客户端满足所有连接器先决条件。 如果在安装连接器软件后升级客户端操作系统，某些先决条件可能不存在，而客户端连接器可能无法运行。  
  
 **解决方案**  
  
 在将客户端操作系统升级到另一版本（例如，将 Windows XP 升级到 Windows Vista 或将 Windows Vista 升级到 Windows 7）之前，应先卸载连接器软件。 使用“控制面板”中的“添加或删除程序”  。 客户端操作系统升级完成后，您可以通过打开 http://&lt 来重新安装客户端连接器*服务器*> / 连接在 Web 浏览器，其中 <*server*> 是的名称 Windows Server Essentials 服务器。  
  
 如果你在已安装连接器软件的情况下升级了客户端，请使用“添加/删除程序”  或“程序和功能”卸载该连接器软件  。 然后再次安装连接器软件。  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials 连接计算机疑难解答 (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)

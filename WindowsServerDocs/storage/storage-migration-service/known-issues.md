---
title: 存储迁移服务的已知问题
description: 有关存储迁移服务的已知问题和疑难解答支持，如如何为 Microsoft 支持部门收集日志。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 10/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 597bcbe647bca3595dc8251ce4d6bf52265d8731
ms.sourcegitcommit: 4b4ff8d9e18b2ddcd1916ffa2cd58fffbed8e7ef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/28/2019
ms.locfileid: "72986436"
---
# <a name="storage-migration-service-known-issues"></a>存储迁移服务的已知问题

本主题包含有关使用[存储迁移服务](overview.md)迁移服务器时的已知问题的解答。

存储迁移服务分为两部分： Windows Server 中的服务和 Windows 管理中心中的用户界面。 服务在 Windows Server、长期服务通道以及 Windows Server、半年通道中可用;虽然可单独下载 Windows 管理中心。 我们还会定期包括 Windows Server 累积更新中的更改（通过 Windows 更新发布）。 

例如，Windows Server 版本1903包括存储迁移服务的新功能和修复程序，它们也可用于 Windows Server 2019 和 Windows Server，版本1809通过安装[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)来实现。

## <a name="collecting-logs"></a>如何在使用 Microsoft 支持部门时收集日志文件

存储迁移服务包含 Orchestrator 服务和代理服务的事件日志。 Urchestrator 服务器始终包含事件日志，并且安装了代理服务的目标服务器包含代理日志。 这些日志位于：

- 应用程序和服务日志 \ Microsoft \ Windows \ StorageMigrationService
- 应用程序和服务日志 \ Microsoft \ Windows \ StorageMigrationService-Proxy

如果需要收集这些日志以供脱机查看或发送到 Microsoft 支持部门，则 GitHub 上提供了一个开源 PowerShell 脚本：

 [存储迁移服务帮助器](https://aka.ms/smslogs) 

查看自述文件的用法。

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>除非管理 Windows Server 2019，否则不会在 Windows 管理中心中显示存储迁移服务

当使用1809版本的 Windows 管理中心来管理 Windows Server 2019 orchestrator 时，将看不到存储迁移服务的工具选项。 

Windows 管理中心存储迁移服务扩展受版本限制，只管理 Windows Server 2019 1809 或更高版本的操作系统。 如果你使用它来管理较早的 Windows Server 操作系统或有问必答预览，则该工具将不会显示。 此行为是设计使然。 

若要解决此问题，请使用或升级到 Windows Server 2019 build 1809 或更高版本。

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>存储迁移服务不允许在切换时选择静态 IP

在 Windows 管理中心使用0.57 版本的存储迁移服务扩展时，如果出现切换阶段，则无法为地址选择静态 IP。 系统会强制使用 DHCP。

若要解决此问题，请在 Windows 管理中心的 " > **设置**" 下查看一个警报，指出已更新版本存储迁移服务**0.57.2 可供**安装。 可能需要重新启动 Windows 管理中心的浏览器选项卡。

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>存储迁移服务转换验证失败，出现错误 "对目标计算机上的令牌筛选器策略的访问被拒绝"

运行切换验证时，收到错误 "失败：目标计算机上的令牌筛选器策略访问被拒绝"。 即使您为源计算机和目标计算机提供了正确的本地管理员凭据，也会出现这种情况。

此问题是由 Windows Server 2019 中的代码缺陷导致的。 当你使用目标计算机作为存储迁移服务 Orchestrator 时，将出现此问题。

若要解决此问题，请在不是预期迁移目标的 Windows Server 2019 计算机上安装存储迁移服务，然后使用 Windows 管理中心连接到该服务器并执行迁移。

我们已在更高版本的 Windows Server 中解决了此情况。 请通过[Microsoft 支持部门](https://support.microsoft.com)打开支持案例，以请求创建此修补程序的向后移植。

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>Windows Server 2019 评估版或 Windows Server 2019 Essentials edition 中不包含存储迁移服务

使用 Windows 管理中心连接到[Windows server 2019 评估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或 windows Server 2019 Essentials edition 时，没有管理存储迁移服务的选项。 角色和功能中也不包含存储迁移服务。

此问题是由 Windows Server 2019 和 Windows Server 2019 Essentials 的评估介质中的服务问题引起的。 

若要解决此问题，请安装 Windows Server 2019 的零售、MSDN、OEM 或批量许可版本，但不要将其激活。 如果没有激活，Windows Server 的所有版本都将在评估模式下运行180天。 

我们已在更高版本的 Windows Server 中解决了此问题。  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>存储迁移服务下载传输错误 CSV 超时

使用 Windows 管理中心或 PowerShell 下载传输操作仅限错误的 CSV 日志时，收到错误消息：

 >   传输日志-请检查防火墙中是否允许进行文件共享。 ：发送到 net.tcp：//localhost： 28940/sms/service/1/transfer 的此请求操作在配置的超时（00:01:00）内未收到答复。 分配给此操作的时间可能是更长超时的一部分。 这可能是因为服务仍在处理此操作，或者是因为服务无法发送答复消息。 请考虑增加操作超时（通过将通道/代理强制转换为 IContextChannel 并设置 OperationTimeout 属性），并确保服务能够连接到客户端。

此问题是由存储迁移服务允许的默认一分钟超时内无法筛选的传输文件数过多造成的。 

若要解决此问题：

1. 在 orchestrator 计算机上，使用 Notepad.exe 编辑 *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config*文件，将 "sendTimeout" 的默认值从1分钟更改为10分钟

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. 重新启动 orchestrator 计算机上的 "存储迁移服务" 服务。 
3. 在 orchestrator 计算机上，启动 Regedit.exe
4. 找到并单击以下注册表子项： 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. 在“编辑”菜单上，指向“新建”，然后单击“DWORD 值”。 
6. 键入 "WcfOperationTimeoutInMinutes" 作为 DWORD 名称，然后按 ENTER。
7. 右键单击 "WcfOperationTimeoutInMinutes"，然后单击 "修改"。 
8. 在 "基本数据" 框中，单击 "Decimal"
9. 在 "数值数据" 框中，键入 "10"，然后单击 "确定"。
10. 退出注册表编辑器。
11. 尝试再次下载错误的 CSV 文件。 

我们打算在更高版本的 Windows Server 2019 中更改此行为。  

## <a name="cutover-fails-when-migrating-between-networks"></a>在网络之间迁移时转换失败

如果迁移到的目标计算机在不同于源的网络（如 Azure IaaS 实例）上运行，则当源使用静态 IP 地址时，转换将无法完成。 

此行为是设计使然，防止从通过 IP 地址进行连接的用户、应用程序和脚本迁移后出现连接问题。 当 IP 地址从旧的源计算机移到新的目标目标时，它不会与新的网络子网信息（或许是 DNS 和 WINS）匹配。

若要解决此问题，请执行到同一网络中的计算机的迁移。 然后，将该计算机移到新网络并重新分配其 IP 信息。 例如，如果迁移到 Azure IaaS，请先迁移到本地 VM，然后使用 Azure Migrate 将 VM 转移到 Azure。  

我们已在 Windows 管理中心的更高版本中解决了此问题。 我们现在允许你指定不改变目标服务器的网络设置的迁移。 发布时，将在此处列出更新的扩展。 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>目标代理和凭据管理权限的验证警告

验证传输作业时，将看到以下警告：

 > **凭据具有管理权限。**
 > 警告：操作无法远程使用。
 > **目标代理已注册。**
 > 警告：找不到目标代理。

如果你尚未在 Windows Server 2019 目标计算机上安装存储迁移服务代理服务，或者目标计算机是 Windows Server 2016 或 Windows Server 2012 R2，则此行为是设计使然。 建议迁移到安装了代理的 Windows Server 2019 计算机以大幅提高传输性能。  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>某些文件未列出清单或传输，错误 5 "拒绝访问"

当在源计算机和目标计算机上清点或传输文件时，用户已删除管理员组权限的文件将无法迁移。 检查存储迁移服务-代理调试显示：

  日志名称： StorageMigrationService/调试源： Microsoft-StorageMigrationService-Proxy Date： 2/26/2019 9:00:04 AM 事件 ID：10000任务类别：无级别：错误关键字：      
  用户：网络服务计算机： srv1.contoso.com 说明：

  02/26/2019-09：00： 04.860 [Error] 传输 \\错误。 com\public\indy.png：（5）访问被拒绝。
Stack 跟踪：在 StorageMigration （String fileName、DesiredAccess desiredAccess、ShareMode shareMode、CreationDisposition creationDisposition、FlagsAndAttributes flagsAndAttributes）处，StorageMigration （FileDirUtils）上的 GetTargetFile （字符串路径），网址为，网址为，（FileDirUtils 文件），网址为StorageMigration （），网址为 FileTransfer. InitializeSourceFileInfo （），网址为，，网址为，，网址为StorageMigration FileTransfer. TryTransfer （） [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs：： TryTransfer：： 55]


此问题的原因是存储迁移服务中的代码缺陷未调用备份权限。 

若要解决此问题，请在[KB4490481 （OS Build 17763.404）上 Windows 更新](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481)安装在 orchestrator 计算机和目标计算机上（如果安装了代理服务）。 请确保源迁移用户帐户是源计算机上的本地管理员和存储迁移服务协调器。 请确保目标迁移用户帐户是目标计算机上的本地管理员和存储迁移服务协调器。 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用存储迁移服务预先播种数据时，DFSR 哈希不匹配

使用存储迁移服务将文件传输到新目标，然后配置 DFS 复制（DFSR）通过 preseeded 复制或 DFSR 数据库克隆将数据复制到现有的 DFSR 服务器，所有文件 experiemce 哈希不匹配，将重新复制。 使用 SMS 传输数据流、安全流、大小和属性后，它们看起来完全匹配。 检查具有 ICACLS 的文件或 DFSR 数据库克隆调试日志会显示以下内容：

源文件：

  icacls d:\test\Source：

  icacls d:\test\thatcher.png/save out .txt/t thatcher D:AI （A;;FA;;;BA）（A;; 0x1200a9;;;DD）（A;; 0）x1301bf;;;DU）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU

目标文件：

  icacls d:\test\thatcher.png/save out .txt/t thatcher D:AI （A;;FA;;;BA）（A;; 0x1301bf;;;DU）（A;; 0x1200a9;;;DD）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU）**S:PAINO_ACCESS_CONTROL**

DFSR 调试日志：

  20190308 10：18： 53.116 3948 DBCL 4045 [警告] DBClone：： IDTableImportUpdate 找不到记录。 

  Local ACL hash： 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime： 20190308 18：09： 44.876 FileSizeLow： 1131654 FileSizeHigh：0属性：32 

  Clone ACL hash：**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime： 20190308 18：09： 44.876 FileSizeLow： 1131654 FileSizeHigh：0属性：32 

此问题是由存储迁移服务使用的库中的代码缺陷来设置安全审核 Acl （SACL）引起的。 当 SACL 为空时，将无意中设置非 null SACL，这是用于正确标识哈希不匹配的领先 DFSR。 

若要解决此问题，请继续为[dfsr 预播种和 DFSR 数据库克隆操作](../dfs-replication/preseed-dfsr-with-robocopy.md)（而不是存储迁移服务）使用 Robocopy。 我们正在调查此问题，并想要在更高版本的 Windows Server 中解决此问题，可能还有一个向后移植 Windows 更新。 

## <a name="error-404-when-downloading-csv-logs"></a>下载 CSV 日志时出现错误404

尝试在传输操作结束时下载传输或错误日志时，会收到错误：

  $jobname：传输日志： ajax 错误404

如果未在 orchestrator 服务器上启用 "文件和打印机共享（SMB）" 防火墙规则，则会出现此错误。 Windows 管理中心文件下载需要连接的计算机上的端口 TCP/445 （SMB）。  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>从 Windows Server 2008 R2 传输时，出现错误 "无法传输任何终结点上的存储"

尝试从 Windows Server 2008 R2 源计算机传输数据时，不会传输任何数据，并且你会收到错误：  

  无法传输任何终结点上的存储。
0x9044

如果 Windows Server 2008 R2 计算机没有通过 Windows 更新的所有重要更新和重要更新进行完全修补，则会出现此错误。 不管使用哪种存储迁移服务，出于安全考虑，我们始终建议修补 Windows Server 2008 R2 计算机，因为该操作系统不包含更高版本的 Windows Server 的安全性改进。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>错误： "无法在任何端点上传输存储" 和 "检查源设备是否处于联机状态-我们无法访问它"。

尝试从源计算机传输数据时，某些或全部共享不会传输，并出现摘要错误：

   无法传输任何终结点上的存储。
0x9044

检查 SMB 传输详细信息时显示错误：

   检查源设备是否处于联机状态-我们无法进行访问。

检查 StorageMigrationService/Admin 事件日志显示：

   无法传输存储。

   作业： Job1 ID：  
   状态：失败错误：36931错误消息： 

   指南：检查详细错误，并确保满足传输要求。 传输作业无法传输任何源和目标计算机。 这可能是因为 orchestrator 计算机无法访问任何源计算机或目标计算机，可能是因为防火墙规则或缺少权限。

检查 StorageMigrationService/Debug 日志显示：

   07/02/2019-13：35： 57.231 [错误] 传输验证失败。 错误代码：40961，源终结点无法访问或不存在，或源凭据无效，或经过身份验证的用户没有足够的权限访问它。
在 StorageMigration （）中的 TransferOperation （），在 StorageMigration （ProcessRequest fileTransferRequest，Guid operationId）上进行验证（& e）   [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

如果迁移帐户没有对 SMB 共享的 "读取" 访问权限，则会出现此错误。 若要解决此错误，请将包含源迁移帐户的安全组添加到源计算机上的 SMB 共享，并向其授予 "读取"、"更改" 或 "完全控制" 权限。 迁移完成后，可以删除此组。

## <a name="error-0x80005000-when-running-inventory"></a>运行清单时出现错误0x80005000

安装[KB4512534](https://support.microsoft.com/en-us/help/4512534/windows-10-update-kb4512534)并尝试运行清单后，清单失败并出现错误：

  HRESULT 中的异常：0x80005000
  
  日志名称： Microsoft StorageMigrationService/Admin 源： Microsoft-StorageMigrationService Date： 9/9/2019 5:21:42 PM 事件 ID：2503任务类别：无级别：错误关键字：      
  用户：网络服务计算机： FS02。TailwindTraders.net 说明：无法清点计算机。
作业： foo2 ID：20ac3f75-4945-41d1-9a79-d11dbb57798b 状态： Failed 错误：36934错误消息：所有设备的清单失败指南：检查详细错误并确保满足清单要求。 作业无法清点任何指定的源计算机。 这可能是因为 orchestrator 计算机无法通过网络访问它，这可能是由于防火墙规则或缺少权限造成的。
  
  日志名称： Microsoft StorageMigrationService/Admin 源： Microsoft-StorageMigrationService Date： 9/9/2019 5:21:42 PM 事件 ID：2509任务类别：无级别：错误关键字：      
  用户：网络服务计算机： FS02。TailwindTraders.net 说明：无法清点计算机。
作业： foo2 计算机： FS01。TailwindTraders.net 状态： Failed 错误：-2147463168 错误消息：指南：检查详细的错误并确保满足清单要求。 清单无法确定指定源计算机的任何方面。 这可能是因为缺少对源或阻止的防火墙端口的权限或特权。
  
当你以用户主体名称（UPN）的形式提供迁移凭据（如 "meghan@contoso.com"）时，存储迁移服务中的代码缺陷会导致此错误。 存储迁移服务协调器服务无法正确分析此格式，这导致在域查找中为 KB4512534 和19H1 中的群集迁移支持添加了故障。

若要解决此问题，请以 "域 \ 用户" 格式提供凭据，如 "Contoso\Meghan"。

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>错误 "ServiceError0x9006" 或 "代理当前不可用"。 迁移到 Windows Server 故障转移群集时

尝试针对群集文件服务器传输数据时，会收到如下错误： 

   请确保代理服务已安装并且正在运行，然后重试。 代理当前不可用。
0x9006 ServiceError0x9006，StorageMigration UnregisterSmsProxyCommand

如果文件服务器资源从其原始 Windows Server 2019 群集所有者节点移到新节点，并且该节点上未安装存储迁移服务代理功能，则会出现此错误。

作为一种解决方法，请将目标文件服务器资源移回首次配置传输配对时使用的原始所有者群集节点。

另一种解决方法：

1. 在群集中的所有节点上安装存储迁移服务代理功能。
2. 在 orchestrator 计算机上运行以下存储迁移服务 PowerShell 命令： 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>从群集节点运行清单时出现错误 "找不到 Dll"

如果尝试使用安装在 Windows Server 2019 故障转移群集节点上的存储迁移服务 orchestrator 运行清单，并以 Windows Server 故障转移群集为目标，则会收到以下错误：

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

若要解决此问题，请在运行存储迁移服务 orchestrator 的服务器上安装 "故障转移群集管理工具" （RSAT 群集管理工具）。 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>针对 Windows Server 2003 源计算机运行清单时出现错误 "终结点映射器中没有更多的终结点可用"

当尝试在存储迁移服务 orchestrator server 中通过[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)累积更新或更高版本进行修补时，会收到以下错误：

    There are no more endpoints available from the endpoint mapper  

若要解决此问题，请从存储迁移服务 orchestrator 计算机暂时卸载 KB4512534 累积更新（以及任何取代它的更新）。 迁移完成后，重新安装最新的累积更新。  

请注意，在某些情况下，卸载 KB4512534 或其取代的更新可能会导致存储迁移服务不再启动。 若要解决此问题，你可以备份和删除存储迁移服务数据库：

1.  打开提升的 cmd 提示符，你是存储迁移服务协调器服务器上的管理员成员，然后运行：

     ```
     TAKEOWN /d /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  启动存储迁移服务服务，该服务将创建一个新数据库。

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>错误 "CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO 无法应对网络名称资源"，Windows Server 2008 R2 群集切换失败

当尝试对 Windows Server 2008 R2 群集源运行 cut 时，该剪切卡会停滞在 "重命名源计算机 ..." 阶段你会收到以下错误：

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

此问题是由较早版本的 Windows Server 中缺少的 API 导致的。 目前没有办法迁移 Windows Server 2008 和 Windows Server 2003 群集。 在 Windows Server 2008 R2 群集上，你可以执行清单和传输，而不是在 Windows Server R2 群集上进行，然后手动执行转换，方法是手动更改群集的源文件服务器资源网络名称和 IP 地址，然后更改目标群集网络名称和 IP与原始源相匹配的地址。 

## <a name="see-also"></a>另请参阅

- [存储迁移服务概述](overview.md)

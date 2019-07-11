---
title: 存储迁移服务的已知问题
description: 已知的问题和故障排除支持存储迁移服务，例如，如何获得 Microsoft 支持收集日志。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 08156a09491d66016b5fcfe6056ed318d682b987
ms.sourcegitcommit: 514d659c3bcbdd60d1e66d3964ede87b85d79ca9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67735156"
---
# <a name="storage-migration-service-known-issues"></a>存储迁移服务的已知问题

本主题包含在使用时的已知问题的答案[存储迁移服务](overview.md)将服务器迁移。

## <a name="collecting-logs"></a> 如何与 Microsoft 支持人员时收集的日志文件

存储迁移服务包含业务流程协调程序服务和代理服务的事件日志。 Urchestrator 服务器始终包含这两个事件日志，并且安装了代理服务的目标服务器包含代理日志。 这些日志位于：

- 应用程序和服务日志 \ Microsoft \ Windows \ StorageMigrationService
- 应用程序和服务日志 \ Microsoft \ Windows \ StorageMigrationService 代理

如果您需要收集这些日志，以便脱机查看或发送给 Microsoft 支持部门，还有一种开放源可在 GitHub 上的 PowerShell 脚本：

 [存储迁移服务帮助程序](https://aka.ms/smslogs) 

查看使用情况的自述文件。

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>存储迁移服务不会显示在 Windows Admin Center 除非管理 Windows Server 2019

当使用 Windows Admin Center 1809年版本来管理 Windows Server 2019 业务流程协调程序，看不到存储迁移服务工具选项。 

Windows Admin Center 存储迁移服务扩展是仅用来管理 Windows Server 2019 版本 1809年或更高版本操作系统的版本绑定。 如果使用它来管理较旧的 Windows Server 操作系统或深入了解预览版，该工具将不显示。 此行为是设计使然。 

若要解决，请使用或升级到 Windows Server 2019 生成 1809年或更高版本。

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>存储迁移服务不允许您选择静态 IP 上直接转换

当使用存储迁移服务中 Windows Admin Center 和您的扩展访问直接转换阶段的 0.57 版本，不能选择静态 IP 地址。 您会被迫使用 DHCP。

若要解决此问题，Windows Admin Center 中的查找下**设置** > **扩展**警报指出该更新的版本存储迁移服务 0.57.2 是可用于安装。 您可能需要重启 Windows 管理员中心在浏览器选项卡。

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>存储迁移服务直接转换验证失败，出现错误"访问被拒绝的目标计算机上的标记筛选器策略"

当运行直接转换验证时，收到错误"失败：访问被拒绝的目标计算机上的标记筛选器策略。" 即使您的源和目标计算机提供正确的本地管理员凭据，将发生这种情况。

在 Windows Server 2019 中的代码缺陷导致此问题。 将目标计算机用作存储迁移服务业务流程协调程序时，将发生此问题。

若要解决此问题，不是预期的迁移目标的 Windows Server 2019 计算机上安装存储迁移服务，然后连接到该服务器与 Windows Admin Center 和执行迁移。

我们已解决这更高版本的 Windows Server 中。 请打开通过一个支持案例[Microsoft 支持部门](https://support.microsoft.com)请求创建此修补程序的向后移植。

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>存储迁移服务不包括在 Windows Server 2019 评估版

当使用 Windows Admin Center 连接到[Windows Server 2019 评估版本](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)没有管理存储迁移服务的选项。 存储迁移服务也不包括在角色和功能。

通过 Windows Server 2019 评估媒体中的服务问题导致此问题。 

若要解决此问题，请安装零售、 MSDN、 OEM 或批量许可版本的 Windows Server 2019 并不进行激活。 不进行激活，所有版本的 Windows Server 在评估模式下都运行 180 天。 

我们已在 Windows Server 2019 的更高版本中解决此问题。  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>存储迁移服务将会超时下载传输错误 CSV

在使用 Windows Admin Center 或 PowerShell 下载传输操作详细错误仅 CSV 日志，你将收到错误：

 >   传输日志-请检查在防火墙中允许文件共享。 :此请求操作发送到 net.tcp: //localhost: 28940/sms/service/1/传输未收到已配置的超时时间内回复 (00: 01:00)。 分配给此操作的时间可能是更长超时的一部分。 这可能是由于服务仍在处理该操作，或因为该服务无法发送回复消息。 请考虑增加操作超时值 （通过强制转换将通道/代理分配给 IContextChannel 并设置 OperationTimeout 属性），确保服务能够连接到客户端。

此问题引起的传输中允许的存储迁移服务的默认值一分钟超时不能筛选的文件数量非常多。 

若要解决此问题：

1. 在业务流程协调程序计算机上，编辑 *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config*文件使用 Notepad.exe 来更改"sendTimeout"不再是 1 分钟默认值为 10 分钟

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. 重新启动业务流程协调程序计算机上的"存储迁移服务"服务。 
3. 在业务流程协调程序计算机上启动 Regedit.exe
4. 找到并单击以下注册表子项： 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. 在“编辑”菜单上，指向“新建”，然后单击“DWORD 值”。 
6. 名称的 DWORD 中，键入"WcfOperationTimeoutInMinutes"，然后按 ENTER。
7. 右键单击"WcfOperationTimeoutInMinutes"，然后单击修改。 
8. 在基本数据框中，单击"Decimal"
9. 值数据框中键入"10"，然后单击确定。
10. 退出注册表编辑器。
11. 尝试再次下载仅限错误的 CSV 文件。 

我们想要更改此行为在 Windows Server 2019 的更高版本。  

## <a name="cutover-fails-when-migrating-between-networks"></a>当网络之间迁移时，直接转换失败

迁移到目标计算机在与源，如 Azure IaaS 实例，不同的网络中运行时直接转换无法完成源使用静态 IP 地址时。 

此行为是默认设置，以防止用户、 应用程序和连接通过 IP 地址的脚本从迁移后的连接问题。 从旧的源计算机中的 IP 地址移到新的目标时，它不会与匹配的新网络子网信息和可能是 DNS 和 WINS。

解决此问题，在同一网络上执行到一台计算机的迁移。 然后将该计算机移到新的网络，并重新分配其 IP 信息。 例如，如果迁移到 Azure IaaS，第一次迁移到本地 VM，然后使用 Azure Migrate 迁移到 Azure VM。  

我们已在更高版本的 Windows Admin Center 版本中解决此问题。 现在，我们将允许您指定不会改变目标服务器的网络设置的迁移。 发布时，将此处列出更新后的扩展。 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>目标代理和凭据管理权限的验证警告

正在验证的传输作业，会看到以下警告：

 > **凭据具有管理权限。**
 > 警告：远程操作不可用。
 > **目标代理注册。**
 > 警告：找不到目标代理。

如果你尚未安装存储迁移服务代理服务的 Windows Server 2019 目标计算机上，或对于计算机是 Windows Server 2016 或 Windows Server 2012 R2，此行为由设计决定。 我们建议迁移到 Windows Server 2019 计算机使用代理安装显著改进了的传输性能。  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>某些文件未列出清单或传输、 错误 5"访问被拒绝"

当清点或将文件从源传输到目标计算机时，用户已从中删除管理员组权限的文件无法迁移。 检查存储迁移服务的代理调试显示：

  日志名称：    Microsoft-Windows-StorageMigrationService-代理/调试源：      Microsoft-Windows-StorageMigrationService-Proxy Date:        2/26/2019年上午 9:00:04 事件 ID:    10000 任务类别：无级别：       错误关键字：      
  用户：        网络服务计算机： srv1.contoso.com 说明：

  [错误] 02/26/2019-09:00:04.860 传输错误\\srv1.contoso.com\public\indy.png:（5） 访问被拒绝。
堆栈跟踪： 在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile （字符串文件名、 DesiredAccess desiredAccess、 ShareMode shareMode、 CreationDisposition creationDisposition、 FlagsAndAttributes flagsAndAttributes）在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile （FileInfo 文件） 在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile （字符串路径）在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


此问题的原因不调用备份特权其中存储迁移服务中的代码缺陷。 

若要解决此问题，请安装[Windows Update 2019 年 4 月 2 日-KB4490481 (OS 生成 17763.404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481)业务流程协调程序计算机和目标计算机，如果那里安装了代理服务上。 请确保源迁移用户帐户是源计算机和存储迁移服务业务流程协调程序上的本地管理员。 请确保目标迁移用户帐户是目标计算机和存储迁移服务业务流程协调程序上的本地管理员。 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用存储迁移服务预先播种数据时，DFSR 哈希不匹配

当使用存储迁移服务将文件传输到新的目标，然后配置 DFS 复制 (DFSR) 复制该数据与现有的 DFSR 服务器通过 presseded 复制或克隆，DFSR 数据库所有文件 experiemce 哈希不匹配，并且是重新复制。 数据流、 安全流、 大小和所有显示要使用 SMS 将其传输之后完全匹配的属性。 检查与 ICACLS 文件或 DFSR 数据库克隆调试日志会显示：

源文件：

  icacls d:\test\Source:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1200a9;;;DD)(A;;0x1301bf;;;DU)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)

目标文件：

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1301bf;;;DU)(A;;0x1200a9;;;DD)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)**S:PAINO_ACCESS_CONTROL**

DFSR 调试日志：

  找到 20190308 10:18:53.116 3948 DBCL 4045 [警告] DBClone::IDTableImportUpdate 不匹配记录。 

  本地 ACL 哈希： 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0属性： 32 

  克隆 ACL 哈希：**DDC4FCE4 DDF329C4 977CED6D F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0属性： 32 

存储迁移服务用于设置安全审核 Acl (SACL) 库中的代码缺陷导致此问题。 SACL 为空，前导 DFSR 以正确地标识哈希不匹配时无意中设置了非 null SACL。 

解决此问题，请继续使用为 Robocopy[预先播种 DFSR 和 DFSR 数据库克隆操作](../dfs-replication/preseed-dfsr-with-robocopy.md)而不是存储迁移服务。 我们正在调查此问题，并想要解决此问题在 Windows Server 和可能是向后移植 Windows 更新的更高版本。 

## <a name="error-404-when-downloading-csv-logs"></a>下载 CSV 时的 404 错误日志

在尝试下载末尾的传输操作传输或错误日志时，你将收到错误：

  $jobname:传输日志： ajax 错误 404

如果没有启用业务流程协调程序服务器上的"文件和打印机共享 (Smb-in)"防火墙规则，会出现此错误。 Windows Admin Center 文件下载要求端口 TCP/445 (SMB) 连接在计算机上。  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>错误"无法将传输存储上的任何终结点"时从 Windows Server 2008 R2 的传输

在尝试从 Windows Server 2008 R2 源计算机传输数据时，没有数据传输，你将收到错误：  

  无法传输在任一终结点上的存储。
0x9044

如果您的 Windows Server 2008 R2 计算机不完全修补，并从 Windows 更新的所有关键和重要更新的则会出现此错误。 而不考虑存储迁移服务，我们始终建议修补 Windows Server 2008 R2 计算机出于安全目的，因为该操作系统不会包含较新版本的 Windows Server 的安全改进。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>错误"无法将传输存储上的任何终结点"和"检查源设备处于联机状态-是否我们无法访问它。"

在尝试从此源计算机传输数据时，某些或所有共享，不会传输，摘要错误：

   无法传输在任一终结点上的存储。
0x9044

检查 SMB 传输详细信息显示了错误：

   检查源设备处于联机状态-是否我们无法访问它。

检查 StorageMigrationService/管理事件日志显示了：

   无法传输存储。

   作业：Job1 ID:  
   状态：失败，错误：36931 出现错误消息： 

   指南：检查详细的错误，并确保满足传输要求。 传输作业无法传输任何源和目标计算机。 这可能是因为业务流程协调程序计算机无法访问任何源或目标的计算机，可能是由于防火墙规则，或缺少权限。

检查 StorageMigrationService 代理/Debug 日志所示：

   [错误] 07/02/2019-13:35:57.231 传输验证失败。 错误代码：40961，源终结点不可访问，或不存在，或源的凭据无效，或经过身份验证的用户不具有足够权限来访问它。
在 Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate() 在 Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest （FileTransferRequest fileTransferRequest，Guid operationId）   [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

会出现此错误，如果你迁移的帐户不在至少具有对 SMB 共享的读取访问权限。 解决此错误，添加包含源迁移的帐户与 SMB 共享源计算机上的安全组，并授予它读取、 更改或完全控制。 在迁移完成后，可以删除此组。 Windows Server 的未来版本可能会更改此行为以不再需要源共享的显式权限。

## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)

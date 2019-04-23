---
title: 存储迁移服务的已知问题
description: 已知的问题和故障排除支持存储迁移服务，例如，如何获得 Microsoft 支持收集日志。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888868"
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

我们想要在 Windows Server 的更高版本中解决此问题。 请打开通过一个支持案例[Microsoft 支持部门](https://support.microsoft.com)请求创建此修补程序的向后移植。

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>存储迁移服务不包括在 Windows Server 2019 评估版

当使用 Windows Admin Center 连接到[Windows Server 2019 评估版本](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)没有管理存储迁移服务的选项。 存储迁移服务也不包括在角色和功能。

通过 Windows Server 2019 评估媒体中的服务问题导致此问题。 

若要解决此问题，请安装零售、 MSDN、 OEM 或批量许可版本的 Windows Server 2019 并不进行激活。 不进行激活，所有版本的 Windows Server 在评估模式下都运行 180 天。 

我们已在 Windows Server 2019 的更高版本中解决此问题。  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>存储迁移服务将会超时下载传输错误 CSV

在使用 Windows Admin Center 或 PowerShell 下载传输操作详细错误仅 CSV 日志，你将收到错误：

 >   传输日志-请检查在防火墙中允许文件共享。 ：此请求操作发送到 net.tcp: //localhost: 28940/sms/service/1/传输未收到已配置的超时时间内回复 (00: 01:00)。 分配给此操作的时间可能是更长超时的一部分。 这可能是由于服务仍在处理该操作，或因为该服务无法发送回复消息。 请考虑增加操作超时值 （通过强制转换将通道/代理分配给 IContextChannel 并设置 OperationTimeout 属性），确保服务能够连接到客户端。

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

迁移到目标 VM 时运行中的源，如 Azure IaaS 实例，与不同的网络直接转换无法完成源使用静态 IP 地址时。 

此行为是默认设置，以防止用户、 应用程序和连接通过 IP 地址的脚本从迁移后的连接问题。 从旧的源计算机中的 IP 地址移到新的目标时，它不会与匹配的新网络子网信息和可能是 DNS 和 WINS。

解决此问题，在同一网络上执行到一台计算机的迁移。 然后将该计算机移到新的网络，并重新分配其 IP 信息。 例如，如果迁移到 Azure IaaS，第一次迁移到本地 VM，然后使用 Azure Migrate 迁移到 Azure VM。  

我们已在 Windows Server 2019 的更高版本中解决此问题。 现在，我们将允许您指定不会改变目标服务器的网络设置的迁移。 我们可能会发布到 Windows Server 2019 作为正常的每月更新周期的一部分的现有版本的更新。 


## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)
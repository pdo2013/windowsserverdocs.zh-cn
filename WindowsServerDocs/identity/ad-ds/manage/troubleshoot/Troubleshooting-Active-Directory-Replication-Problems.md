---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Active Directory 复制问题疑难解答
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffc0933f70a2ab518c575a944efdac4c2dcd6a1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869418"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Active Directory 复制问题疑难解答

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 复制问题可以有多个不同的源。 例如，域名系统 (DNS) 问题、 网络问题或安全问题所有可以导致 Active Directory 复制失败。 

本主题的其余部分介绍的工具和常规的方法，以修复 Active Directory 复制错误。 以下子主题介绍症状、 原因和如何解决特定的复制错误：

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>简介和 Active Directory 复制进行故障排除的资源

入站或出站复制失败将导致代表复制拓扑、 复制计划、 域控制器、 用户、 计算机、 密码、 安全组、 组成员身份和组策略不一致的 Active Directory 对象之间的域控制器。 目录不一致和复制失败和可以阻止组策略的应用程序，并会导致操作失败或不一致的结果，具体取决于域控制器，从中获得该操作的访问控制权限。 Active Directory 域服务 (AD DS) 取决于网络连接、 名称解析、 身份验证和授权、 目录数据库、 复制拓扑和复制引擎。 当复制问题的根本原因并不是显而易见的时确定的许多可能的原因的原因需要系统化消除可能的原因。

有关基于 UI 的工具，来帮助监视复制和诊断错误，请参阅[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)

提供了有关一个综合文档，介绍如何使用 Repadmin 工具来排查 Active Directory 复制的;请参阅[监视和故障排除 Active Directory 复制使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

有关 Active Directory 复制的工作原理的信息，请参阅以下技术参考：

- [Active Directory 复制模型技术参考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Director 复制拓扑技术参考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具的解决方案建议

理想情况下，红色 （错误） 和目录服务事件日志中的黄色 （警告） 事件建议在复制故障导致源或目标域控制器的特定约束。 如果事件消息建议的解决方案的步骤，请尝试在事件中所述的步骤。 Repadmin 工具和其他诊断工具还提供可帮助你解决复制故障的信息。 

有关使用 Repadmin 进行故障排除复制问题的详细信息，请参阅[监视和故障排除 Active Directory 复制使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>不再故意中断或硬件故障

有时由于故意中断发生复制错误。 例如，在解决 Active Directory 复制问题时，排除有意断开连接和硬件故障或升级第一次。

### <a name="intentional-disconnections"></a>有意断开连接

如果尝试使用其内置于暂存站点和当前处于脱机状态，等待其最终的生产站点 （远程站点，例如分支机构中部署的域控制器复制域控制器通过报告复制错误)，可以考虑为这些复制错误。 为了避免分隔的域控制器从复制拓扑的很长时间，它将导致连续错误，直到重新连接域控制器，请考虑最初将此类计算机添加为成员服务器和使用从介质安装若要安装 Active Directory 域服务 (AD DS) 的 IFM) 方法。 可以使用 Ntdsutil 命令行工具创建安装介质，可以将存储在可移动介质 （CD、 DVD 或其他介质） 并寄送到目标站点。 然后，可以使用安装媒体在站点上，而不复制使用的域控制器上安装 AD DS。 

### <a name="hardware-failures-or-upgradestitle"></a>硬件故障或升级</title>

如果由于硬件故障 （例如，主板、 磁盘子系统或硬盘驱动器故障） 而出现复制问题，通知服务器所有者，以便可以解决硬件问题。

定期的硬件升级也会导致域控制器无法停止服务。 请确保服务器所有者已预先进行此类中断通信的一个优秀的系统。

### <a name="firewall-configuration"></a>防火墙配置

默认情况下，Active Directory 复制远程过程调用 (Rpc) 进行动态通过端口 135 上通过 RPC 终结点映射器 (RPCSS) 的可用端口。 请确保具有高级安全性的 Windows 防火墙并正确配置其他防火墙以允许复制。 有关 Active Directory 复制和端口设置为指定端口的信息，请参阅[一文 224196 Microsoft 知识库中相应](https://go.microsoft.com/fwlink/?LinkId=22578)。 

有关 Active Directory 复制使用的端口的信息，请参阅[Active Directory 复制工具和设置](https://go.microsoft.com/fwlink/?LinkId=123774)。

有关通过防火墙管理 Active Directory 复制的信息，请参阅[通过防火墙的 Active Directory 复制](https://go.microsoft.com/fwlink/?LinkId=123775)。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>响应故障使用旧版的服务器运行 Windows 2000 Server

如果运行 Windows 2000 Server 的域控制器出现故障的时间超过逻辑删除生存期的最大天数，解决方法是始终相同： 

1. 将服务器从企业网络移动到专用网络。
2. 强制删除 Active Directory，或者重新安装操作系统。
3. 从 Active Directory 删除服务器元数据，以便不能恢复的服务器对象。 

可以使用脚本以清理服务器元数据，在大多数 Windows 操作系统上。 有关使用此脚本的信息，请参阅[删除 Active Directory 域控制器元数据](https://go.microsoft.com/fwlink/?LinkID=123599)。 

默认情况下，会删除 NTDS 设置对象是为 14 天的期间自动恢复。 因此，如果不删除服务器元数据 （使用 Ntdsutil 或以前所述执行元数据清除的脚本），在目录中，它会提示进行的复制尝试恢复过程的服务器元数据。 在这种情况下，错误将持续记录由于不能与缺失的域控制器进行复制。

## <a name="root-causes"></a>根本原因

如果排除有意断开连接、 硬件故障和过时的 Windows 2000 域控制器时，请复制问题的其余部分几乎始终具有以下的根本原因之一：

- 网络连接：网络连接可能不可用，或者未正确配置网络设置。
- 名称解析：DNS 配置错误会导致复制失败的常见原因。
- 身份验证和授权：当域控制器尝试与其复制伙伴建立连接时，身份验证和授权问题会导致"拒绝访问"错误。
- 目录数据库 （存储）：目录数据库可能不能以足够快的速度处理事务才能跟上复制超时。
- 复制引擎：如果站点间复制计划太短，复制队列可能太大而无法处理中所需的出站复制计划的时间。 在这种情况下，复制的某些更改可以是无限期可能会停止、 足够长，超过逻辑删除生存期。
- 复制拓扑：域控制器必须具有映射到实际广域网 (WAN) 或虚拟专用网络 (VPN) 连接的 AD DS 中的站点间的链接。 如果在不受网络的实际站点拓扑的 AD DS 复制拓扑中创建的对象，需要配置错误的拓扑的复制将失败。

## <a name="general-approach-to-fixing-problems"></a>解决问题的常规方法

使用以下常规方法来解决复制问题： 

1. 每日、 监视复制运行状况，或者使用 Repadmin.exe 来每日检索复制状态。
2. 尝试通过使用事件消息和本指南中描述的方法中及时地解决报告的任何故障。 如果软件可能会导致问题，然后继续其他解决方案中卸载软件。
3. 如果不能通过任何已知方法解决此问题，导致复制失败，从服务器中删除 AD DS，并重新安装 AD DS。 重新安装 AD DS 的详细信息，请参阅[取消配置域控制器](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果在服务器连接到网络时，不能正常情况下删除 AD DS，则可使用以下方法之一来解决该问题：

   - 强制删除 AD DS 中目录服务还原模式 (DSRM) 清理服务器元数据，然后重新安装 AD DS。
   - 重新安装操作系统，然后重新生成的域控制器。

有关强制删除 AD DS 的详细信息，请参阅[强制删除域控制器](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 检索复制状态</title>

复制状态将为您要评估的目录服务状态的重要方法。 如果复制有效且未出错，您知道都处于联机状态的域控制器。 您还知道以下系统和服务正常工作：


- DNS 基础结构
- Kerberos 身份验证协议
- Windows 时间服务 (W32time)
- 远程过程调用 (RPC)
- 网络连接

使用 Repadmin 监视复制状态每日通过运行评估在林中所有域控制器的复制状态的命令。 该过程将生成可以在 Microsoft Excel 和复制故障的筛选器中打开一个.csv 文件。

以下过程可用于检索在林中所有域控制器的复制状态。 

要求

**Enterprise Admins** 中的成员身份或同等身份是完成此过程所需的最低要求。 

工具：

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>若要生成的域控制器的 repadmin /showrepl 电子表格

1. 以管理员身份打开命令提示符：在开始菜单中，右键单击命令提示符，然后单击以管理员身份运行。 如果出现用户帐户控制对话框中，如果需要，提供企业管理员凭据，然后单击继续。
2. 在命令提示符处，键入以下命令，然后按 ENTER: `repadmin /showrepl * /csv > showrepl.csv`
3. 打开 Excel。
4. 单击 Office 按钮，单击打开，导航到 showrepl.csv，，然后单击打开。
5. 隐藏或删除列，以及传输类型列中，按如下所示：
6. 选择你想要隐藏或删除的列。

   - 若要隐藏的列，右键单击的列，然后单击隐藏。
   - 若要删除列，右键单击所选的列，然后单击删除。

7. 选择在列标题行的下方的第 1 行。 在视图选项卡上，单击冻结窗格，然后单击冻结顶端行。
8. 选择整个电子表格。 在数据选项卡上，单击筛选器。
9. 在最后一个成功时间列中，单击向下箭头，然后单击升序排序。
10. 在源 DC 专栏中，单击向下键的筛选器，指向文本筛选器，然后单击自定义筛选器。
11. 在自定义自动筛选对话框中，在显示行不包含其中，单击下。 在相邻的文本框中，键入<userInput>del</userInput>以消除从视图的结果中删除域控制器。
12. 上次失败时间列，但使用的值不等于、，然后键入值 0 为重复步骤 11。
13. 解决复制失败。

对于每个林中的域控制器，电子表格将显示源复制伙伴，时间上一次发生复制，以及每个命名上下文 （目录分区） 上次复制发生失败的时间。 通过在 Excel 中使用自动筛选，可以查看用于处理仅，域控制器的复制运行状况失败，域控制器或最低或大多数当前的域控制器和您所见的复制伙伴进行复制的已成功。

## <a name="replication-problems-and-resolutions"></a>复制问题和解决方法

复制问题都会报告在事件消息和各种应用程序或服务尝试执行操作时发生的错误消息中。 理想情况下，这些消息被收集的监视应用程序或检索复制状态。

目录服务事件日志中记录的事件消息中标识为大多数复制问题。 复制问题也可能将其标识的输出中的错误消息的形式<system>repadmin /showrepl</system>命令。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin /showrepl 错误消息，以表明复制问题

若要标识 Active Directory 复制问题，请使用<system>repadmin /showrepl</system>命令，如在上一部分中所述。 下表显示了此命令生成，以及错误和解决方案提供的错误的主题的链接的根本原因的错误消息。

|Repadmin 错误|根本原因|解决方案|
| --- | --- | --- |
|自上次复制与此时间服务器已经超过 tombstone 生存时间。|域控制器发生故障与命名的源域控制器很长足够为已逻辑删除，删除复制，而且从 AD DS 垃圾回收的入站的复制。|事件 ID 2042:它已过太久以来复制此计算机|
|没有入站的邻居。|Repadmin /showrepl 由生成输出的"入站邻居"部分中未不显示任何项，如果域控制器无法建立与另一个域控制器的复制链接。|解决复制连接性问题（事件 ID 1925）| 
|访问被拒绝。|复制链接两个域控制器之间存在，但由于身份验证失败而无法正确执行复制。|解决复制安全问题| 
|< 日期-时间 > 上次尝试失败，出现"目标帐户名称不正确。"|此问题可以与连接、 DNS 或身份验证问题。 如果这是 DNS 错误，本地域控制器无法解析的全局唯一标识符 (GUID)-根据其复制伙伴的 DNS 名称。|解决复制 DNS 查找问题 (事件 Id 1925、 2087、 2088年) 修复复制安全问题解决复制连接性问题 (事件 ID 1925)| 
|LDAP 错误 49。|域控制器计算机帐户可能不会同步密钥分发中心 (KDC)。|解决复制安全问题| 
|无法打开 LDAP 连接到本地主机|管理工具无法联系 AD DS。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）| 
|Active Directory 复制已被占用。|优先级较高的复制请求，例如使用 repadmin /sync 命令手动生成的请求被中断的入站复制进度。|等待复制完成。 此信息性消息指示正常操作。| 
|复制发布，等待。| 域控制器发布的复制请求，正在等待答案。 正在为此源中复制。|等待复制完成。 此信息性消息指示正常操作。| 

下表列出了常见的事件指示可能存在问题的 Active Directory 复制，以及根导致的问题和解决方案提供的问题的主题的链接。 

|事件 ID 和来源|根本原因|解决方案|
| --- | --- | --- | 
|1311 NTDS KCC|在 AD DS 中的复制配置信息不准确地反映物理网络的拓扑。|解决复制拓扑问题（事件 ID 1311）| 
|1388 NTDS 复制|不起作用，严格复制一致性和延迟对象已复制到域控制器。|修复复制延迟对象问题（事件 ID 1388、1988、2042）|
|1925 NTDS KCC|尝试建立可写目录分区的复制链接失败。 此事件可以具有不同的原因，根据具体的错误。| 解决复制连接性问题 (事件 ID 1925) 解决复制 DNS 查找问题 (事件 Id 1925、 2087、 2088年）| 
|1988 NTDS 复制|本地域控制器已经尝试从因为它可能已删除和已垃圾回收不存在本地域控制器上的源域控制器复制对象。 复制不会与该合作伙伴此目录分区继续，直到解决这种情况。|修复复制延迟对象问题（事件 ID 1388、1988、2042）|
|2042 NTDS 复制|复制一个 tombstone 生存时间，不发生与此合作伙伴，复制无法继续。|修复复制延迟对象问题（事件 ID 1388、1988、2042）| 
|2087 NTDS 复制|AD DS 无法解析为 IP 地址和失败的复制源域控制器的 DNS 主机名。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）| 
|2088 NTDS 复制 |AD DS 无法解析为 IP 地址，但已成功复制的源域控制器的 DNS 主机名。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）|
|5805 网络登录|如果将计算机帐户无法进行身份验证，这通常由多个实例相同的计算机名称的或不会复制到每个域控制器的计算机名称。|解决复制安全问题| 

有关复制概念的详细信息，请参阅[Active Directory 复制技术](https://go.microsoft.com/fwlink/?LinkId=41950)。
  
## <a name="next-steps"></a>后续步骤

有关详细信息，包括支持特定于错误代码的文章请参阅支持文章：[如何排查常见的 Active Directory 复制错误](https://support.microsoft.com/help/3108513)

---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Active Directory 复制问题疑难解答
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409067"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Active Directory 复制问题疑难解答

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 复制问题可具有多个不同的源。 例如，域名系统（DNS）问题、网络问题或安全问题都可能导致复制失败 Active Directory。 

本主题的其余部分将介绍工具和解决 Active Directory 复制错误的一般方法。 以下子主题涵盖了症状、原因以及如何解决特定的复制错误：

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Active Directory 复制疑难解答的简介和资源

入站或出站复制失败导致 Active Directory 对象代表复制拓扑、复制计划、域控制器、用户、计算机、密码、安全组、组成员身份和组策略不一致域控制器之间。 目录不一致和复制失败会导致操作失败或不一致的结果，具体取决于与该操作联系的域控制器，并可阻止应用组策略和访问控制权限。 Active Directory 域服务（AD DS）依赖于网络连接、名称解析、身份验证和授权、目录数据库、复制拓扑和复制引擎。 如果复制问题的根本原因不是很明显，则确定可能的原因可能会导致系统排除可能的原因。

有关基于 UI 的工具以帮助监视复制和诊断错误，请参阅[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)

有关介绍如何使用 Repadmin 工具解决 Active Directory 复制可用的综合文档，请执行下面的操作：请参阅[使用 Repadmin 进行 Active Directory 复制的监视和故障排除](https://go.microsoft.com/fwlink/?LinkId=122830)。

有关 Active Directory 复制的工作原理的信息，请参阅以下技术参考：

- [Active Directory 复制模型技术参考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [活动控制器复制拓扑技术参考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>事件和工具解决方案建议

理想情况下，目录服务事件日志中的红色（错误）和黄色（警告）事件建议在源或目标域控制器上导致复制失败的特定约束。 如果事件消息为解决方案建议步骤，请尝试执行事件中所述的步骤。 Repadmin 工具和其他诊断工具还提供可帮助您解决复制故障的信息。 

有关使用 Repadmin 排查复制问题的详细信息，请参阅[使用 repadmin 进行 Active Directory 复制的监视和故障排除](https://go.microsoft.com/fwlink/?LinkId=122830)。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>排除有意中断或硬件故障

有时会由于蓄意中断而发生复制错误。 例如，在对 Active Directory 复制问题进行故障排除时，请先排除有意断开和硬件故障或升级。

### <a name="intentional-disconnections"></a>有意断开

如果某个域控制器报告复制错误，该控制器尝试使用已在暂存站点中生成的域控制器进行复制，并且当前处于脱机状态，但在最终生产站点（如分支机构）中等待其部署，），您可以考虑这些复制错误。 若要避免长时间将域控制器从复制拓扑中分离出来（这会导致连续错误，直到域控制器重新连接），请考虑将此类计算机初始作为成员服务器并使用从介质安装（IFM）方法安装 Active Directory 域服务（AD DS）。 可以使用 Ntdsutil 命令行工具创建可存储在可移动介质（CD、DVD 或其他介质）上并寄送到目标站点的安装介质。 然后，你可以使用安装媒体将 AD DS 安装在站点上的域控制器上，而无需使用复制。 

### <a name="hardware-failures-or-upgradestitle"></a>硬件故障或升级 @ no__t-0

如果由于硬件故障（例如，主板、磁盘子系统或硬盘驱动器故障）导致复制问题，请通知服务器所有者，以便能够解决硬件问题。

定期硬件升级还会导致域控制器停止服务。 请确保服务器所有者有一种系统提前传达此类中断。

### <a name="firewall-configuration"></a>防火墙配置

默认情况下，通过端口135上的 RPC 终结点映射器（RPCSS）在可用端口上动态地执行复制远程过程调用（Rpc） Active Directory。 请确保正确配置具有高级安全性的 Windows 防火墙和其他防火墙以允许复制。 有关指定 Active Directory 复制和端口设置的端口的信息，请参阅[Microsoft 知识库中的文章 224196](https://go.microsoft.com/fwlink/?LinkId=22578)。 

有关 Active Directory 复制使用的端口的信息，请参阅[Active Directory 复制工具和设置](https://go.microsoft.com/fwlink/?LinkId=123774)。

有关通过防火墙管理 Active Directory 复制的信息，请参阅[通过防火墙 Active Directory 复制](https://go.microsoft.com/fwlink/?LinkId=123775)。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>响应运行 Windows 2000 服务器的过时服务器的故障

如果运行 Windows 2000 服务器的域控制器的运行时间超过逻辑删除生存期中的天数，则解决方案始终相同： 

1. 将服务器从企业网络转移到专用网络。
2. 强制删除 Active Directory 或重新安装操作系统。
3. 从 Active Directory 中删除服务器元数据，以便无法恢复服务器对象。 

您可以使用脚本在大多数 Windows 操作系统上清理服务器元数据。 有关使用此脚本的信息，请参阅[删除 Active Directory 域控制器元数据](https://go.microsoft.com/fwlink/?LinkID=123599)。 

默认情况下，删除的 NTDS 设置对象会在14天内自动恢复。 因此，如果您未删除服务器元数据（如使用 Ntdsutil 或前面提到的脚本执行元数据清理），则会在目录中恢复服务器元数据，从而会出现复制尝试。 在这种情况下，由于无法与缺少的域控制器进行复制，将永久记录错误。

## <a name="root-causes"></a>根本原因

如果你排除蓄意断开、硬件故障和过时的 Windows 2000 域控制器的问题，复制问题几乎始终具有以下根本原因之一：

- 网络连接：网络连接可能不可用，或者网络设置配置不正确。
- 名称解析：DNS 错误配置是复制失败的一个常见原因。
- 身份验证和授权：当域控制器尝试连接到其复制伙伴时，身份验证和授权问题会导致 "拒绝访问" 错误。
- 目录数据库（存储）：目录数据库可能无法足够快地处理事务，因而无法满足复制超时的需要。
- 复制引擎：如果站点间复制计划太短，则复制队列可能太大，无法在出站复制计划所需的时间进行处理。 在这种情况下，某些更改的复制可能会无限期地停止，足以超过 tombstone 生存时间。
- 复制拓扑：域控制器的 AD DS 中的站点间链接必须映射到实际的广域网（WAN）或虚拟专用网络（VPN）连接。 如果在网络的实际站点拓扑不支持的复制拓扑 AD DS 中创建对象，则需要配置错误的拓扑的复制会失败。

## <a name="general-approach-to-fixing-problems"></a>解决问题的常规方法

使用以下常规方法解决复制问题： 

1. 每天监视复制运行状况，或使用 Repadmin 每日检索复制状态。
2. 使用事件消息和本指南中所述的方法尝试及时解决任何报告的故障。 如果软件可能导致此问题，请先卸载该软件，然后再继续阅读其他解决方案。
3. 如果任何已知的方法无法解决导致复制失败的问题，请从服务器中删除 AD DS，然后重新安装 AD DS。 有关重新安装 AD DS 的详细信息，请参阅[解除域控制器的授权](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果在服务器连接到网络时无法正常删除 AD DS，请使用以下方法之一来解决问题：

   - 在目录服务还原模式（DSRM）中强制 AD DS 删除，清理服务器元数据，然后重新安装 AD DS。
   - 重新安装操作系统，并重建域控制器。

有关强制删除 AD DS 的详细信息，请参阅[强制删除域控制器](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 检索复制状态 @ no__t-0

复制状态是评估目录服务状态的一项重要方式。 如果复制正常工作而没有出现错误，则你知道联机的域控制器。 还知道以下系统和服务正在运行：


- DNS 基础结构
- Kerberos 身份验证协议
- Windows 时间服务（W32time）
- 远程过程调用（RPC）
- 网络连接

使用 Repadmin 通过运行一个命令来监视林中所有域控制器的复制状态，每天监视复制状态。 此过程将生成一个 .csv 文件，您可以在 Microsoft Excel 中打开该文件并筛选复制失败。

你可以使用以下过程来检索林中所有域控制器的复制状态。 

要求

**Enterprise Admins** 中的成员身份或同等身份是完成此过程所需的最低要求。 

工具：

- Repadmin.exe
- Excel （Microsoft Office）

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>为域控制器生成 repadmin/showrepl 电子表格

1. 以管理员身份打开命令提示符：在 "开始" 菜单上，右键单击 "命令提示符"，然后单击 "以管理员身份运行"。 如果出现 "用户帐户控制" 对话框，请提供企业管理员凭据（如果需要），然后单击 "继续"。
2. 在命令提示符下，键入以下命令，然后按 ENTER： `repadmin /showrepl * /csv > showrepl.csv`
3. 打开 Excel。
4. 单击 "Office" 按钮，单击 "打开"，导航到 "showrepl"，然后单击 "打开"。
5. 隐藏或删除列 A 以及 "传输类型" 列，如下所示：
6. 选择要隐藏或删除的列。

   - 若要隐藏列，请右键单击该列，然后单击 "隐藏"。
   - 若要删除列，请右键单击所选的列，然后单击 "删除"。

7. 选择列标题行下的第1行。 在 "视图" 选项卡上，单击 "冻结窗格"，然后单击 "冻结首行"。
8. 选择整个电子表格。 在 "数据" 选项卡上，单击 "筛选器"。
9. 在 "上次成功时间" 列中，单击向下箭头，然后单击 "升序排序"。
10. 在 "源 DC" 列中，单击筛选器向下箭头，指向 "文本筛选器"，然后单击 "自定义筛选器"。
11. 在 "自定义自动筛选" 对话框中的 "显示行位置" 下，单击 "不包含"。 在相邻的文本框中，键入<userInput>del</userInput>以消除查看删除的域控制器的结果。
12. 对于 "上次失败时间" 列重复步骤11，但使用 "不等于" 值，然后键入值0。
13. 解决复制失败问题。

对于林中的每个域控制器，电子表格会显示源复制伙伴、复制上一次发生的时间，以及每个命名上下文（目录分区）发生最后一次复制失败的时间。 通过在 Excel 中使用自动筛选，你可以仅查看工作域控制器的复制运行状况、仅失败的域控制器或至少或最新的域控制器，并且你可以看到正在复制的复制伙伴顺利.

## <a name="replication-problems-and-resolutions"></a>复制问题和解决方法

在应用程序或服务尝试操作时出现的事件消息和各种错误消息中会报告复制问题。 理想情况下，这些消息由监视应用程序收集，或在检索复制状态时收集。

大多数复制问题都在目录服务事件日志中记录的事件消息中进行标识。 还可以在<system>repadmin/showrepl</system>命令的输出中以错误消息的形式标识复制问题。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>repadmin/showrepl 错误消息，指出复制问题

若要确定 Active Directory 复制问题，请使用<system>repadmin/showrepl</system>命令，如前一部分中所述。 下表显示了此命令生成的错误消息，以及错误的根本原因以及提供错误解决方案的主题的链接。

|Repadmin 错误|根本原因|解决方案|
| --- | --- | --- |
|自上次复制此服务器以来的时间超出了 tombstone 生存时间。|域控制器与命名的源域控制器的入站复制失败时间足以用于从 AD DS 中逻辑删除、复制和垃圾收集。|事件 ID 2042：它太长，因为此计算机已复制|
|无入站邻居。|如果在 repadmin/showrepl 生成的输出的 "入站邻居" 部分中未显示任何项，则域控制器无法与另一个域控制器建立复制链接。|解决复制连接性问题（事件 ID 1925）| 
|拒绝访问。|两个域控制器之间存在一个复制链接，但由于身份验证失败，无法正确执行复制。|解决复制安全问题| 
|上次尝试 < 日期-时间 > 失败，出现 "目标帐户名不正确"。|此问题可能与连接、DNS 或身份验证问题相关。 如果这是 DNS 错误，则本地域控制器无法解析其复制伙伴的基于全局唯一标识符（GUID）的 DNS 名称。|解决复制 DNS 查找问题（事件 Id 1925、2087、2088）修复复制安全问题修复复制连接问题（事件 ID 1925）| 
|LDAP 错误49。|域控制器计算机帐户可能未与密钥发行中心（KDC）同步。|解决复制安全问题| 
|无法打开到本地主机的 LDAP 连接|管理工具无法联系 AD DS。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）| 
|Active Directory 复制已被抢占。|入站复制的进度由较高优先级的复制请求中断，如使用 repadmin/sync 命令手动生成的请求。|等待复制完成。 此信息性消息指示正常操作。| 
|复制已发布，正在等待。| 域控制器已发布复制请求并正在等待答案。 正在从此源进行复制。|等待复制完成。 此信息性消息指示正常操作。| 

下表列出了一些常见事件，这些事件可能指示 Active Directory 复制的问题，以及问题的根本原因以及指向提供问题解决方案的主题的链接。 

|事件 ID 和源|根本原因|解决方案|
| --- | --- | --- | 
|1311 NTDS KCC|AD DS 中的复制配置信息不能准确反映网络的物理拓扑。|解决复制拓扑问题（事件 ID 1311）| 
|1388 NTDS 复制|严格复制一致性无效，已将延迟对象复制到域控制器。|修复复制延迟对象问题（事件 ID 1388、1988、2042）|
|1925 NTDS KCC|尝试为可写目录分区建立复制链接失败。 此事件的原因可能不同，具体取决于错误。| 解决复制连接问题（事件 ID 1925）修复复制 DNS 查找问题（事件 Id 1925、2087、2088）| 
|1988 NTDS 复制|本地域控制器已尝试从源域控制器复制某个对象，但该对象可能已被删除并已进行垃圾回收。 在解决此问题之前，复制不会继续与此伙伴进行复制。|修复复制延迟对象问题（事件 ID 1388、1988、2042）|
|2042 NTDS 复制|对于逻辑删除生存期，此伙伴未发生复制，因此无法继续复制。|修复复制延迟对象问题（事件 ID 1388、1988、2042）| 
|2087 NTDS 复制|AD DS 无法将源域控制器的 DNS 主机名解析为 IP 地址，并且复制失败。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）| 
|2088 NTDS 复制 |AD DS 无法将源域控制器的 DNS 主机名解析为 IP 地址，但复制已成功。|解决复制 DNS 查找问题（事件 ID 1925、2087、2088）|
|5805 Net Logon|计算机帐户无法进行身份验证，这通常是由同一计算机名的多个实例或不复制到每个域控制器的计算机名称导致的。|解决复制安全问题| 

有关复制概念的详细信息，请参阅[Active Directory 复制技术](https://go.microsoft.com/fwlink/?LinkId=41950)。
  
## <a name="next-steps"></a>后续步骤

有关详细信息，包括特定于错误代码的支持文章，请参阅支持文章：[如何排查常见 Active Directory 复制错误](https://support.microsoft.com/help/3108513)

---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: "Active Directory 复制问题疑难解答"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: eb93ea7cab297d70aa86b71bd5466004e47bfeaf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Active Directory 复制问题疑难解答

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


有多种不同的来源 Active Directory 复制的问题。 例如，域名系统 (DNS) 的问题、网络问题或安全问题所有可能导致 Active Directory 复制失败。 

本主题中的其余部分介绍工具和常规方法来解决 Active Directory 复制错误。 动手实验中的说明如何排除 Active Directory 复制的问题，请参阅[TechNet 虚拟实验：Active Directory 复制错误疑难解答](https://go.microsoft.com/?linkid=9844718)

以下子主题键盘盖症状、的原因，以及解决特定复制错误：
   
[修复复制逗留对象问题 (事件 Id 1388，于 1988 年由 2042 年)](https://technet.microsoft.com/library/cc949124.aspx)
  
[修复复制安全问题](https://technet.microsoft.com/library/cc949132.aspx)

[解决复制 DNS 查阅问题 (事件 Id 1925，2087，2088 年)](https://technet.microsoft.com/library/cc949133.aspx)
  
[修复复制连接问题 (事件 ID 1925)](https://technet.microsoft.com/library/cc949131.aspx)
  
[修复复制拓扑问题 (事件 ID 1311)](https://technet.microsoft.com/library/cc949129.aspx)
  
[验证支持目录复制 DNS 功能](https://technet.microsoft.com/library/dd728017.aspx)
  
[复制错误 8614 Active Directory 无法与该服务器复制由于超过的 tombstone 生存期自上次复制与此服务器的时间。](https://technet.microsoft.com/library/replication-error-8614-the-active-directory-cannot-replicate-with-this-server-because-the-time-since-the-last-replication-with-this-server-has-exceeded-the-tombstone-lifetime.aspx)
     
[复制错误 8524 DSA 操作将无法继续由于 DNS 查询失败](https://technet.microsoft.com/library/replication-error-8524-the-dsa-operation-is-unable-to-proceed-because-of-a-dns-lookup-failure.aspx)
   
[复制错误 8456 或 8457 源 |目前拒绝目标服务器复制请求](https://technet.microsoft.com/library/replication-error-8456-the-source-server-is-currently-rejecting-replication-requests-or-replication-error-8457-the-destination-server-is-currently-rejecting-replication-requests.aspx)
   
[复制错误 8453 复制无法访问](https://technet.microsoft.com/library/replication-error-8453-replication-access-was-denied.aspx)
  
[复制错误 8452 命名上下文正在被删除或不会将其复制从指定服务器](https://technet.microsoft.com/library/replication-error-8452-the-naming-context-is-in-the-process-of-being-removed-or-is-not-replicated-from-the-specified-server.aspx)
   
[复制的错误 5 拒绝访问](https://technet.microsoft.com/library/replication-error-5-access-is-denied.aspx)
  

      
      

  
[复制错误-2146893022 不正确的目标主要名称](https://technet.microsoft.com/library/replication-error-2146893022-the-target-principal-name-is-incorrect.aspx)
  

      
      

  
[有复制错误 1753 年有更多端点可从端点映射程序](https://technet.microsoft.com/library/replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper.aspx)
  

      
      

  
[复制错误 1722 年 RPC 服务器不可用](https://technet.microsoft.com/library/replication-error-1722-the-rpc-server-is-unavailable.aspx)
  

      
      

  
[复制错误 1396 年登录失败目标的帐户名称不正确](https://technet.microsoft.com/library/replication-error-1396-logon-failure-the-target-account-name-is-incorrect.aspx)
  

      
      

  
[复制错误 1256 年远程系统不可用](https://technet.microsoft.com/library/replication-error-1256-the-remote-system-is-not-available.aspx)
  

      
      

  
[复制访问硬盘磁盘操作时出现错误 1127 年失败甚至后重试](https://technet.microsoft.com/library/replication-error-1127-while-accessing-the-hard-disk-a-disk-operation-failed-even-after-retries.aspx)
  

      
      

  
[复制错误 8451 复制操作遇到出现数据库错误](https://technet.microsoft.com/library/replication-error-8451-the-replication-operation-encountered-a-database-error.aspx)
  

      
      

  
[复制错误 8606 不足属性给定创建一个](https://technet.microsoft.com/library/replication-error-8606-insufficient-attributes-were-given-to-create-an-object.aspx)
  

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>引入和资源的 Active Directory 复制疑难解答

归巢或站复制故障表示复制拓扑、复制日程安排，域控制器、用户、的计算机、密码、安全组、组成员和不一致域控制器之间的组策略的 Active Directory 对象将使。 目录不一致、复制故障会导致操作故障或不一致的结果，具体取决于联系以执行操作时，域控制器可以阻止的组策略应用程序和访问控制权限。 Active Directory 域服务(广告 DS) 取决于网络连接名称分辨率、身份验证和授权、目录数据库、复制拓扑，，复制引擎。 当不可立即明显复制问题的根本原因时，确定原因可能很多原因要求系统性消除可能的原因。 

基于 UI 工具，以帮助监视器复制和诊断错误，请参阅[Active Directory 复制状态工具](https://www.microsoft.com/download/details.aspx?id=30005)。 另外，还有[动手实验](https://go.microsoft.com/?linkid=9844718)，展示如何使用 Active Directory 复制状态和其他工具来诊断错误。 

复制是可用，则为介绍了如何使用 Repadmin 工具进行疑难解答 Active Directory 的完整的文档请参阅[监视和疑难解答的 Active Directory 复制使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

有关 Active Directory 复制的工作原理的信息，请参阅下面的技术参考：


- [Active Directory 复制型号技术参考](https://go.microsoft.com/fwlink/?LinkId=65958)
- [活动主管复制拓扑技术参考](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>活动和工具解决方案建议

最好（错误）红色和黄色（警告）事件目录服务事件日志中的建议复制故障导致在源或目标该域控制器的特定约束。 如果事件消息建议的解决方案的步骤，尝试的步骤，述事件。 Repadmin 工具和其他诊断工具，还提供，可帮助你解决复制故障的信息。 

有关使用 Repadmin 复制问题疑难解答的详细信息，请参阅[监视和疑难解答的 Active Directory 复制使用 Repadmin](https://go.microsoft.com/fwlink/?LinkId=122830)。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>允许有意中断或硬件故障

有时会发生复制错误，因为有意中断。 例如，当疑难解答 Active Directory 复制的问题，排除有意断开和硬件故障或升级第一次。

### <a name="intentional-disconnections"></a>有意断开连接

如果在尝试复制与中某个临时站点已内置和当前处于脱机状态，等待其部署最终的产品站点（远程网站，例如分支机构）中的域控制器域控制器报告了复制错误，可以考虑这些复制错误。 若要避免从复制拓扑长期，这将导致连续错误，直到重新连接域控制器，域控制器的分隔考虑为成员服务器最初添加此类计算机和使用安装媒体 (IFM) 方法从安装 Active Directory 域服务 (广告 DS)。 你可以使用 Ntdsutil 命令行工具创建安装媒体，您可以在可移动媒体（CD、DVD 或其他媒体）上存储和发布到目标网站。 然后，你可以使用安装媒体的站点，无需使用复制的域控制器上安装广告 DS。 

### <a name="hardware-failures-or-upgradestitle"></a>硬件故障或升级</title>

复制出现问题，则因硬件故障（例如，出现故障主板、磁盘子系统或硬盘驱动器），以便可以解决硬件问题通知服务器所有者。

定期硬件升级，也可能导致域控制器，以便不能提供服务。 确保你服务器所有者有提前进行通信此类中断的一个优秀的系统。

### <a name="firewall-configuration"></a>防火墙配置

默认情况下，Active Directory 复制远程过程调用 (Rpc) 会发生动态通过提供端口端口 135 上通过 RPC 端点映射程序 (RPCSS)。 请确保该高级安全 Windows 防火墙和其他防火墙配置无误允许进行复制。 有关指定的 Active Directory 复制和端口设置端口的信息，请参阅[文章 224196 Microsoft 知识库中的](https://go.microsoft.com/fwlink/?LinkId=22578)。 

有关使用 Active Directory 复制的端口的信息，请参阅[Active Directory 复制工具和设置](https://go.microsoft.com/fwlink/?LinkId=123774)。 

有关管理通过防火墙 Active Directory 复制信息，请参阅[防火墙通过 Active Directory 复制](https://go.microsoft.com/fwlink/?LinkId=123775)。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>运行 Windows 2000 Server 过期服务器的故障响应

如果运行的 Windows 2000 Server 域控制器的时间比数天的 tombstone 生存期失败，解决方法是始终相同： 

1. 公司的网络将服务器移至专用网络中。
2. 请强行删除 Active Directory 或重新安装操作系统。
3. 从 Active Directory 删除服务器元数据，以便服务器对象无法恢复。 

你可以使用脚本清理服务器上大多数 Windows 操作系统的元数据。 有关使用此脚本的信息，请参阅[删除 Active Directory 的域控制器元数据](https://go.microsoft.com/fwlink/?LinkID=123599)。 

默认情况下，将会删除各自的对象有 14 天内自动恢复。 因此，如果你不会删除服务器元数据（使用 Ntdsutil 或上文所述以执行元数据清理脚本）服务器元数据还原目录，提示复制尝试发生中。 在此情况下，错误将永久记录定不能与缺少域控制器进行复制。

## <a name="root-causes"></a>根本原因

如果你判断有意断开硬件故障，过期的 Windows 2000 域控制器，请复制的问题的其余部分总是有以下的根本原因之一： 


- 网络连接：网络连接可能不可用，或者网络设置未正确配置。
- 命名分辨率：DNS 错误配置是复制故障的一个常见原因。
- 身份验证和授权：身份验证和授权的问题会导致"拒绝访问"错误，当尝试连接到其复制合作伙伴域控制器。
- 目录数据库（应用商店）：目录数据库可能无法足够快地处理事务跟复制超时。
- 复制引擎：复制队列是否太短站点间复制日程安排，可能太大，无法在所需的站复制计划的时间进行处理。 在此情况下，复制的某些更改可方面的 indefinitelypotentially，足够超过的 tombstone 生存期。
- 复制拓扑：域控制器中将映射到实际宽的区域广域网）或虚拟专用网络 (VPN) 连接的广告 DS 必须站点间链接。 如果您在由你的网络的实际网站拓扑不受支持的广告 DS 复制拓扑创建对象，需要错误配置的拓扑复制无法正常工作。

## <a name="general-approach-to-fixing-problems"></a>常规方法来解决问题

使用以下常见地修复复制的问题的方法： 


1. 每日监控复制状况，或使用 Repadmin.exe 每天检索复制状态。
2. 尝试使用的方法，述事件消息和本指南及时解决任何报告的失败情况。 软件可能会导致该问题，如果卸载该软件之前，你继续使用的其他解决方案。
3. 如果通过任何已知的方法，不能解决该问题导致复制失败，服务器删除广告 DS，然后重新安装广告 DS。 有关重新安装广告 DS 的详细信息，请参阅[停止使用域控制器](https://go.microsoft.com/fwlink/?LinkId=128290)。
4. 如果该服务器连接到该网络时不能通常删除广告 DS，，使用以下方法之一来解决该问题：
 

    - 强制清理服务器元数据的广告 DS 删除中目录服务还原模式 (DSRM)，然后重新安装广告 DS。
    - 重新安装操作系统，然后重新生成域控制器。

有关强制删除广告 DS 的详细信息，请参阅[强制域控制器删除](https://go.microsoft.com/fwlink/?LinkId=128291)。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>使用 Repadmin 检索复制状态</title>

复制状态是重要的方式，供你评估目录服务的状态。 复制正常没有任何错误，如果你知道处于联机状态域控制器。 你还知道下面的系统和服务工作：


- DNS 的基础结构。
- Kerberos 身份验证协议
- Windows 时间服务 (W32time)
- 远程过程调用 (RPC)
- 网络连接

使用 Repadmin 监视器复制状态每日通过运行评估复制状态你森林中的所有域控制器的命令。 该过程将生成.csv 可以在 Microsoft Excel 以及复制故障筛选器中打开的文件。

可以使用下面的过程中检索森林中的所有域控制器复制状态。 
      
要求
      
在会员**企业管理员**，或等效的最低要求完成此过程。 

工具： 


- Repadmin.exe 
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>若要生成 repadmin /showrepl 域控制器电子表格



1. 打开 Command prompt 下以 administrator 身份：在开始菜单命令提示符下，右键单击，然后单击以管理员身份运行。 如果显示了用户帐户控制对话框中，如有必要，提供企业管理员凭据，然后单击继续。 
2. 在命令提示符下，键入以下命令，，然后按 ENTER:`repadmin /showrepl * /csv &gt;showrepl.csv`
3. 打开 Excel。
4. 单击 Office 按钮、单击打开，导航到 showrepl.csv，，然后单击打开。
5. 隐藏或删除列 A 以及传输类型列中，如下所示：
6. 选择你想要隐藏或删除列。
      

    - 若要隐藏列，请右键单击列，，然后单击隐藏。
    - 若要删除列、右键单击所选的列，然后单击删除。
7. 选择下列标题的行 1。 在视图选项卡上，单击冻结窗格中，，然后单击冻结顶行。
8. 选择整个电子表格。 单击数据选项卡中的筛选器。
9. 在成功上次列中，单击的向下箭头，然后单击升序。
10. 在源直流列中，单击向下键滤镜，文本筛选器，请指向，然后单击自定义筛选器。
11. 在自定义自动筛选对话框中，在其中，单击不包含的显示行。 在旁边的文本框中，键入<userInput>del</userInput>以清除了的视图的结果删除域控制器。
12. 上次故障列中，但使用值不等于，，然后键入值 0 重复步骤 11。
13. 解决复制故障。

森林中的每个域控制器上的电子表格显示源复制合作伙伴，时间进行最后一次过复制，以及每个命名上下文（目录分区）最后一个复制故障发生时间。 在 Excel 中使用筛选，你可以查看复制健康方式仅，域控制器故障仅，域控制器或最或大多数当前，域控制器并且您可看到成功复制复制合作伙伴。

## <a name="replication-problems-and-resolutions"></a>复制的问题和分辨率
    
在事件消息和应用程序或服务尝试的操作时，会出现的各种错误消息报告复制的问题。 理想的情况下，这些消息会收集你监视的应用程序或检索复制状态。

在登录目录服务事件日志事件消息标识大多数复制的问题。 复制问题也可能将识别的输出中的错误消息的形式<system>repadmin /showrepl</system>命令。 

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>Repadmin /showrepl 表示复制的问题的错误消息

若要找出 Active Directory 复制的问题，请使用<system>repadmin /showrepl</system>命令，因为以前部分中所述。 下表显示错误消息，此命令产生以及错误以及提供了错误的解决方法的主题链接的根本原因。


|Repadmin 错误|根本原因|解决方案|
| --- | --- | --- |
|自上一次复制与此时间服务器超出的 tombstone 生存期。|域控制器命名的源的域控制器足够为删除了逻辑，复制，并从广告 DS 垃圾回收长的入站的复制失败。|事件 ID 2042：后太长复制这台计算机|
|没有站的邻居。|如果不显示的输出中生成的 repadmin"归巢的邻居"一节中的任何项 /showrepl，域控制器程序无法建立将链接复制与另一台域控制器。|修复复制连接问题 (事件 ID 1925)| 
|拒绝。|复制链接存在之间两个域控制器，但复制无法正确运行身份验证失败的结果。|修复复制安全问题| 
|上次尝试在 < 日期的时间 > 失败，"目标的帐户名称是错误。"|此问题可能与连接、DNS 或身份验证问题。 如果这是 DNS 错误，本地域控制器无法解析全球的唯一标识符 (GUID)-基于 DNS 其复制伙伴名称。|解决复制 DNS 查阅 (事件 Id 1925，2087，2088 年) 的问题解决复制安全问题解决复制连接问题 (事件 ID 1925)| 
|LDAP 错误 49。|域控制器计算机帐户可能不会同步密钥 Distribution 中心 (KDC)。|修复复制安全问题| 
|无法打开 LDAP 连接到本地主机|管理工具可能不会联系广告 DS。|解决复制 DNS 查阅问题 (事件 Id 1925，2087，2088 年)| 
|已抢先 Active Directory 复制。|较高优先级复制请求，如 repadmin /sync 命令手动生成的请求被中断的入站复制进度。|等待复制才能完成。 此信息性消息指示正常操作。| 
|复制发布，等待。| 域控制器发布复制请求和等待答案。 正在来源于此复制。|等待复制才能完成。 此信息性消息指示正常操作。| 

下表列出了常见事件可能指示存在问题的 Active Directory 复制，以及问题的主题提供的问题的解决方法的链接的根本原因。 

|事件 ID 和源|根本原因|解决方案|
| --- | --- | --- | 
|1311 NTDS KCC|复制配置信息广告 DS 中的不准确反映网络物理拓扑。|修复复制拓扑问题 (事件 ID 1311)| 
|1388 NTDS 复制|严格复制一致性不起作用，并且已复制到域控制器中延迟的对象。|修复复制逗留对象问题 (事件 Id 1388，于 1988 年由 2042 年)|
|1925 NTDS KCC|尝试建立复制链接可写的目录分区失败。 此事件产生不同的原因，具体取决于错误。| 解决复制连接问题 (事件 ID 1925) 修复复制 DNS 查阅问题 (事件 Id 1925，2087，2088 年)| 
|1988 NTDS 复制|本地域控制器已尝试复制从源域控制器，因为它可能已删除和已的垃圾回收不存在本地域控制器上的对象。 复制不会继续使用此合作伙伴此目录分区，直到解决此问题。|修复复制逗留对象问题 (事件 Id 1388，于 1988 年由 2042 年)|
|2042 NTDS 复制|复制的 tombstone 生存期，不发生与此合作伙伴，并且无法进行复制。|修复复制逗留对象问题 (事件 Id 1388，于 1988 年由 2042 年)| 
|2087 NTDS 复制|广告 DS 无法解析为 IP 地址，并复制失败源域控制器 DNS 主机名。|解决复制 DNS 查阅问题 (事件 Id 1925，2087，2088 年)| 
|2088 NTDS 复制 |广告 DS 无法解析为 IP 地址，但复制成功源域控制器 DNS 主机名称。|解决复制 DNS 查阅问题 (事件 Id 1925，2087，2088 年)|
|5805 网络登录|计算机帐户没有身份验证，这通常由计算机同名任一多个实例或不将其复制到每个域控制器的计算机名称。|修复复制安全问题| 

有关复制概念的详细信息，请参阅[Active Directory 复制技术](https://go.microsoft.com/fwlink/?LinkId=41950)。
  

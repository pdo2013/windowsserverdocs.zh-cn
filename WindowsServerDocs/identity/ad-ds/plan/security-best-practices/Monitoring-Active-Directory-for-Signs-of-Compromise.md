---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: 监视 Active Directory 遭到破坏的迹象
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882938"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>监视 Active Directory 遭到破坏的迹象

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

*定律 5 个数字：永久警戒是安全的价格。* - [十个永恒定律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
监视系统 solid 事件日志是任何安全的 Active Directory 设计的一个重要部分。 许多计算机安全破坏无法发现早期事件中如果受害者实施相应的事件日志监视和警报。 独立的报表一直都支持此结论。 例如， [2009 Verizon 数据违规报告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)状态：  
  
"事件监视和日志分析明显 ineffectiveness 继续让人感到有些 enigma。 检测的机会是控制。调查人员请注意 66%的受害者，必须在其日志以发现违反了它们已在分析此类资源更认真内可用的充足证据。"  
  
缺乏监视活动事件日志将保留在许多公司的安全防御计划的一致的漏洞。 [2012 Verizon 数据违规报告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)找到，即使 85%的违规花费了几周时间才能被注意到，受害者的 84%必须安全违规的证据，在其事件日志中。  
  
## <a name="windows-audit-policy"></a>Windows 审核策略

以下是 Microsoft 官方企业支持博客的链接。 这些博客的内容提供了建议和指南以及有关审核的建议将帮助您增强您的 Active Directory 基础结构的安全性和设计审核策略时是有价值的资源。  
  
* [全局对象访问审核是 Magic](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -介绍了一种称为高级审核策略配置已添加到 Windows 7 和 Windows Server 2008 R2，可让您设置您希望轻松地审核并不交替使用哪些类型的数据的控制机制脚本和 auditpol.exe。  
* [介绍 Windows 2008 中的审核更改](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-引入了 Windows Server 2008 中的审核的更改。  
* [太棒了 Vista 和 2008年中的审核技巧](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)-介绍了 Windows Vista 和 Windows Server 2008 的可用于故障排除问题或查看您的环境中发生的有趣审核功能。  
* [针对 Windows Server 2008 和 Windows Vista 中的审核的一站式](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含编译的审核功能和 Windows Server 2008 和 Windows Vista 中包含的信息。  
  
以下链接提供有关对 Windows 审核 Windows 8 和 Windows Server 2012 中的改进的信息和有关 AD DS 的信息，Windows Server 2008 中的审核。  
  
* [What's New in 安全审核](https://technet.microsoft.com/library/hh849638.aspx)-提供新的安全审核 Windows 8 和 Windows Server 2012 中的功能的概述。  
* [AD DS 审核分步指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-介绍 Windows Server 2008 中新的 Active Directory 域服务 (AD DS) 审核功能。 它还提供实施这一新功能的步骤。  
  
### <a name="windows-audit-categories"></a>Windows 审核类别

在 Windows Vista 和 Windows Server 2008 中之前, Windows 必须仅有九个事件日志审核策略类别：  
  
* 帐户登录事件  
* 帐户管理  
* 目录服务访问  
* 登录事件  
* 对象访问  
* 策略更改  
* 权限使用  
* 过程跟踪  
* 系统事件  
  
这些九个传统审核类别包含审核策略。 可以为成功、 失败或成功和失败启用每个审核策略类别的事件。 下一节中提供其说明。  
  
#### <a name="audit-policy-category-descriptions"></a>审核策略类别说明  
审核策略类别启用以下事件日志消息类型。  
  
##### <a name="audit-account-logon-events"></a>审核帐户登录事件  
报告的安全主体 （例如，用户、 计算机或服务帐户） 登录到或从使用另一台计算机来验证的帐户的一台计算机日志记录的每个实例。 域安全主体帐户进行身份验证的域控制器上时，会生成帐户登录事件。 本地计算机上的本地用户进行身份验证生成的本地安全日志中记录的登录事件。 不记录任何帐户注销事件。  
  
此类别在正常业务过程生成大量的"噪声"因为 Windows 不断地拥有帐户登录到和从本地和远程计算机。 尽管如此，任何安全计划应包括的成功和失败的此审核类别。  
  
##### <a name="audit-account-management"></a>审核帐户管理  
此审核设置确定是否要跟踪的用户和组管理。 例如，用户和组应跟踪时创建、 更改或删除，则用户或计算机帐户、 安全组或通讯组当重命名、 禁用或启用，则用户或计算机帐户或者，当发生更改的用户或计算机的密码。 可以为用户或组添加到或从其他组中删除生成一个事件。  
  
##### <a name="audit-directory-service-access"></a>审核目录服务访问  

此策略设置确定是否要审核安全主体的访问权限的 Active Directory 对象的具有其自己指定的系统访问控制列表 (SACL)。 一般情况下，该类别仅应在域控制器上启用。 此设置启用时，会生成大量"噪声"。  
  
##### <a name="audit-logon-events"></a>审核登录事件  
本地安全主体进行身份验证本地计算机上时，会生成登录事件。 登录事件记录发生在本地计算机的域登录。 不生成帐户注销事件。 登录事件启用时，会生成大量"干扰"，但它们应默认情况下启用，任何安全审核计划中。  
  
##### <a name="audit-object-access"></a>审核对象访问  
对象访问可以生成事件 （适用于示例中，打开、 读取、 重命名、 已删除或已关闭） 访问启用了审核的随后定义的对象时。 启用主审核类别后，管理员必须单独定义哪些对象将审核已启用。 启用了审核，以便启用此类别将通常开始生成事件之前管理员定义任何来自多个 Windows 系统对象。  
  
此类别是非常"干扰"，将生成的每个对象访问的五到十个事件。 它可能很困难的新对象审核管理员以获得有用的信息。 其值仅应在需要时启用。  
  
##### <a name="auditing-policy-change"></a>审核策略更改  
此策略设置确定是否要审核对用户权限分配策略、 Windows 防火墙策略、 信任策略或审核策略更改的更改的每个事件。 应在所有计算机上启用此类别。 它会生成很少的噪音。  
  
##### <a name="audit-privilege-use"></a>审核特权使用  

有许多用户权限和 Windows （例如，作为批处理作业，并作为操作系统的一部分的登录） 中的权限。 此策略设置确定是否通过执行用户权限或特权审核安全主体的每个实例。 允许此类别会导致大量"干扰，"但它可能会有助于跟踪安全主体帐户使用提升的权限。  
  
##### <a name="audit-process-tracking"></a>审核进程跟踪  
此策略设置确定是否要审核的事件，例如程序激活、 进程退出、 句柄复制和间接对象访问跟踪信息的详细的过程。 它可用于跟踪恶意用户以及他们使用的程序。  
  
启用审核进程跟踪通常被设为生成大量事件，**无审核**。 但是，此设置可以提供很大的收益期间从启动的进程的详细日志事件响应和未启动的时间。 对于域控制器和其他单角色基础结构服务器，此类别可以安全地打开的时间。 单个角色的服务器不会生成多进程履行职责正常期间跟踪流量。 在这种情况下，它们可以启用捕获未经授权的事件，如果它们发生。  
  
##### <a name="system-events-audit"></a>系统事件审核  

系统事件是几乎一个通用的全能类别，注册会影响计算机、 其系统安全性或在安全日志的各种事件。 它包括了事件的计算机关机和重新启动、 电源故障、 系统时间更改、 身份验证包的初始化、 审核日志 clearings、 模拟问题和其他常规事件的主机。 一般情况下，启用此审核类别会生成大量"干扰"，但它会生成很难曾建议不启用它足够非常有用的事件。  
  
#### <a name="advanced-audit-policies"></a>高级的审核策略

从 Windows Vista 和 Windows Server 2008 开始，Microsoft 将改进可以通过创建每个主审核类别下的子类别进行事件日志类别选择的方式。 子类别允许审核要比此外可以使用的主要类别的精细程度。 通过使用子类别，可以启用部分的特定主类别，并跳过对其具有无需使用的生成事件。 可以针对“成功”、“失败”或“成功和失败”事件启用每种审核策略子类别。  
  
若要列出所有可用审核子类别，查看组策略对象中的高级审核策略容器，或在运行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 中，Windows 8 的任何计算机上的命令提示符下键入以下内容Windows 7 或 Windows Vista:  
  
`auditpol /list /subcategory:\*`
  
若要获取运行 Windows Server 2012、 Windows Server 2008 R2 或 Windows 2008 的计算机上的当前配置的审核子类别列表，请键入以下命令：  
  
`auditpol /get /category:\*`
  
以下屏幕截图显示 auditpol.exe 列出当前审核策略的示例。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 而 auditpol.exe，组策略不会始终准确地报告所有已启用的审核策略的状态。 请参阅[获取 Windows 7 和 2008 R2 中的有效审核策略](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)的更多详细信息。  
  
每个主要类别都有多个子类别。 下面是一系列类别、 及其子类别和及其功能的说明。  
  
### <a name="auditing-subcategories-descriptions"></a>审核子类别说明  
审核策略子类别启用以下事件日志消息类型：  
  
#### <a name="account-logon"></a>帐户登录  
  
##### <a name="credential-validation"></a>凭据验证  
此子类别上提交以供用户帐户登录请求的凭据报告验证测试的结果。 这些事件是权威的凭据在计算机上发生。 对于域帐户，域控制器是权威，而对于本地帐户，请在本地计算机是权威。  
  
在域环境中，大多数帐户登录事件记录具有权威的域帐户的域控制器的安全日志中。 但是，这些事件可能发生在组织中其他计算机上时使用本地帐户登录。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服务票证操作  
此子类别报告是域帐户的授权的域控制器上的 Kerberos 票证请求进程生成的事件。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 身份验证服务  
此子类别报告生成 Kerberos 身份验证服务的事件。 这些事件是权威的凭据在计算机上发生。  
  
##### <a name="other-account-logon-events"></a>其他帐户登录事件  
此子类别报告在与凭据验证或 Kerberos 票证不相关的凭据的用户帐户登录请求提交到响应中发生的事件。 这些事件是权威的凭据在计算机上发生。 对于域帐户，域控制器是权威，而对于本地帐户，请在本地计算机是权威。  
  
在域环境中，大多数帐户登录事件记录具有权威的域帐户的域控制器的安全日志中。 但是，这些事件可能发生在组织中其他计算机上时使用本地帐户登录。 示例可以包括：  
  
* 远程桌面服务会话断开连接  
* 新的远程桌面服务会话  
* 锁定和解锁工作站  
* 调用一个屏幕保护程序  
* 关闭屏幕保护程序  
* 将 Kerberos 检测重播的攻击，在其中两次收到具有相同信息的 Kerberos 请求  
* 到无线网络的用户或计算机帐户授予访问权限  
* 访问有线 802.1 x 网络授予用户或计算机帐户  
  
#### <a name="account-management"></a>帐户管理  
  
##### <a name="user-account-management"></a>用户帐户管理  
此子类别报告用户帐户管理，例如用户帐户是创建、 更改或删除，则每个的事件重命名、 禁用或启用，则用户帐户或设定或更改密码。 如果启用此审核策略设置，管理员可以跟踪事件以检测恶意、 意外，和授权的用户帐户的创建。  
  
##### <a name="computer-account-management"></a>计算机帐户管理  
此子类别报告计算机帐户管理，例如当计算机帐户是创建、 更改、 删除、 重命名、 禁用或启用每个的事件。  
  
##### <a name="security-group-management"></a>安全组管理  
此子类别报告安全组管理，例如当创建、 更改或删除安全组或成员添加到或从安全组中删除时每个的事件。 如果启用此审核策略设置，管理员可以跟踪事件以检测恶意、 意外，和授权的安全组帐户的创建。  
  
##### <a name="distribution-group-management"></a>分发组管理  
此子类别报告通讯组管理，例如当创建、 更改或删除通讯组或成员添加到或从分发组中删除时的每个事件。 如果启用此审核策略设置，管理员可以跟踪事件以检测恶意和意外，授权创建的组帐户。  
  
##### <a name="application-group-management"></a>应用程序组管理  
此子类别的计算机上，例如当创建、 更改或删除应用程序组或成员添加到或从应用程序组中删除时报告应用程序组管理每个的事件。 如果启用此审核策略设置，管理员可以跟踪事件以检测恶意、 意外，和授权的应用程序组帐户的创建。  
  
##### <a name="other-account-management-events"></a>其他帐户管理事件  
此子类别报告其他帐户管理事件。  
  
#### <a name="detailed-process-tracking"></a>详细的过程跟踪  
  
##### <a name="process-creation"></a>进程创建  
此子类别报告创建进程并创建它的程序的用户的名称。  
  
##### <a name="process-termination"></a>进程终止  
在进程终止时，将报告此子类别。  
  
##### <a name="dpapi-activity"></a>DPAPI 活动  
此子类别报告加密或解密调入数据保护应用程序编程接口 (DPAPI)。 使用 DPAPI 来保护机密信息，例如存储密码和密钥信息。  
  
##### <a name="rpc-events"></a>RPC 事件  
此子类别报告远程过程调用 (RPC) 连接事件。  
  
#### <a name="directory-service-access"></a>目录服务访问  
  
##### <a name="directory-service-access"></a>目录服务访问  
访问 AD DS 对象时，将报告此子类别。 唯一对象使用 Sacl 配置会导致审核事件，以进行生成，并且只匹配 SACL 条目的方式进行访问时。 这些事件是类似于早期版本的 Windows Server 中的目录服务访问事件。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-changes"></a>目录服务更改  
此子类别报告到 AD DS 中的对象的更改。 报告的更改的类型是创建、 修改、 移动和撤消删除对象执行的操作。 目录服务更改审核，根据需要，指示已更改的对象已更改属性的新旧值。 仅限使用 Sacl 的对象会导致审核事件，以进行生成，并且只与其 SACL 条目匹配的方式进行访问时。 某些对象和属性不会导致审核事件，以生成由于架构中的对象类上的设置。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-replication"></a>目录服务复制  
当两个域控制器之间的复制开始和结束时，将报告此子类别。  
  
##### <a name="detailed-directory-service-replication"></a>详细的目录服务复制  
此子类别报告有关域控制器之间复制的信息的详细的信息。 这些事件可能会非常高卷中。  
  
#### <a name="logonlogoff"></a>登录/注销  
  
##### <a name="logon"></a>登录  
当用户尝试登录到系统时，将报告此子类别。 访问计算机上发生这些事件。 交互式登录时，生成的这些事件发生在登录到计算机上。 如果网络登录进行访问的共享，这些事件生成托管访问的资源的计算机上。 如果此设置配置为**任何审核**，很难或无法确定哪些用户具有访问或者尝试访问组织的计算机。  
  
##### <a name="network-policy-server"></a>网络策略服务器  
此子类别报告生成 RADIUS (IAS) 和网络访问保护 (NAP) 用户访问请求的事件。 这些请求可以是**Grant**，**拒绝**，**放弃**，**隔离**，**锁**，和**解锁**。 审核此设置将导致中等或高的 NPS 和 IAS 服务器上的记录量。  
  
##### <a name="ipsec-main-mode"></a>IPsec 主模式  
此子类别在主模式协商期间报告的 Internet 密钥交换 (IKE) 协议和身份验证 Internet 协议 (AuthIP) 的结果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 扩展模式  
此子类别报告在扩展模式协商过程的 AuthIP 的结果。  
  
##### <a name="other-logonlogoff-events"></a>其他登录/注销事件  
此子类别报告其他登录和注销相关的事件，如远程桌面服务会话断开连接，并重新连接，使用运行方式运行在不同帐户下的进程和锁定和解锁工作站。  
  
##### <a name="logoff"></a>在用户界面中，  
当用户从系统注销时，将报告该子类别。 访问计算机上发生这些事件。 交互式登录时，生成的这些事件发生在登录到计算机上。 如果网络登录进行访问的共享，这些事件生成托管访问的资源的计算机上。 如果此设置配置为**任何审核**，很难或无法确定哪些用户具有访问或者尝试访问组织的计算机。  
  
##### <a name="account-lockout"></a>帐户锁定  
用户的帐户因失败的登录尝试次数过多而被锁定时，将报告此子类别。  
  
##### <a name="ipsec-quick-mode"></a>IPsec 快速模式  
此子类别报告在快速模式协商过程 IKE 协议和 AuthIP 的结果。  
  
##### <a name="special-logon"></a>特殊登录  
使用特殊登录时，将报告此子类别。 特殊的登录名是具有管理员等效权限，可用于提升到更高级别进程的登录帐户。  
  
#### <a name="policy-change"></a>策略更改  
  
##### <a name="audit-policy-change"></a>审核策略更改  
此子类别报告中包括 SACL 的更改的审核策略的更改。  
  
##### <a name="authentication-policy-change"></a>身份验证策略更改  
此子类别报告在身份验证策略中的更改。  
  
##### <a name="authorization-policy-change"></a>授权策略更改  
此子类别报告中包括的权限 (DACL) 的更改的授权策略的更改。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>MPSSVC 规则级别策略更改  
此子类别报告在 Microsoft Protection Service (MPSSVC.exe) 所使用的策略规则中的更改。 通过 Windows 防火墙使用此服务。  
  
##### <a name="filtering-platform-policy-change"></a>筛选平台策略更改  
此子类别报告添加和删除 WFP，包括启动筛选器中的对象。 这些事件可能会非常高卷中。  
  
##### <a name="other-policy-change-events"></a>其他策略更改事件  
此子类别报告其他类型的安全策略更改，如受信任的平台模块 (TPM) 或加密提供程序的配置。  
  
#### <a name="privilege-use"></a>权限使用  
  
##### <a name="sensitive-privilege-use"></a>敏感权限使用  
此子类别报告时的用户帐户或服务使用敏感权限。 敏感权限包括以下用户权限： 充当操作系统的一部分，备份文件和目录、 创建令牌对象、 调试程序，允许计算机和用户帐户是受信任可以进行委派，生成安全审核身份验证后模拟客户端、 加载和卸载设备驱动程序、 管理审核和安全日志中，修改固件环境值，替换进程级别标记、 还原文件和目录，和取得文件或其他对象的所有权。 审核此子类别会创建大量的事件。  
  
##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 权限使用  
此子类别报告时的用户帐户或服务使用 nonsensitive 特权。 Nonsensitive 权限包括以下用户权限： 访问作为受信任调用方的凭据管理器，从网络访问这台计算机，将工作站添加到域、 调整进程的内存配额，允许本地登录，允许通过远程登录桌面服务，绕过遍历检查，更改系统时间、 创建页面文件、 创建全局对象、 创建永久共享的对象、 创建符号链接、 拒绝访问此计算机可以通过网络、 拒绝作为批处理作业登录、 拒绝作为服务登录拒绝本地登录、 拒绝通过远程桌面服务登录、 从远程系统强制关机、 增加进程工作集，提高计划优先级，锁定内存页、 作为批处理作业登录、 作为服务登录、 修改对象标签，执行卷维护任务，一个进程中配置文件，分析系统性能、 从扩展坞中删除计算机、 关闭系统，和同步目录服务数据。 审核此子类别将创建事件的量非常大。  
  
##### <a name="other-privilege-use-events"></a>其他权限使用事件  
当前未使用此安全策略设置。  
  
#### <a name="object-access"></a>对象访问  
  
##### <a name="file-system"></a>文件系统  
当访问文件系统对象时，将报告此子类别。 仅使用 Sacl 的文件系统对象会导致审核事件，以进行生成，而且只能才能以匹配其 SACL 项的方式进行访问时。 其本身而言，此策略设置不会导致任何事件的审核。 它将确定是否要审核的事件的访问具有指定的系统访问控制列表 (SACL) 的文件系统对象的用户有效地启用进行审核。  
  
如果审核对象访问设置配置为**成功**，一个审核项生成每次用户成功访问指定 SACL 的对象。 如果此策略设置配置为**失败**，用户在失败尝试访问具有指定 SACL 的对象中每次生成一个审核项。  
  
##### <a name="registry"></a>注册表  
当访问注册表对象时，将报告此子类别。 仅注册表对象使用 Sacl 会导致审核事件，以进行生成，而且只能才能以匹配其 SACL 项的方式进行访问时。 其本身而言，此策略设置不会导致任何事件的审核。  
  
##### <a name="kernel-object"></a>内核对象  
访问内核对象，如进程和互斥体时，将报告此子类别。 仅内核对象使用 Sacl 会导致审核事件，以进行生成，而且只能才能以匹配其 SACL 项的方式进行访问时。 通常内核对象仅指定 Sacl 如果 AuditBaseObjects 或 AuditBaseDirectories 审核选项处于启用状态。  
  
##### <a name="sam"></a>SAM  
访问本地安全帐户管理器 (SAM) 身份验证数据库对象时，将报告此子类别。  
  
##### <a name="certification-services"></a>证书服务  
执行证书服务操作时，将报告此子类别。  
  
##### <a name="application-generated"></a>生成应用程序  
当应用程序尝试使用 Windows 审核应用程序编程接口 (Api) 生成审核事件时，将报告此子类别。  
  
##### <a name="handle-manipulation"></a>句柄操作  
对象的句柄打开或关闭时，将报告此子类别。 仅限使用 Sacl 的对象会导致这些事件，以进行生成，且仅当尝试句柄操作与 SACL 条目相匹配。 对于对象类型 （如文件系统或注册表） 中启用相应的对象访问子类别，则只生成句柄操作事件。  
  
##### <a name="file-share"></a>文件共享  
当访问文件共享时，将报告此子类别。 其本身而言，此策略设置不会导致任何事件的审核。 它将确定是否要审核的事件的访问具有指定的系统访问控制列表 (SACL) 的文件共享对象的用户有效地启用进行审核。  
  
##### <a name="filtering-platform-packet-drop"></a>筛选平台数据包丢弃  
通过 Windows 筛选平台 (WFP) 会丢弃数据包时，将报告此子类别。 这些事件可能会非常高卷中。  
  
##### <a name="filtering-platform-connection"></a>筛选平台连接  
允许或阻止的 WFP 连接时，将报告此子类别。 这些事件可以是高卷中。  
  
##### <a name="other-object-access-events"></a>其他对象访问事件  
此子类别报告任务计划程序作业和 COM + 对象等其他对象访问相关事件。  
  
#### <a name="system"></a>系统  
  
##### <a name="security-state-change"></a>安全状态更改  
此子类别中的系统，如安全子系统启动和停止时的安全状态报告的更改。  
  
##### <a name="security-system-extension"></a>安全系统扩展  
此子类别会通过安全子系统报告扩展插件代码，例如身份验证包的加载。  
  
##### <a name="system-integrity"></a>系统完整性  
此子类别报表的安全子系统的完整性的冲突。  
  
IPsec 驱动程序  
  
此子类别报告在 Internet 协议安全 (IPsec) 驱动程序的活动上。  
  
##### <a name="other-system-events"></a>其他系统事件  
此子类别报告在其他系统事件。  
  
有关子类别说明的详细信息，请参阅[Microsoft Security Compliance Manager 工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每个组织应查看的已覆盖的类别和子类别，并启用，最适合其环境。 始终应在生产环境中部署之前测试更改审核策略。  
  
## <a name="configuring-windows-audit-policy"></a>配置 Windows 审核策略

可以使用组策略、 auditpol.exe、 Api 或注册表编辑设置 Windows 审核策略。 对于大多数公司配置审核策略的推荐的方法是组策略或 auditpol.exe。 设置系统审核策略要求管理员级帐户权限或适当的委派的权限。  
  
> [!NOTE]  
> **管理审核和安全日志**权限必须授予的安全主体 （管理员具有其默认情况下） 允许修改对象访问审核的各个资源，例如文件处于活动状态的选项目录对象和注册表项。  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>通过使用组策略设置 Windows 审核策略

若要将审核策略使用组策略设置，配置下的相应的审核类别**计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 审核策略**(请参阅以下屏幕截图示例从本地组策略编辑器 (gpedit.msc) 中）。 可以为启用每个审核策略类别**成功**，**失败**，或**成功**和故障事件。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
可以通过使用 Active Directory 或本地组策略设置高级的审核策略。 若要设置高级审核策略，配置下的相应子类别**计算机配置 \windows 设置 \ 安全设置 \ 高级审核策略**（请参阅有关从本地示例下面的屏幕截图组策略编辑器 (gpedit.msc)）。 可以为启用每个审核策略子类别**成功**，**失败**，或**成功**并**失败**事件。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 Auditpol.exe 设置 Windows 审核策略

在 Windows Server 2008 和 Windows Vista 中引入 Auditpol.exe （用于设置 Windows 审核策略）。 最初，仅 auditpol.exe 无法用于设置高级审核策略，但可以在 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008、 Windows 8 和 Windows 7 中使用组策略。  
  
Auditpol.exe 是一个命令行实用程序。 语法是按如下所示：  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Auditpol.exe 语法示例：  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe 本地设置高级审核策略。 如果本地策略与 Active Directory 或本地组策略冲突，组策略设置将通常优先于 auditpol.exe 设置。 当存在多个组或本地策略冲突时，只能有一个策略将继续执行 （即，将为）。 审核策略不会合并。  
  
#### <a name="scripting-auditpol"></a>脚本 Auditpol

Microsoft 提供了[的示例脚本](https://support.microsoft.com/kb/921469)的管理员想要将高级审核策略设置通过使用脚本而无需手动键入每个 auditpol.exe 命令。  
  
**请注意**组策略不会始终准确地报告所有已启用的审核策略的状态而 auditpol.exe。 请参阅[获取 Windows 7 和 Windows 2008 R2 中的有效审核策略](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)的更多详细信息。  
  
#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令

可以使用 Auditpol.exe，保存和还原的本地审核策略，并查看其他审核的相关的命令。 下面是另**auditpol**命令。  
  
`auditpol /clear` -用来清除和重置本地审核策略  
  
`auditpol /backup /file:<filename>` -用于为二进制文件备份当前的本地审核策略  
  
`auditpol /restore /file:<filename>` -用于以前保存的审核策略文件导入到本地审核策略  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -如果启用此审核策略设置，则会导致系统立即停止 (停止：C0000244 {审核失败} 消息） 出于任何原因不能记录如果安全审核。 通常情况下，事件失败时的安全审核日志已满并且指定的安全日志的保留方法是记录**不覆盖事件**或**按天数改写事件**。 通常它是只有通过启用需要安全日志记录的更高版本保证的环境。 如果启用，管理员必须密切关注安全日志大小，并循环所需的日志。 它也可以设置与组策略通过修改安全选项**审核：如果无法记录安全审核则立即关闭系统**(默认值 = 已禁用)。  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -此审核策略设置确定是否要审核对全局系统对象的访问。 如果启用此策略，则会导致系统对象，例如互斥锁、 事件、 信号量和 DOS 设备便可创建通过默认的系统访问控制列表 (SACL)。 大多数管理员考虑审核将太"干扰"，全局系统对象和它们只会启用它，如果怀疑恶意黑客。 命名的对象仅指定了 SACL。 如果还启用审核对象访问审核策略 （或内核对象的审核子类别），对这些系统对象的访问进行审核。 在配置此安全设置时，更改才会生效，直到重新启动 Windows。 此策略也可以设置与组策略通过修改全局系统对象的安全选项审核访问权限 (默认值 = 已禁用)。  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -此审核策略设置指定要在创建时指定 Sacl 命名的内核对象 （如互斥锁和信号量）。 AuditBaseDirectories 会影响容器对象，而 AuditBaseObjects 影响不能包含其他对象的对象。  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -此审核策略设置指定是否在客户端生成一个事件时一个或多个这些权限分配给用户安全令牌：AssignPrimaryTokenPrivilege、 AuditPrivilege、 BackupPrivilege、 CreateTokenPrivilege、 DebugPrivilege、 EnableDelegationPrivilege、 ImpersonatePrivilege、 LoadDriverPrivilege、 RestorePrivilege、 SecurityPrivilege、 SystemEnvironmentPrivilege，TakeOwnershipPrivilege 和 TcbPrivilege。 如果未启用此选项 (默认值 = 已禁用)，则不记录 BackupPrivilege 和 RestorePrivilege 特权。 启用此选项可以使备份操作期间的安全日志极其干扰性 （有时数百个第二个事件）。 此策略也可以设置与组策略通过修改安全选项**审核：审核备份和还原权限的使用**。  
  
> [!NOTE]  
> 此处提供的一些信息摘自 Microsoft[审核选项类型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 Microsoft SCM 工具。  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>强制实施传统审核或高级审核

在 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows 8、 Windows 7 和 Windows Vista 中，管理员可以选择启用九个传统类别或使用子类别。 它是一个二进制的选择，必须在每个 Windows 系统中进行。 可以启用的主要类别，或者 subcategoriesit 不能既是。  
  
若要防止旧的传统类别策略覆盖审核策略子类别，必须启用**强制审核策略子类别设置 (Windows Vista 或更高版本) 替代审核策略类别设置**策略设置位于**计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 安全选项**。  
  
我们建议启用子类别和配置而不是九个主要类别。 这要求组策略设置启用 （以便留出子类别来重写审核类别） 以及配置支持审核策略的不同子类别。  
  
可以通过使用多个方法，包括组策略的命令行程序、 auditpol.exe 配置审核子类别。  
  
## <a name="next-steps"></a>后续步骤
  
* [高级的安全审核在 Windows 7 和 Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [审核和 Windows Server 2008 中的符合性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [如何使用组策略配置详细的审核设置的基于 Windows Vista 和 Windows Server 2008 的计算机在 Windows Server 2008 域中、 在 Windows Server 2003 域中或在 Windows 2000 域中的安全](https://support.microsoft.com/kb/921469)  
  
* [高级安全审核策略循序渐进指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  

---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: 监视 Active Directory 遭到破坏的迹象
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ba67a5fcc127bbe6ffce9454ff98fd3bc3725e55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367709"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>监视 Active Directory 遭到破坏的迹象

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*Law Number 5：永久警惕是 @no__t 的价格。[安全管理的 @no__t 0 10 永恒定律](https://technet.microsoft.com/library/cc722488.aspx)  
  
固态事件日志监视系统是任何安全 Active Directory 设计的重要组成部分。 如果受害者制订适当的事件日志监视和警报，则很多计算机安全威胁可能在事件初期发现。 此结论支持独立报表。 例如， [2009 Verizon 数据违规报告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)状态：  
  
"事件监视和日志分析的明显 ineffectiveness 仍是一个 enigma。 检测的机会有;调查人员指出，66% 的受害者在其日志中有足够的证据来发现违规行为是否更用心分析此类资源。 "  
  
缺乏监视活动的事件日志在许多公司的安全防御计划中仍然是一种一致的缺点。 [2012 Verizon 数据违规报告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)发现，即使违反了 85% 的行为，也需要几周的时间才能注意到84，因为受害者的攻击百分比在其事件日志中有违反的证据。  
  
## <a name="windows-audit-policy"></a>Windows 审核策略

下面是 Microsoft 官方企业支持博客的链接。 这些博客的内容提供有关审核的建议、指导和建议，这些建议可帮助你增强 Active Directory 基础结构的安全性，并且在设计审核策略时是有价值的资源。  
  
* [全局对象访问审核非常神奇](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx)-介绍一种称为 "高级审核策略配置" 的控制机制，该机制已添加到 windows 7 和 windows Server 2008 R2，使你可以设置要轻松审核的数据类型，而不是调整的脚本和auditpol。  
* [Windows 2008 中的审核更改简介](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-介绍了在 windows Server 2008 中进行的审核更改。  
* [Vista 和2008中的冷审核技巧](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)-介绍了 windows Vista 和 windows Server 2008 的有趣审核功能，这些功能可用于排查问题或查看环境中发生的情况。  
* [Windows server 2008 和 Windows vista 中的一站式审核](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含对 windows server 2008 和 windows vista 中包含的审核功能和信息的编译。  
  
以下链接提供了有关 windows 8 和 Windows Server 2012 中 Windows 审核改进的信息，以及有关 Windows Server 2008 中 AD DS 审核的信息。  
  
* [安全审核中的新增](https://technet.microsoft.com/library/hh849638.aspx)功能-概述了 windows 8 和 windows Server 2012 中的新安全审核功能。  
* [AD DS 审核循序渐进指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-介绍了 Windows Server 2008 中的新 Active Directory 域服务（AD DS）审核功能。 它还提供了实现此新功能的过程。  
  
### <a name="windows-audit-categories"></a>Windows 审核类别

在 Windows Vista 和 Windows Server 2008 之前，Windows 只有9个事件日志审核策略类别：  
  
* 帐户登录事件  
* 帐户管理  
* 目录服务访问  
* 登录事件  
* 对象访问  
* 策略更改  
* 权限使用  
* 进程跟踪  
* 系统事件  
  
这九个传统审核类别构成了审核策略。 可以为成功、失败或成功和失败事件启用每个审核策略类别。 下一节中提供了这些说明。  
  
#### <a name="audit-policy-category-descriptions"></a>审核策略类别说明  
审核策略类别启用以下事件日志消息类型。  
  
##### <a name="audit-account-logon-events"></a>审核帐户登录事件  
报告每个安全主体（例如用户、计算机或服务帐户）的每个实例，这些实例在一台计算机上登录或注销，而另一台计算机用于验证帐户。 帐户登录事件是在域控制器上对域安全主体帐户进行身份验证时生成的。 本地计算机上本地用户的身份验证将生成一个登录事件，该事件记录在本地安全日志中。 不记录任何帐户注销事件。  
  
由于 Windows 在正常业务过程中一直在本地和远程计算机上登录和注销，因此此类别会生成许多 "干扰"。 尽管如此，任何安全计划都应包括此审核类别的成功和失败。  
  
##### <a name="audit-account-management"></a>审核帐户管理  
此审核设置确定是否跟踪用户和组的管理。 例如，在创建、更改或删除用户或计算机帐户、安全组或通讯组时，应跟踪用户和组;如果重命名、禁用或启用用户帐户或计算机帐户，则为;或更改了用户或计算机密码。 可以为在其他组中添加或删除的用户或组生成事件。  
  
##### <a name="audit-directory-service-access"></a>审核目录服务访问  

此策略设置确定是否审核对具有自己的指定系统访问控制列表（SACL）的 Active Directory 对象的安全主体的访问权限。 通常，只应在域控制器上启用此类别。 启用后，此设置会生成许多 "干扰"。  
  
##### <a name="audit-logon-events"></a>审核登录事件  
在本地计算机上对本地安全主体进行身份验证时，将生成登录事件。 登录事件记录在本地计算机上发生的域登录。 不会生成帐户注销事件。 启用后，登录事件会生成很多 "噪音"，但在任何安全审核计划中，它们应默认启用。  
  
##### <a name="audit-object-access"></a>审核对象访问  
在访问启用了审核功能的后续定义的对象（例如，已打开、读取、重命名、删除或关闭）时，对象访问可生成事件。 启用主审核类别之后，管理员必须单独定义哪些对象将启用审核。 许多 Windows 系统对象都启用了审核功能，因此，启用此类别通常会在管理员定义任何事件之前开始生成事件。  
  
此类别非常 "干扰"，将为每个对象访问生成五到十个事件。 对象审核的新管理员难以获取有用的信息。 只应在需要时启用它。  
  
##### <a name="auditing-policy-change"></a>审核策略更改  
此策略设置确定是否审核对用户权限分配策略、Windows 防火墙策略、信任策略或审核策略更改的每个更改。 应在所有计算机上启用此类别。 它生成的噪音非常小。  
  
##### <a name="audit-privilege-use"></a>审核权限使用  

Windows 中有数十种用户权限（例如，作为批处理作业登录并作为操作系统的一部分）。 此策略设置确定是否通过行使用户权限或特权来审核安全主体的每个实例。 启用此类别将导致很多 "噪音"，但在使用提升的权限跟踪安全主体帐户时可能会有帮助。  
  
##### <a name="audit-process-tracking"></a>审核过程跟踪  
此策略设置确定是否审核详细的进程跟踪信息，如程序激活、进程退出、处理重复和间接对象访问等。 这对于跟踪恶意用户及其使用的程序很有用。  
  
启用审核过程跟踪会生成大量事件，因此通常会将其设置为 "**无审核**"。 但是，在事件响应期间，此设置可以从已启动进程的详细日志和启动时间的详细日志中提供极大的好处。 对于域控制器和其他单一角色基础结构服务器，可以随时安全地启用此类别。 单个角色服务器在其职责的正常过程中不会生成大量的进程跟踪流量。 这样，就可以在发生未经授权的事件时捕获它们。  
  
##### <a name="system-events-audit"></a>系统事件审核  

系统事件几乎是一般的全部捕获类别，用于注册影响计算机、其系统安全或安全日志的各种事件。 其中包括用于计算机关闭和重启的事件、电源故障、系统时间更改、身份验证包初始化、审核日志 clearings、模拟问题和其他一般事件的主机。 通常，启用此审核类别会生成很多 "噪音"，但会生成足够多的事件，这很难建议不要启用。  
  
#### <a name="advanced-audit-policies"></a>高级审核策略

从 Windows Vista 和 Windows Server 2008 开始，Microsoft 通过在每个主审核类别下创建子类别，改进了事件日志类别选择的方式。 子类别允许审核比使用主要类别更精细。 通过使用子类别，可以仅启用特定主类别的部分，并跳过生成不使用的事件。 可以针对“成功”、“失败”或“成功和失败”事件启用每种审核策略子类别。  
  
若要列出所有可用的审核子类别，请在组策略对象中查看高级审核策略容器，或在运行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008，Windows 8 的任何计算机上的命令提示符处键入以下命令：Windows 7 或 Windows Vista：  
  
`auditpol /list /subcategory:*`
  
若要获取运行 Windows Server 2012、Windows Server 2008 R2 或 Windows 2008 的计算机上当前配置的审核子类别的列表，请键入以下内容：  
  
`auditpol /get /category:*`
  
以下屏幕截图显示了列出当前审核策略的 svchost.exe 的示例。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 组策略不会始终准确报告所有启用的审核策略的状态，而 svchost.exe 则执行。 有关更多详细信息，请参阅[在 Windows 7 和 2008 R2 中获取有效的审核策略](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)。  
  
每个主类别都有多个子类别。 下面是类别、子类别和其功能的说明的列表。  
  
### <a name="auditing-subcategories-descriptions"></a>审核子类别说明  
审核策略子类别启用以下事件日志消息类型：  
  
#### <a name="account-logon"></a>帐户登录  
  
##### <a name="credential-validation"></a>凭据验证  
此子类别报告针对用户帐户登录请求提交的凭据进行验证测试的结果。 这些事件发生在对凭据具有权威的计算机上。 对于域帐户，域控制器是权威性的，而对于本地帐户，本地计算机是权威的。  
  
在域环境中，大多数帐户登录事件都记录在域帐户的权威域控制器的安全日志中。 但是，当本地帐户用于登录时，这些事件可能发生在组织中的其他计算机上。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服务票证操作  
此子类别报告域控制器上的 Kerberos 票证请求进程生成的事件，这些事件对于域帐户是权威的。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 身份验证服务  
此子类别报告由 Kerberos 身份验证服务生成的事件。 这些事件发生在对凭据具有权威的计算机上。  
  
##### <a name="other-account-logon-events"></a>其他帐户登录事件  
此子类别将报告为响应提交给与凭据验证或 Kerberos 票证无关的用户帐户登录请求凭据时发生的事件。 这些事件发生在对凭据具有权威的计算机上。 对于域帐户，域控制器是权威性的，而对于本地帐户，本地计算机是权威的。  
  
在域环境中，大多数帐户登录事件都记录在域帐户的权威域控制器的安全日志中。 但是，当本地帐户用于登录时，这些事件可能发生在组织中的其他计算机上。 示例可以包括以下内容：  
  
* 远程桌面服务会话断开  
* 新远程桌面服务会话  
* 锁定和解锁工作站  
* 调用屏幕保护程序  
* 关闭屏幕保护程序  
* 检测到 Kerberos 重播攻击，其中接收到具有相同信息的 Kerberos 请求两次  
* 访问授予用户或计算机帐户的无线网络  
* 访问授予用户或计算机帐户的有线 802.1 x 网络  
  
#### <a name="account-management"></a>帐户管理  
  
##### <a name="user-account-management"></a>用户帐户管理  
此子类别报告用户帐户管理的每个事件，例如，在创建、更改或删除用户帐户时;用户帐户已重命名、禁用或启用;或者设置或更改密码。 如果启用了此审核策略设置，则管理员可以跟踪事件，以检测恶意、意外和授权创建的用户帐户。  
  
##### <a name="computer-account-management"></a>计算机帐户管理  
此子类别报告计算机帐户管理的每个事件，例如，在创建、更改、删除、重命名、禁用或启用计算机帐户时。  
  
##### <a name="security-group-management"></a>安全组管理  
此子类别报告安全组管理的每个事件，例如，在创建、更改或删除安全组时，或者在安全组中添加或删除成员时。 如果启用了此审核策略设置，则管理员可以跟踪事件，以检测安全组帐户的恶意、意外和授权创建。  
  
##### <a name="distribution-group-management"></a>通讯组管理  
此子类别报告通讯组管理的每个事件，例如，在创建、更改或删除通讯组时，或在通讯组中添加或删除成员时。 如果启用了此审核策略设置，则管理员可以跟踪事件，以检测恶意、意外和授权创建的组帐户。  
  
##### <a name="application-group-management"></a>应用程序组管理  
此子类别报告计算机上应用程序组管理的每个事件，例如，在创建、更改或删除应用程序组时，或在应用程序组中添加或删除成员时。 如果启用了此审核策略设置，则管理员可以跟踪事件，以检测恶意、意外和授权的应用程序组帐户创建。  
  
##### <a name="other-account-management-events"></a>其他帐户管理事件  
此子类别报告其他帐户管理事件。  
  
#### <a name="detailed-process-tracking"></a>详细的过程跟踪  
  
##### <a name="process-creation"></a>进程创建  
此子类别报告进程的创建以及创建该进程的用户或程序的名称。  
  
##### <a name="process-termination"></a>进程终止  
此子类别报告进程终止的时间。  
  
##### <a name="dpapi-activity"></a>DPAPI 活动  
此子类别报告对数据保护应用程序编程接口（DPAPI）进行加密或解密。 DPAPI 用于保护机密信息，例如存储的密码和密钥信息。  
  
##### <a name="rpc-events"></a>RPC 事件  
此子类别报告远程过程调用（RPC）连接事件。  
  
#### <a name="directory-service-access"></a>目录服务访问  
  
##### <a name="directory-service-access"></a>目录服务访问  
当访问 AD DS 对象时，此子类别将进行报告。 只有具有配置的 Sacl 的对象才会导致生成审核事件，并且仅当以与 SACL 条目匹配的方式访问它们时才会引发这些事件。 这些事件类似于早期版本的 Windows Server 中的 "目录服务访问" 事件。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-changes"></a>目录服务更改  
此子类别报告对 AD DS 中的对象所做的更改。 所报告的更改类型为 "创建"、"修改"、"移动" 和 "撤消删除" 在对象上执行的操作。 目录服务更改审核（如果适用）指示已更改对象的已更改属性的新旧值。 只有具有 Sacl 的对象才会导致生成审核事件，并且仅在以与其 SACL 条目匹配的方式进行访问时才生成审核事件。 由于架构中对象类的设置，某些对象和属性不会导致生成审核事件。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-replication"></a>目录服务复制  
当两个域控制器之间的复制开始和结束时，此子类别将进行报告。  
  
##### <a name="detailed-directory-service-replication"></a>详细的目录服务复制  
此子类别报告有关域控制器之间复制的信息的详细信息。 这些事件在数量上可能非常高。  
  
#### <a name="logonlogoff"></a>登录/注销  
  
##### <a name="logon"></a>登录  
此子类别报告用户尝试登录到系统的时间。 这些事件在访问的计算机上发生。 对于交互式登录，这些事件的生成发生在登录到的计算机上。 如果发生网络登录以访问共享，则这些事件会在承载访问资源的计算机上生成。 如果此设置配置为 "**无审核**"，则很难或不可能确定哪些用户访问或尝试访问组织计算机。  
  
##### <a name="network-policy-server"></a>网络策略服务器  
此子类别报告 RADIUS （IAS）和网络访问保护（NAP）用户访问请求生成的事件。 这些请求可以是**Grant**、 **Deny**、**放弃**、**隔离**、**锁定**和**解锁**。 审核此设置将导致 NPS 和 IAS 服务器上出现中等或大量记录。  
  
##### <a name="ipsec-main-mode"></a>IPsec 主模式  
在主模式协商期间，此子类别报告 Internet 密钥交换（IKE）协议和已验证 Internet 协议（AuthIP）的结果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 扩展模式  
此子类别报告在扩展模式协商期间 AuthIP 的结果。  
  
##### <a name="other-logonlogoff-events"></a>其他登录/注销事件  
此子类别报告其他登录和注销相关事件，例如远程桌面服务会话断开连接和重新连接，使用 RunAs 在不同帐户下运行进程，以及锁定和解锁工作站。  
  
##### <a name="logoff"></a>在用户界面中，  
此子类别报告用户何时从系统注销。 这些事件在访问的计算机上发生。 对于交互式登录，这些事件的生成发生在登录到的计算机上。 如果发生网络登录以访问共享，则这些事件会在承载访问资源的计算机上生成。 如果此设置配置为 "**无审核**"，则很难或不可能确定哪些用户访问或尝试访问组织计算机。  
  
##### <a name="account-lockout"></a>帐户锁定  
由于登录尝试失败次数过多，此子类别将报告用户的帐户被锁定的时间。  
  
##### <a name="ipsec-quick-mode"></a>IPsec 快速模式  
在快速模式协商期间，此子类别报告 IKE 协议和 AuthIP 的结果。  
  
##### <a name="special-logon"></a>特殊登录  
此子类别报告何时使用特殊登录。 特殊的登录名是具有管理员等效权限的登录名，可用于将进程提升到较高的级别。  
  
#### <a name="policy-change"></a>策略更改  
  
##### <a name="audit-policy-change"></a>审核策略更改  
此子类别报告审核策略中的更改，包括 SACL 更改。  
  
##### <a name="authentication-policy-change"></a>身份验证策略更改  
此子类别报告身份验证策略中的更改。  
  
##### <a name="authorization-policy-change"></a>授权策略更改  
此子类别报告包括权限（DACL）更改的授权策略更改。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>MPSSVC 规则级别策略更改  
此子类别报告 Microsoft 保护服务（MPSSVC）所使用的策略规则的更改。 Windows 防火墙使用此服务。  
  
##### <a name="filtering-platform-policy-change"></a>筛选平台策略更改  
此子类别报告在 WFP 中添加和删除对象，包括启动筛选器。 这些事件在数量上可能非常高。  
  
##### <a name="other-policy-change-events"></a>其他策略更改事件  
此子类别报告其他类型的安全策略更改，如受信任的平台模块（TPM）或加密提供程序的配置。  
  
#### <a name="privilege-use"></a>权限使用  
  
##### <a name="sensitive-privilege-use"></a>敏感权限使用  
此子类别报告用户帐户或服务使用敏感权限的时间。 敏感权限包括以下用户权限：作为操作系统的一部分进行操作、备份文件和目录、创建令牌对象、调试程序、使计算机和用户帐户可以被信任以进行委派、生成安全审核身份验证后模拟客户端、加载和卸载设备驱动程序、管理审核和安全日志、修改固件环境值、替换进程级令牌、还原文件和目录以及获取文件或其他对象的所有权。 审核此子类别将产生大量事件。  
  
##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 权限使用  
当用户帐户或服务使用 nonsensitive 权限时，此子类别将进行报告。 Nonsensitive 权限包括以下用户权限：作为受信任的调用方访问凭据管理器、从网络访问此计算机、将工作站添加到域、调整进程的内存配额、允许在本地登录，允许通过远程登录桌面服务，绕过遍历检查，更改系统时间，创建页面文件，创建全局对象，创建永久共享对象，创建符号链接，拒绝从网络访问此计算机、拒绝作为批处理作业登录、拒绝作为服务登录、拒绝本地登录，拒绝通过远程桌面服务登录，从远程系统强制关机，增加进程工作集，提高计划优先级，锁定内存中的页，作为批处理作业登录、作为服务登录、修改对象标签、执行卷维护任务、配置单一进程、配置文件系统性能、从扩展坞中取出计算机、关闭系统并同步目录服务数据。 审核此子类别将创建大量事件。  
  
##### <a name="other-privilege-use-events"></a>其他权限使用事件  
当前未使用此安全策略设置。  
  
#### <a name="object-access"></a>对象访问  
  
##### <a name="file-system"></a>文件系统  
此子类别报告何时访问文件系统对象。 只有具有 Sacl 的文件系统对象才会导致生成审核事件，并且仅在以匹配其 SACL 条目的方式对其进行访问时才生成审核事件。 此策略设置本身将不会引起任何事件的审核。 它确定是否审核访问具有指定系统访问控制列表（SACL）的文件系统对象的用户的事件，从而有效地启用审核。  
  
如果 "审核对象访问" 设置配置为 "**成功**"，则每次用户使用指定的 SACL 成功访问对象时，都会生成一个审核条目。 如果此策略设置配置为**失败**，则每次用户尝试使用指定的 SACL 访问对象失败时都会生成一个审核条目。  
  
##### <a name="registry"></a>注册表  
当访问注册表对象时，此子类别将进行报告。 只有具有 Sacl 的注册表对象会导致生成审核事件，并且仅在以匹配其 SACL 条目的方式对其进行访问时才生成审核事件。 此策略设置本身将不会引起任何事件的审核。  
  
##### <a name="kernel-object"></a>内核对象  
此子类别报告在访问进程和互斥体等内核对象的时间。 只有具有 Sacl 的内核对象会导致生成审核事件，并且仅在以匹配其 SACL 条目的方式对其进行访问时才生成审核事件。 通常，仅当启用了 AuditBaseObjects 或 AuditBaseDirectories 审核选项时，才会向内核对象提供 Sacl。  
  
##### <a name="sam"></a>SAM  
当访问本地安全帐户管理器（SAM）身份验证数据库对象时，此子类别将进行报告。  
  
##### <a name="certification-services"></a>认证服务  
此子类别报告何时执行证书服务操作。  
  
##### <a name="application-generated"></a>已生成应用程序  
当应用程序尝试使用 Windows 审核应用程序编程接口（Api）生成审核事件时，此子类别将进行报告。  
  
##### <a name="handle-manipulation"></a>处理操作  
当打开或关闭对象的句柄时，此子类别将进行报告。 只有具有 Sacl 的对象才会导致生成这些事件，并且仅当尝试的句柄操作与 SACL 条目匹配时才会引发这些事件。 仅为启用了相应对象访问子类别的对象类型（例如，文件系统或注册表）生成句柄操作事件。  
  
##### <a name="file-share"></a>文件共享  
此子类别报告何时访问文件共享。 此策略设置本身将不会引起任何事件的审核。 它确定是否审核访问具有指定系统访问控制列表（SACL）的文件共享对象的用户的事件，从而有效地启用审核。  
  
##### <a name="filtering-platform-packet-drop"></a>筛选平台数据包丢弃  
当 Windows 筛选平台（WFP）丢弃数据包时，此子类别将进行报告。 这些事件在数量上可能非常高。  
  
##### <a name="filtering-platform-connection"></a>筛选平台连接  
此子类别报告通过 WFP 允许或阻止连接的时间。 这些事件的数量可能会很高。  
  
##### <a name="other-object-access-events"></a>其他对象访问事件  
此子类别报告其他与对象访问相关的事件，例如任务计划程序作业和 COM + 对象。  
  
#### <a name="system"></a>系统  
  
##### <a name="security-state-change"></a>安全状态更改  
此子类别报告系统的安全状态更改，例如安全子系统启动和停止的时间。  
  
##### <a name="security-system-extension"></a>安全系统扩展  
此子类别报告安全子系统的扩展代码（例如身份验证包）的加载。  
  
##### <a name="system-integrity"></a>系统完整性  
此子类别报告安全子系统的完整性的冲突。  
  
IPsec 驱动程序  
  
此子类别报告 Internet 协议安全（IPsec）驱动程序的活动。  
  
##### <a name="other-system-events"></a>其他系统事件  
此子类别报告其他系统事件。  
  
有关子类别说明的详细信息，请参阅[Microsoft 安全合规管理器工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每个组织应查看以前涵盖的类别和子类别，并启用最适合其环境的类别和子类别。 在生产环境中部署之前，始终应对审核策略的更改进行测试。  
  
## <a name="configuring-windows-audit-policy"></a>配置 Windows 审核策略

可以使用组策略、auditpol、Api 或注册表编辑来设置 Windows 审核策略。 为大多数公司配置审核策略的建议方法是组策略或 svchost.exe。 设置系统审核策略需要管理员级别的帐户权限或适当的委派权限。  
  
> [!NOTE]  
> 必须为安全主体提供 "**管理审核和安全日志**" 权限（默认情况下，管理员具有该权限），以允许修改单个资源（如文件、Active Directory 对象）的对象访问审核选项，以及注册表项。  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用组策略设置 Windows 审核策略

若要使用组策略设置审核策略，请在 "**计算机配置 \Windows 设置 \ 安全**设置 \ 本地审核策略" 下配置适当的审核类别（请参阅以下屏幕截图，获取本地组策略编辑器（gpedit.msc）。 可以为**成功**、**失败**或**成功**和失败事件启用每个审核策略类别。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
可以通过使用 Active Directory 或本地组策略来设置高级审核策略。 若要设置高级审核策略，请配置位于 "**计算机配置 \Windows 设置 \ 安全设置高级审核策略**" 下的相应子类别（请参阅以下屏幕截图，了解本地组策略编辑器中的示例（gpedit.msc）。 可以为**成功**、**失败**或**成功**和**失败**事件启用每个审核策略子类别。  
  
![监视 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 svchost.exe 设置 Windows 审核策略

Windows Server 2008 和 Windows Vista 中引入了 Auditpol （用于设置 Windows 审核策略）。 最初，只能使用 svchost.exe 来设置高级审核策略，但组策略可以在 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 和 Windows 7 中使用。  
  
Auditpol 是一个命令行实用工具。 语法如下所示：  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Auditpol 语法示例：  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol 在本地设置高级审核策略。 如果本地策略与 Active Directory 或本地组策略发生冲突，组策略设置通常优先于 aspnet_wp.exe 设置。 当存在多个组或本地策略冲突时，只会有一个策略（即替换）。 审核策略将不会合并。  
  
#### <a name="scripting-auditpol"></a>脚本 Auditpol

Microsoft 为想要使用脚本设置高级审核策略，而不是在每个 svchost.exe 命令中手动键入的管理员提供了一个[示例脚本](https://support.microsoft.com/kb/921469)。  
  
**注意**组策略不会始终准确报告所有启用的审核策略的状态，而 svchost.exe 则执行。 有关更多详细信息，请参阅[在 windows 7 和 windows 2008 R2 中获取有效的审核策略](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)。  
  
#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令

Svchost.exe 可用于保存和还原本地审核策略，以及查看其他审核相关命令。 下面是其他**auditpol**命令。  
  
`auditpol /clear`-用于清除和重置本地审核策略  
  
`auditpol /backup /file:<filename>`-用于将当前的本地审核策略备份到二进制文件  
  
`auditpol /restore /file:<filename>`-用于将以前保存的审核策略文件导入到本地审核策略  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`-如果启用此审核策略设置，则会导致系统立即停止（停止：C0000244 {Audit Failed} 消息）（如果出于任何原因无法记录安全审核）。 通常，当安全审核日志已满并且为安全日志指定的保持方法不会**覆盖事件**或**按天覆盖事件**时，将无法记录事件。 通常，它仅由需要更高保障安全日志日志记录的环境启用。 如果启用，则管理员必须密切监视安全日志大小并根据需要轮换日志。 还可以通过修改 security 选项 **Audit 来设置组策略：如果无法记录安全审核 @ no__t （默认值 = 禁用），请立即关闭系统。  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>`-此审核策略设置确定是否审核全局系统对象的访问权限。 如果启用此策略，则会导致使用默认系统访问控制列表（SACL）创建系统对象，如互斥体、事件、信号量和 DOS 设备。 大多数管理员都将审核全局系统对象视为 "干扰"，只有在怀疑恶意攻击时才会启用。 只为指定的对象提供 SACL。 如果还启用了审核对象访问审核策略（或内核对象审核子类别），则会审核对这些系统对象的访问权限。 配置此安全设置时，只有在重新启动 Windows 后，更改才会生效。 还可以通过修改安全选项 "审核全局系统对象的访问权限（默认为禁用）" 来设置此策略组策略。  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`-此审核策略设置指定在创建命名的内核对象（如互斥体和信号量）时为其提供 Sacl。 AuditBaseDirectories 会影响容器对象，而 AuditBaseObjects 会影响不能包含其他对象的对象。  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>`-此审核策略设置指定在将一个或多个这些权限分配给用户安全令牌时，客户端是否生成事件：AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege,TakeOwnershipPrivilege 和 TcbPrivilege。 如果未启用此选项（默认值为 "禁用"），则不会记录 "BackupPrivilege" 和 "RestorePrivilege" 特权。 如果启用此选项，则在执行备份操作的过程中，安全日志会极大地干扰（有时是数百个事件）。 还可以通过修改 security 选项 **Audit 将此策略设置为组策略：审核对备份和还原权限的使用 @ no__t。  
  
> [!NOTE]  
> 此处提供的某些信息是从 Microsoft[审核选项类型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 microsoft SCM 工具获取的。  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>强制执行传统审核或高级审核

在 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 8、Windows 7 和 Windows Vista 中，管理员可以选择启用9个传统类别或使用子类别。 这是一种二进制选择，必须在每个 Windows 系统中进行。 主类别可以启用或 subcategoriesit 不能同时启用。  
  
若要防止旧的传统类别策略覆盖审核策略子类别，你必须启用 "**强制审核策略子类别设置（Windows Vista 或更高版本）" 来替代 "审核策略类别设置**" 策略设置在 "**计算机配置 \Windows 设置 \ 安全设置 \ 安全选项**" 下。  
  
建议启用并配置子类别，而不是九个主要类别。 这要求启用组策略设置（允许子类别重写审核类别）以及配置支持审核策略的不同子类别。  
  
可以通过使用多种方法（包括组策略和命令行程序 svchost.exe）来配置审核子类别。  
  
## <a name="next-steps"></a>后续步骤
  
* [Windows 7 和 Windows Server 2008 R2 中的高级安全审核](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Windows Server 2008 中的审核和符合性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [如何使用组策略为 windows Server 2008 域、windows Server 2003 域或 Windows 2000 域中基于 Windows Vista 和 Windows Server 2008 的计算机配置详细的安全审核设置](https://support.microsoft.com/kb/921469)  
  
* [高级安全审核策略循序渐进指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  

---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 附录 B-Active Directory 中的特权帐户和组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7ea17f6cff1e7c212a9cce080fcaa415eb9ca461
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367766"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 B：Active Directory 中的特权帐户和组

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 B：Active Directory 中的特权帐户和组  
"特权" 帐户和 Active Directory 中的组是指那些向其授予了强大权限、特权和权限的用户，允许他们在 Active Directory 和已加入域的系统上执行几乎所有操作。 本附录首先讨论权限、特权和权限，然后介绍 Active Directory 中 "最高权限" 帐户和组的信息，即最强大的帐户和组。  

除了其权限外，还提供有关 Active Directory 中的内置和默认帐户和组的信息。 尽管提供安全级别最高的帐户和组的特定配置建议作为单独的附录提供，但本附录提供了背景信息，可帮助你识别你应关注的用户和组. 你应这样做，因为攻击者可能会利用它们来损害甚至销毁你的 Active Directory 安装。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>Active Directory 中的权限、特权和权限  
权限、权限和权限之间的差异可能会令人感到困惑和矛盾，甚至是在 Microsoft 的文档中。 本部分介绍了在本文档中使用的各个特性。 对于其他 Microsoft 文档，这些说明不应被视为权威说明，因为它可能会以不同的方式使用这些术语。  

#### <a name="rights-and-privileges"></a>权限和特权  
权限和特权实际上是授予安全主体（如用户、服务、计算机或组）的系统范围相同的功能。 通常由 IT 专业人员使用的接口通常称为 "权限" 或 "用户权限"，它们通常由组策略对象分配。 以下屏幕截图显示了可以分配给安全主体的一些最常见的用户权限（它代表 Windows Server 2012 域中的默认域控制器 GPO）。 其中某些权限适用于 Active Directory （例如 "**使计算机和用户帐户可用于委派**" 用户权限），而其他权限适用于 Windows 操作系统（如**更改系统时间**）。  

![特权帐户和组](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

在组策略对象编辑器的接口中，这些可分配的功能广泛称为用户权限。 但实际上，某些用户权限是以编程方式称为权限，而另一些则是以编程方式称为特权。 表 B-1：用户权限和特权提供了一些最常见的可分配用户权限及其编程常量。 尽管组策略和其他接口是指所有这些接口都是用户权限，但有些接口以编程方式标识为权限，而另一些则定义为特权。  

有关下表中列出的每个用户权限的详细信息，请使用表中的链接，或参阅 [Threats and 对策指南：Microsoft TechNet 站点上 Windows Server 2008 R2 的[威胁和漏洞缓解](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)指南中的用户权限 @ no__t。 有关适用于 Windows Server 2008 的信息，请参阅 Microsoft TechNet 站点上的[威胁和漏洞缓解](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)文档中的[用户权限](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)。 在撰写本文档时，尚未发布有关 Windows Server 2012 的相应文档。  

> [!NOTE]  
> 出于本文档的目的，除非另外指定，否则将使用术语 "权限" 和 "用户权限" 来标识权利和权限。  

##### <a name="table-b-1-user-rights-and-privileges"></a>表 B-1：用户权限和特权  

|||  
|-|-|  
|**组策略中的用户权限**|**常量名称**|  
|[作为受信任的调用方访问凭据管理器](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[从网络访问此计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
|[充当操作系统的一部分](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_3)|SeTcbPrivilege|  
|[将工作站添加到域](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|  
|[调整进程的内存配额](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|  
|[允许本地登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|  
|[允许通过终端服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|  
|[备份文件和目录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_8)|SeBackupPrivilege|  
|[绕过遍历检查](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|  
|[更改系统时间](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|  
|[更改时区](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|  
|[创建页面文件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|  
|[创建令牌对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|  
|[创建全局对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|  
|[创建永久共享对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[创建符号链接](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[调试程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[拒绝从网络访问此计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[拒绝作为批处理作业登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[拒绝作为服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[拒绝本地登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[拒绝通过终端服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[使计算机和用户帐户可用于委派](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
|[从远程系统强制关机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|  
|[生成安全审核](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_24)|SeAuditPrivilege|  
|[身份验证后模拟客户端](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|  
|[增加进程工作集](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|  
|[提高计划优先级](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|  
|[加载和卸载设备驱动程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|  
|[锁定内存页](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|  
|[作为批处理作业登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_30)|SeBatchLogonRight|  
|[作为服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_31)|SeServiceLogonRight|  
|[管理审核和安全日志](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_32)|SeSecurityPrivilege|  
|[修改对象标签](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_33)|SeRelabelPrivilege|  
|[修改固件环境值](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|  
|[执行卷维护任务](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|  
|[分析单一进程](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[配置系统性能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[从扩展坞中删除计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[替换进程级令牌](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[还原文件和目录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[关闭系统](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[同步目录服务数据](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[获得文件或其他对象的所有权](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>权限  
权限是应用于安全对象（如文件系统、注册表、服务和 Active Directory 对象）的访问控制。 每个安全对象都有一个关联的访问控制列表（ACL），其中包含的访问控制项（Ace）允许或拒绝安全主体（用户、服务、计算机或组）对对象执行各种操作的能力。 例如，Active Directory 中的多个对象的 Acl 包含允许经过身份验证的用户读取有关这些对象的常规信息的 Ace，但不授予它们读取敏感信息或更改对象的能力。
除每个域的内置来宾帐户之外，每个登录并通过 Active Directory 林中的域控制器或受信任的林中的域控制器进行身份验证的安全主体都已将经过身份验证的用户安全标识符（SID）添加到其访问权限默认标记。 因此，用户、服务或计算机帐户是否尝试读取域中用户对象的常规属性，读取操作成功。  

如果安全主体试图访问某个对象，但该对象的 Ace 未定义，并且包含主体的访问令牌中存在的 SID，则主体将无法访问该对象。 而且，如果对象的 ACL 中的 ACE 包含与用户访问令牌匹配的 SID 的 "拒绝" 条目，则 "拒绝" ACE 通常会替代冲突的 "允许" ACE。 有关 Windows 中的访问控制的详细信息，请参阅 MSDN 网站上的[访问控制](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx)。  

在本文档中，权限是指授予或拒绝对安全对象安全主体的功能。 每当用户权限和权限之间发生冲突时，用户权限通常优先。 例如，如果 Active Directory 中的对象已配置有一个 ACL，该 ACL 拒绝管理员对某个对象的所有读取和写入访问权限，那么该域的 Administrators 组成员的用户将无法查看有关该对象的很多信息。 但是，因为向管理员组授予用户权限 "获取文件或其他对象的所有权"，所以用户只需获得相关对象的所有权，然后重写对象的 ACL 即可授予管理员对对象的完全控制权限。  

出于此原因，本文档建议你避免使用功能强大的帐户和组进行日常管理，而不是尝试限制帐户和组的功能。 不能有效地阻止有权访问强大凭据的已确定用户使用这些凭据来访问任何安全资源。  

### <a name="built-in-privileged-accounts-and-groups"></a>内置特权帐户和组  
Active Directory 旨在促进管理委派和分配权限的最低权限原则。 默认情况下，在 Active Directory 域中具有帐户的 "常规" 用户可以读取目录中的大部分内容，但只能更改目录中非常有限的一组数据。 需要额外权限的用户可以被授予目录中内置的各种特权组的成员身份，以便他们可以执行与其角色相关的特定任务，但无法执行与任务不相关的任务。  

在 Active Directory 中，有三个内置组，这些组包含目录中的最高特权组：企业管理员（EA）组、域管理员（DA）组和内置管理员（BA）组。  

第四个组（即架构管理员（SA）组）有权损坏或销毁整个 Active Directory 林，但此组的功能比 EA、DA 和 BA 组更受限制。  

除了这四个组外，还在 Active Directory 中还有许多其他内置帐户和默认帐户和组，其中每个帐户和权限都被授予了允许执行特定管理任务的权限。 尽管本附录并未全面讨论 Active Directory 中的每个内置组或默认组，但它提供了一个表，其中显示了你最可能在安装中看到的组和帐户。  

例如，如果将 Microsoft Exchange Server 安装到 Active Directory 林中，则可在域中的内置和用户容器中创建其他帐户和组。 本附录仅介绍了基于本机角色和功能在 Active Directory 中的内置和用户容器中创建的组和帐户。 不包括通过安装企业软件创建的帐户和组。  

#### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins （EA）组位于目录林根级域中，默认情况下，它是林中每个域内内置 Administrators 组的成员。 目录林根级域中的内置管理员帐户是 EA 组中唯一的默认成员。 EAs 被授予了权限，使其能够影响林范围的更改。 这些更改会影响林中的所有域，如添加或删除域、建立林信任或提升林功能级别。 在正确设计和实现的委托模型中，仅在第一次构造林或进行特定林范围的更改（例如建立出站林信任）时才需要 EA 成员身份。  

默认情况下，EA 组位于目录林根级域的 "用户" 容器中，它是一个通用安全组，除非林根域在 Windows 2000 服务器混合模式下运行，在这种情况下，该组为全局安全组。 尽管某些权限直接授予 EA 组，但此组的许多权限实际上是由 EA 组继承的，因为它是林中每个域中 Administrators 组的成员。 Enterprise Admins 对工作站或成员服务器没有默认权限。  

#### <a name="domain-admins"></a>Domain Admins  
林中的每个域都有自己的域管理员（DA）组，该组是该域的内置管理员（BA）组的成员，此外还包含加入域的每台计算机上的本地管理员组的成员。 域的 DA 组唯一的默认成员是该域的内置管理员帐户。  

DAs 在其域中具有全部功能，而 EAs 具有全林性的权限。 在正确设计和实现的委派模型中，只需在 "中断玻璃" 方案中才需要 DA 成员身份，这种情况下，需要对域中的每台计算机使用高级别权限的帐户，或在某些域范围内必须进行更改。 尽管本机 Active Directory 委托机制确实允许在紧急情况下使用 DA 帐户进行委托，但构建有效的委派模型可能会很耗时，并且许多组织都使用第三方用于加快过程的应用程序。  

DA 组是位于域的 "用户" 容器中的全局安全组。 林中的每个域都有一个 DA 组，而一个 DA 组的默认成员是域的内置管理员帐户。 由于域的 "DA" 组嵌套在域的 "BA" 组和每个已加入域的系统本地管理员组中，因此，DAs 不仅具有专门授予给域管理员的权限，而且还继承授予的所有权利和权限域的 Administrators 组和所有已加入域的系统上的本地管理员组。  

#### <a name="administrators"></a>Administrators  
内置管理员（BA）组是域的内置容器中的域本地组，DAs 和 EAs 嵌套在该域中，这是在目录和域控制器上被授予了许多直接权限和权限的组。 但是，域的 Administrators 组对成员服务器或工作站没有任何特权。 已加入域的计算机的本地 Administrators 组中的成员身份是授予本地权限的位置;讨论了组，默认情况下，只有 DAs 是所有已加入域的计算机的本地管理员组的成员。  

Administrators 组是域的内置容器中的域本地组。 默认情况下，每个域的 BA 组都包含本地域的内置管理员帐户、本地域组和林根域的 EA 组。 Active Directory 和域控制器上的许多用户权限专门授予 Administrators 组，而不是 EAs 或 DAs。 域的 BA 组被授予对大多数目录对象的完全控制权限，并且可以获得目录对象的所有权。 尽管在林和域中向 EA 和 DA 组授予特定于对象的特定权限，但实际上，组中的大部分功能都是从其 BA 组中的成员身份 "继承" 的。  

> [!NOTE]  
> 尽管这些是这些特权组的默认配置，但这三个组中的任何一个成员都可以操作该目录，以获取任何其他组的成员身份。 在某些情况下，很难实现，而在其他情况下更难，但从潜在权限的角度来看，所有这三个组都应该被视为有效的等效。  

#### <a name="schema-admins"></a>Schema Admins  
架构管理员（SA）组是目录林根级域中的一个通用组，只包含该域的内置管理员帐户作为默认成员，与 EA 组类似。 尽管 SA 组中的成员身份可以允许攻击者损害 Active Directory 架构（这是整个 Active Directory 林的框架），但 SAs 除了架构外，还具有一些默认权限和权限。  

你应仔细管理和监视 SA 组中的成员身份，但在某些方面，此组比前面介绍的三个最高优先级的组 "权限更低"，因为其权限的作用域非常窄;也就是说，除架构以外，SAs 没有任何其他管理权限。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>Active Directory 中的其他内置和默认组  
为了便于在目录中委派管理，Active Directory 附带了已被授予特定权限的各种内置和默认组。 下表简要描述了这些组。  

下表列出了 Active Directory 中的内置组和默认组。 默认情况下，两组组都存在;不过，内置组会在 Active Directory 的内置容器中找到（默认情况下），而默认组位于 Active Directory 的 "用户" 容器中。 内置容器中的组都是所有域本地组，而 Users 容器中的组是域本地组、全局组和通用组的组合，以及三个单独的用户帐户（管理员、来宾和 Krbtgt）。  

除了本附录前面介绍的最高特权组外，某些内置帐户和默认帐户和组将被授予提升的权限，并且还应受到保护并仅在安全管理主机上使用。 这些组和帐户可在表 B-1 的阴影行中找到：Active Directory 中的内置和默认组和帐户。 由于这些组和帐户中的某些组和帐户被授予了可能被误用以损害 Active Directory 或域控制器的权利和权限，因此它们将作为 [Appendix C 中所述的其他保护措施：Active Directory @ no__t 中的受保护帐户和组。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1：Active Directory 中的内置和默认帐户和组  

||||  
|-|-|-|  
|**帐户或组**|**默认容器、组作用域和类型**|**说明和默认用户权限**|  
|访问控制协助操作员（Windows Server 2012 中的 Active Directory）|内置容器<br /><br />域本地安全组|此组的成员可以远程查询此计算机上的资源的授权属性和权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Account Operators|内置容器<br /><br />域本地安全组|成员可以管理域用户和组帐户。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Administrator 帐户|用户容器<br /><br />不是组|用于管理域的内置帐户。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|Administrators 组|内置容器<br /><br />域本地安全组|管理员对域具有完全且无限制的访问权限。<br /><br />**直接用户权限：**<br /><br />从网络访问此计算机<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权<br /><br />继承的用户权限：<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|允许的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员可以将其密码复制到域中的所有只读域控制器。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Backup Operators|内置容器<br /><br />域本地安全组|备份操作员只能出于备份或还原文件的目的覆盖安全限制。<br /><br />**直接用户权限：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />作为批处理作业登录<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Cert Publishers|用户容器<br /><br />域本地安全组|此组的成员允许将证书发布到目录。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|证书服务 DCOM 访问|内置容器<br /><br />域本地安全组|如果在域控制器上安装了证书服务（不推荐），此组将授予域用户和域计算机 DCOM 注册访问权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|可克隆域控制器（Windows Server 2012AD DS 中的 AD DS）|用户容器<br /><br />全局安全组|此组的成员可以是克隆的域控制器。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Cryptographic Operators|内置容器<br /><br />域本地安全组|授权成员执行加密操作。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|调试器用户|这既不是默认值，也不是内置组，但在 AD DS 中，会导致进一步调查。|调试器用户组的存在指示在某个时间点已在系统上安装了调试工具，不管是通过 Visual Studio、SQL、Office 还是需要并支持调试环境的其他应用程序。 此组允许对计算机进行远程调试访问。 如果此组在域级别存在，则表示已在域控制器上安装了调试器或包含调试器的应用程序。|  
|拒绝的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员不能将其密码复制到域中的任何只读域控制器。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DHCP 管理员|用户容器<br /><br />域本地安全组|此组的成员具有对 DHCP 服务器服务的管理访问权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DHCP 用户|用户容器<br /><br />域本地安全组|此组的成员对 DHCP 服务器服务具有 "仅查看" 访问权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Distributed COM Users|内置容器<br /><br />域本地安全组|此组的成员允许在此计算机上启动、激活和使用分布式 COM 对象。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DnsAdmins|用户容器<br /><br />域本地安全组|此组的成员具有对 DNS 服务器服务的管理访问权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DnsUpdateProxy|用户容器<br /><br />全局安全组|此组的成员是指允许代表不能执行动态更新的客户端来执行动态更新的 DNS 客户端。 此组的成员通常是 DHCP 服务器。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Domain Admins|用户容器<br /><br />全局安全组|域的指定管理员;域管理员是每个已加入域的计算机本地管理员组的成员，并且除了域的 Administrators 组外，还会接收授予本地管理员组的权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|域计算机|用户容器<br /><br />全局安全组|加入该域的所有工作站和服务器都是此组的默认成员。<br /><br />**默认直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域控制器|用户容器<br /><br />全局安全组|域中的所有域控制器。 注意:域控制器不是域计算机组的成员。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域来宾|用户容器<br /><br />全局安全组|域中的所有来宾<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域用户|用户容器<br /><br />全局安全组|域中的所有用户<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Enterprise Admins （仅存在于林根域中）|用户容器<br /><br />通用安全组|企业管理员有权更改林范围的配置设置;Enterprise Admins 是每个域的 Administrators 组的成员，并接收授予该组的权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|企业只读域控制器|用户容器<br /><br />通用安全组|此组包含林中所有只读域控制器的帐户。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Event Log Readers|内置容器<br /><br />域本地安全组|此组的成员可以读取域控制器上的事件日志。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Group Policy Creator Owners|用户容器<br /><br />全局安全组|此组的成员可以创建和修改域中的组策略对象。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Guest|用户容器<br /><br />不是组|这是未将经过身份验证的用户 SID 添加到其访问令牌的 AD DS 域中的唯一帐户。 因此，此帐户将无法访问配置为向经过身份验证的用户组授予访问权限的任何资源。 此行为并不是域来宾和来宾组的成员，然而，这些组的成员会将 "已验证用户" SID 添加到他们的访问令牌。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Guests|内置容器<br /><br />域本地安全组|默认情况下，来宾与 Users 组的成员具有相同的访问权限，但 Guest 帐户除外，如前文所述，这会进一步限制。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Hyper-v 管理员（Windows Server 2012）|内置容器<br /><br />域本地安全组|此组的成员拥有对 Hyper-v 所有功能的完整且不受限制的访问权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|IIS_IUSRS|内置容器<br /><br />域本地安全组|Internet Information Services 使用的内置组。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|传入林信任生成器（仅存在于林根域中）|内置容器<br /><br />域本地安全组|此组的成员可以创建对此林的传入单向信任。 （为企业管理员保留了出站林信任的创建。）<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Krbtgt|用户容器<br /><br />不是组|Krbtgt 帐户是域中 Kerberos 密钥发行中心的服务帐户。 此帐户有权访问 Active Directory 中存储的所有帐户凭据。 此帐户在默认情况下处于禁用状态，因此不应启用<br /><br />**用户权限：** 不可用|  
|Network Configuration Operators|内置容器<br /><br />域本地安全组|此组的成员被授予权限，使其可以管理网络功能的配置。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Performance Log Users|内置容器<br /><br />域本地安全组|此组的成员可以计划性能计数器的日志记录、启用跟踪提供程序，以及在本地收集事件跟踪以及通过远程访问计算机收集事件跟踪。<br /><br />**直接用户权限：**<br /><br />作为批处理作业登录<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Performance Monitor Users|内置容器<br /><br />域本地安全组|此组的成员可以本地和远程访问性能计数器数据。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Windows 之前2000兼容访问|内置容器<br /><br />域本地安全组|存在此组是为了与 Windows 2000 服务器之前的操作系统向后兼容，并提供成员读取域中的用户和组信息的能力。<br /><br />**直接用户权限：**<br /><br />从网络访问此计算机<br /><br />跳过遍历检查<br /><br />**继承的用户权限：**<br /><br />将工作站添加到域<br /><br />增加进程工作集|  
|打印操作员|内置容器<br /><br />域本地安全组|此组的成员可以管理域打印机。<br /><br />**直接用户权限：**<br /><br />允许本地登录<br /><br />加载和卸载设备驱动程序<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RAS 和 IAS 服务器|用户容器<br /><br />域本地安全组|此组中的服务器可以读取域中用户帐户的远程访问属性。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 终结点服务器（Windows Server 2012）|内置容器<br /><br />域本地安全组|此组中的服务器运行用户 RemoteApp 程序和个人虚拟机在其中运行的虚拟机和主机会话。 此组需要在运行 RD 连接代理的服务器上填充。 在部署中使用的 RD 会话主机服务器和 RD 虚拟化主机服务器需要位于此组中。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 管理服务器（Windows Server 2012）|内置容器<br /><br />域本地安全组|此组中的服务器可以在运行远程桌面服务的服务器上执行常规管理操作。 此组需要在远程桌面服务部署中的所有服务器上进行填充。 运行 RDS Central Management 服务的服务器必须包含在此组中。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 远程访问服务器（Windows Server 2012）|内置容器<br /><br />域本地安全组|此组中的服务器允许 RemoteApp 程序和个人虚拟机的用户访问这些资源。 在面向 Internet 的部署中，这些服务器通常部署在边缘网络中。 此组需要在运行 RD 连接代理的服务器上填充。 在部署中使用的 RD 网关服务器和 RD Web 访问服务器需要位于此组中。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|只读域控制器|用户容器<br /><br />全局安全组|此组包含域中的所有只读域控制器。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Remote Desktop Users|内置容器<br /><br />域本地安全组|此组的成员被授予使用 RDP 远程登录的权限。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|远程管理用户（Windows Server 2012）|内置容器<br /><br />域本地安全组|此组的成员可以通过管理协议（如通过 Windows 远程管理服务的 WS-MANAGEMENT）访问 WMI 资源。 这仅适用于向用户授予访问权限的 WMI 命名空间。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Replicator|内置容器<br /><br />域本地安全组|支持域中的旧文件复制。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|架构管理员（仅存在于林根域中）|用户容器<br /><br />通用安全组|只有架构管理员才能对 Active Directory 架构进行修改，并且仅当该架构是启用写功能时才可以修改。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Server Operators|内置容器<br /><br />域本地安全组|此组的成员可以管理域服务器。<br /><br />**直接用户权限：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />更改系统时间<br /><br />更改时区<br /><br />从远程系统强制关机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|终端服务器许可证服务器|内置容器<br /><br />域本地安全组|此组的成员可以使用有关许可证颁发的信息更新 Active Directory 的用户帐户，以跟踪和报告 TS 每用户 CAL 使用情况<br /><br />**默认直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|用户|内置容器<br /><br />域本地安全组|用户有权允许他们读取 Active Directory 中的许多对象和属性，但它们不能更改大多数。 用户被阻止进行意外或有意的系统范围的更改，并可运行大多数应用程序。<br /><br />**直接用户权限：**<br /><br />增加进程工作集<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查|  
|Windows 授权访问组|内置容器<br /><br />域本地安全组|此组的成员有权访问用户对象上的已计算 tokenGroupsGlobalAndUniversal 属性<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|WinRMRemoteWMIUsers_ （Windows Server 2012）|用户容器<br /><br />域本地安全组|此组的成员可以通过管理协议（如通过 Windows 远程管理服务的 WS-MANAGEMENT）访问 WMI 资源。 这仅适用于向用户授予访问权限的 WMI 命名空间。<br /><br />**直接用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  

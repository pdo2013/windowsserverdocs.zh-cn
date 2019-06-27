---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 附录 B-特权帐户和组在 Active Directory 中
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6a4bfa6d59bd8049d39106a4cbd4f8a4dee8ba8
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407676"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 B：权限的帐户和 Active Directory 中的组

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 B：权限的帐户和 Active Directory 中的组  
在 Active Directory"特权"帐户和组是功能强大的权限、 权限和权限都授予，使其能够在 Active Directory 和已加入域的系统上执行几乎任何操作。 本附录首先介绍权限、 权限和权限后, 跟有关"最高特权"帐户和 Active Directory 中的组的信息，它是功能最强大的帐户和组。  

有关内置的和默认帐户和 Active Directory 组中，除了它们的权限，还提供信息。 尽管作为单独的附录提供了用于保护的最高特权帐户和组特定的配置建议，但本附录提供了可帮助你识别的用户和组应重点保护的背景信息. 您应这样做，因为攻击者能够破坏，甚至会破坏您的 Active Directory 安装可以利用它们。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>权限、 权限和 Active Directory 中的权限  
令人困惑且相互矛盾，即使在来自 Microsoft 的文档中，可以是权限、 权限和特权之间的差异。 如本文档中使用本部分介绍了一些的每个特征。 这些说明不应将权威有关其他 Microsoft 文档，因为它可能以不同的方式使用这些术语。  

#### <a name="rights-and-privileges"></a>权限和特权  
权限和特权，实际上是相同的系统级功能，例如用户、 服务、 计算机或组的安全主体授予的。 在界面中通常由 IT 专业人员，这些通常称为"权限"或"用户权限，"并通常由组策略对象。 下面的屏幕截图显示了一些可以分配给安全主体的最常见用户权限 （它表示默认域控制器 GPO 在 Windows Server 2012 域中）。 这些权限的一些适用于 Active Directory 中，例如**使计算机和用户帐户以进行受信任可以进行委派**用户权限，而其他权限适用于 Windows 操作系统，如**更改的系统时间**。  

![特权的帐户和组](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

在接口如组策略对象编辑器中，所有这些可分配功能即是广泛的用户权限。 在现实中但是，某些用户权限以编程方式统称为权限，而其他人以编程方式称为特权。 表 B-1:用户权限和特权提供的一些最常见的可分配的用户权限和其编程的常量。 虽然组策略和其他接口引用所有这些为用户权限，但某些以编程方式标识为权限，而其他人被定义为权限。  

有关每个下表中列出的用户权限的详细信息，使用表中的链接，或参阅[威胁和对策指南：用户权限](https://technet.microsoft.com/library/hh125917(v=ws.10).aspx)中[威胁和漏洞减少](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 站点上的 Windows Server 2008 R2 的指南。 有关适用于 Windows Server 2008 的信息，请参阅[用户权限](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)中[威胁和漏洞减少](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 网站上的文档。 在撰写本文档时，Windows Server 2012 的相应文档尚未发布。  

> [!NOTE]  
> 有关此文档的目的，使用术语"权限"和"用户权限"来标识的权限和特权，除非另行指定。  

##### <a name="table-b-1-user-rights-and-privileges"></a>表 B-1:用户权限和特权  

|||  
|-|-|  
|**直接在组策略中的用户**|**常量的名称**|  
|[作为受信任调用方的访问凭据管理器](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[从网络访问这台计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
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
|[创建永久共享的对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[创建符号链接](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[调试程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[拒绝从网络访问这台计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[拒绝作为批处理作业登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[拒绝作为服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[拒绝本地登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[拒绝通过终端服务登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[使计算机和用户帐户以进行受信任可以进行委派](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
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
|[配置单一进程](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[配置文件系统性能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[从扩展坞中删除计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[替换进程级令牌](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[还原文件和目录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[关闭系统](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[同步目录服务数据](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[取得文件或其他对象的所有权](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>权限  
权限是应用于安全对象的对象，如文件系统、 注册表、 服务和 Active Directory 对象的访问控制。 每个安全对象具有关联的访问控制列表 (ACL)，其中包含访问控制项 (Ace) 的授予或拒绝对对象执行各种操作的功能的安全主体 （用户、 服务、 计算机或组）。 例如，Active Directory 中的许多对象的 Acl 包含 Ace 允许经过身份验证用户读取有关的对象的常规信息，但不授予他们读取敏感的信息或更改对象的权限。
除了内置来宾帐户的每个域，每个安全主体的登录并在 Active Directory 林或受信任的林中的域控制器进行身份验证已通过身份验证的用户安全标识符 (SID) 添加到其访问权限默认情况下的标记。 因此，用户、 服务或计算机帐户尝试读取域中的用户对象上的常规属性，读取的操作成功。  

如果定义的安全主体尝试访问的任何 ace 的对象，并包含一个主体的访问令牌中存在的 SID，主体无法访问的对象。 此外，如果对象的 ACL 中的 ACE 的拒绝条目包含与用户的访问令牌，"拒绝"ACE 相匹配的 sid 将通常一个冲突的重写"允许"ACE。 有关 Windows 中的访问控制的详细信息，请参阅[访问控制](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx)MSDN 网站上。  

在本文档中，权限是指被授予或拒绝对安全对象上的安全主体的功能。 只要用户权限和权限之间存在冲突，用户权限通常优先。 例如，如果 Active Directory 中的对象已配置了拒绝管理员，所有读取和写入访问的对象的 ACL，是域的管理员组的成员的用户将无法查看有关对象太多信息。 但是，管理员组被授予用户适当"取得所有权的文件或其他对象"，因为用户可以只需将对象的所有权有问题，然后重写该对象的 ACL，以授予对象的管理员完全控制权限。  

正是出于此原因，本文档会鼓励您避免使用的日常管理功能强大的帐户和组，而不是尝试限制的帐户和组的功能。 不可以有效地停止确定的用户有权访问使用这些凭据来访问任何安全对象的资源从功能强大的凭据。  

### <a name="built-in-privileged-accounts-and-groups"></a>内置特权帐户和组  
Active Directory 旨在促进委派的管理和分配权利和权限中的最小特权原则。 Active Directory 域中具有帐户的"常规"用户在默认情况下，能够读取大量的目录中存储的内容，但可以更改仅一组非常有限的目录中的数据。 可以在各种内置目录，以便它们可能执行与其角色相关的特定任务，但不能执行其职责与不相关的任务的特权组中的成员身份授予需要额外权限的用户。  

在 Active Directory 中，有三个内置组构成的目录中的最高特权组： Enterprise Admins (EA) 组、 域管理员 (DA) 组和内置的管理员 (BA) 组。  

第四个组，架构管理员 (SA) 组，具有的权限，如果被滥用，可能会损坏或销毁整个 Active Directory 林中，但此组是更受限制的其功能比 EA、 DA 和 BA 组。  

除了这四个组，还有许多其他内置和默认帐户和组在 Active Directory，其中每个授予权限并允许执行特定管理任务的权限。 尽管本附录不提供 Active Directory 中的每个内置或默认组的深入讨论，但它确实提供组和您最有可能看到在您的安装的帐户的表。  

例如，如果您安装 Microsoft Exchange Server 到 Active Directory 林时，其他帐户和组可能会创建在你的域中的内置和用户容器中。 本附录介绍组和基于本机角色和功能的 Active Directory 中的内置和用户容器中创建的帐户。 帐户和组创建的企业软件的安装不包括。  

#### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) 组位于林根域，并且默认情况下，它为林中的每个域中的内置 Administrators 组的成员。 目录林根域中的内置 Administrator 帐户是 EA 组的唯一默认成员。 EAs 被授予权利和权限，允许它们会影响全林性的更改。 它们会影响所有域林中，如添加或删除域、 建立林信任或提升林功能级别的更改。 在正确设计和实施委派模型中，EA 成员身份是所需，仅当首次构造该林或进行某些全林性更改，例如建立出站林信任。  

EA 组位于林根域中的用户容器中的默认情况下，它是通用安全组，除非目录林根域运行在 Windows 2000 Server 混合模式下，在其中用例组是一个全局安全组。 尽管某些权限直接授予 EA 组，但许多此组的权限实际上由继承 EA 组因为它是林中每个域中的管理员组的成员。 企业管理员工作站或成员服务器上有没有默认权限。  

#### <a name="domain-admins"></a>Domain Admins  
在林中每个域都有其自己的域管理员 (DA) 组，即除了已加入域的每台计算机上的本地管理员组的成员的域的内置管理员 (BA) 组的成员。 DA 组为域的唯一默认成员是该域的内置 Administrator 帐户。  

DAs 是其领域内一完全权限，而 EAs 具有全林性权限。 在正确设计和实施委派模型中，应仅在"不受限"方案中，这是在域中的每台计算机上具有高级别的特权的帐户输入，或在某些时的情况下需要 DA 成员身份全域性必须进行更改。 尽管本机 Active Directory 委派机制允许委派的范围内就可以使用 DA 帐户仅在紧急情况下，构造一个有效的委派模型可能会非常耗时，并且许多组织使用第三方要加快该过程的应用程序。  

DA 组是位于域的用户容器中的全局安全组。 为每个域、 林的一个数据组和 DA 组的唯一默认成员是域的内置 Administrator 帐户。 因为域的数据组嵌套在域的 BA 组和每个已加入域的系统的本地 Administrators 组中、 DAs 不仅必须专门授予到域管理员的权限，但它们还继承的所有权利和权限授予域的管理员组并加入到域中的所有系统上的本地管理员组。  

#### <a name="administrators"></a>Administrators  
内置管理员 (BA) 组是域的内置容器到 DAs 和 EAs 嵌套的并且它是已获得的许多直接权限和权限的目录中和域控制器上的此组中的域本地组。 但是，域管理员组不会不具有任何权限，在成员服务器或工作站上。 已加入域的计算机的本地管理员组的成员身份是其中授予本地特权;和讨论的组，仅 DAs 是默认情况下所有已加入域的计算机的本地管理员组的成员。  

Administrators 组是域的内置容器中的域本地组。 默认情况下，每个域 BA 组包含本地域的内置 Administrator 帐户、 本地域 DA 组和目录林根域的 EA 组。 许多用户权限在 Active Directory 和域控制器上专门授予管理员组不是向 EAs 或 DAs。 域的 BA 组被授予完全控制权限大多数目录对象，并可以获得的目录对象的所有权。 尽管 EA 和 DA 组授予的林和域中的某些特定于对象的权限，但很多组的强大功能实际上"继承"自 BA 组中的成员身份。  

> [!NOTE]  
> 尽管这些是这些特权组的默认配置，但任何一种的三个组的成员可以操作的目录以获取任何其他组中的成员身份。 在某些情况下，很容易实现，而在其他人会更加困难，但从潜在的权限的角度来看，所有三个组应被视为有效地等效。  

#### <a name="schema-admins"></a>Schema Admins  
架构管理员 (SA) 组是目录林根域中的通用组，作为默认成员，类似于 EA 组只有该域的内置 Administrator 帐户。 尽管中的 SA 组的成员身份可以允许攻击者危害 Active Directory 架构，这是整个 Active Directory 林的框架，SAs 有几个默认权限和权限超出该架构的权限。  

您应仔细管理和监视中的 SA 组的成员身份，但在某些方面，此组是"较低"比这三个最高特权组及其权限的作用域是范围非常窄; 因为前面所述也就是说，SAs 有架构以外的任意位置没有管理权限。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>其他内置和 Active Directory 中的默认组  
为了便于委派管理的目录中，Active Directory 提供各种内置组和默认组已被授予特定权限和权限。 下表简要介绍了这些组。  

下表列出了在 Active Directory 中的内置和默认组。 这两个集的组存在默认设置。但是，内置组位于 （默认情况下） 中的内置容器中 Active Directory 时默认组位于 Active Directory 中的用户容器中 （默认）。 内置容器中的组是所有域本地组，而用户容器中的组都是域本地、 全局和通用组，除了三个单独的用户帐户 （Administrator、 Guest 和 Krbtgt） 的混合。  

在本附录前面部分中所述的最高特权组中，除了一些内置的和默认帐户和组被授予提升的权限也是受保护并应当只能在安全的管理主机上使用。 可以在表 B-1 阴影的行中找到这些组和帐户：内置和默认组和 Active Directory 中的帐户。 因为其中的某些组和帐户被授予权限的权利和权限可能被误用来危害 Active Directory 或域控制器，它们同时也为额外的保护中所述[附录 c:受保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1:内置和默认帐户和 Active Directory 中的组  

||||  
|-|-|-|  
|**帐户或组**|**默认容器、 组作用域和类型**|**说明和默认用户权限**|  
|访问控制协助运算符 (Windows Server 2012 中的 Active Directory)|内置容器<br /><br />域本地安全组|授权属性和此计算机上的资源的权限，此组的成员可以远程查询。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Account Operators|内置容器<br /><br />域本地安全组|成员可以管理域用户和组帐户。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Administrator 帐户|用户容器<br /><br />不是组|用于管理域的内置帐户。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|管理员组|内置容器<br /><br />域本地安全组|管理员具有完全的无限制访问权限域。<br /><br />**直接的用户权限：**<br /><br />从网络访问此计算机<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权<br /><br />继承的用户权限：<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|允许的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员可以具有其密码复制到域中的所有只读域控制器。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Backup Operators|内置容器<br /><br />域本地安全组|备份或还原文件的唯一目的，备份操作员可以重写安全限制。<br /><br />**直接的用户权限：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />作为批处理作业登录<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Cert Publishers|用户容器<br /><br />域本地安全组|此组的成员有权将证书发布到的目录。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|证书服务 DCOM 访问权限|内置容器<br /><br />域本地安全组|（不推荐） 在域控制器上安装证书服务，如果此组将授予给域用户和域的计算机的 DCOM 注册访问权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|可克隆域控制器 (在 Windows Server 2012AD DS 中的 AD DS)|用户容器<br /><br />全局安全组|可以克隆域控制器是此组的成员。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Cryptographic Operators|内置容器<br /><br />域本地安全组|成员有权执行加密操作。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|调试程序用户|这是默认值既不内置组，但当在 AD DS 中存在，以便进一步进行调查原因。|调试程序用户组存在指示是否通过 Visual Studio、 SQL、 Office 或其他应用程序的需要，并且支持调试的环境具有已调试工具安装在某些时候，在系统上。 此组允许对计算机的远程调试访问。 如果此组在域级别存在，它指示调试器或包含调试程序的应用程序已安装程序在域控制器上。|  
|被拒绝的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员不能具有其密码复制到域中的任何只读域控制器。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DHCP 管理员|用户容器<br /><br />域本地安全组|此组的成员具有对 DHCP 服务器服务的管理访问权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DHCP 用户|用户容器<br /><br />域本地安全组|此组的成员具有对 DHCP 服务器服务的只读访问。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Distributed COM Users|内置容器<br /><br />域本地安全组|此组的成员可以启动、 激活和此计算机上使用分布式的 COM 对象。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DnsAdmins|用户容器<br /><br />域本地安全组|此组的成员具有对 DNS 服务器服务的管理访问权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|DnsUpdateProxy|用户容器<br /><br />全局安全组|此组的成员是允许执行代表本身不能执行动态更新的客户端的动态更新 DNS 客户端。 此组的成员通常是 DHCP 服务器。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Domain Admins|用户容器<br /><br />全局安全组|指定的管理员的应用程序域。域管理员是每个已加入域的计算机的本地 Administrators 组的成员，并且接收到本地管理员组中，除了域的管理员组授予权利和权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|域的计算机|用户容器<br /><br />全局安全组|此组的默认成员的所有工作站和服务器加入到域都。<br /><br />**默认直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域控制器|用户容器<br /><br />全局安全组|在域中的所有域控制器。 注意：域控制器不是域的计算机组的成员。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域来宾|用户容器<br /><br />全局安全组|在域中的所有来宾<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|域用户|用户容器<br /><br />全局安全组|域中的所有用户<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|企业管理员 （仅在目录林根域中存在）|用户容器<br /><br />通用安全组|企业管理员有权更改林范围的配置设置;企业管理员是每个域管理员组的成员和接收权限并向该组授予的权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />为进程调整内存配额<br /><br />允许本地登录<br /><br />允许通过远程桌面服务登录<br /><br />备份文件和目录<br /><br />跳过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全局对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />信任计算机和用户帐户可以执行委派<br /><br />从远程系统强制关机<br /><br />身份验证后模拟客户端<br /><br />增加进程工作集<br /><br />提高计划优先级<br /><br />加载和卸载设备驱动程序<br /><br />作为批处理作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行批量维护任务<br /><br />配置文件单个进程<br /><br />配置文件系统性能<br /><br />从扩展坞中移除计算机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />取得文件或其他对象的所有权|  
|企业只读域控制器|用户容器<br /><br />通用安全组|此组包含林中所有只读域控制器的帐户。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Event Log Readers|内置容器<br /><br />域本地安全组|在此组的成员可以读取域控制器上的事件日志。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Group Policy Creator Owners|用户容器<br /><br />全局安全组|此组的成员可以创建和修改域中的组策略对象。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Guest|用户容器<br /><br />不是组|这是不具有身份验证的用户 SID 添加到其的访问令牌中的 AD DS 域中的唯一帐户。 因此，配置为向 Authenticated Users 组授予访问权限的任何资源将无法访问此帐户。 此行为是不正确的域来宾和来宾组的成员，但-这些组的成员执行具有经过身份验证的用户 SID 添加到其的访问令牌。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Guests|内置容器<br /><br />域本地安全组|来宾具有相同的访问权限的用户组的成员默认情况下，除了来宾帐户，这是进一步限制如前面所述。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|HYPER-V 管理员 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组的成员具有 HYPER-V 的所有功能的完整且不受限制访问权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|IIS_IUSRS|内置容器<br /><br />域本地安全组|使用 Internet 信息服务的内置组。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|传入 Forest Trust Builders （仅在目录林根域中存在）|内置容器<br /><br />域本地安全组|此组的成员可以创建到此林的传入的单向信任。 （出站林信任关系创建保留的 Enterprise Admins 中）。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Krbtgt|用户容器<br /><br />不是组|Krbtgt 帐户是域中 Kerberos 密钥分发中心的服务帐户。 此帐户有权访问存储在 Active Directory 中的所有帐户的凭据。 此帐户默认处于禁用状态，因此应该永远不会启用<br /><br />**用户权限：** 不可用|  
|Network Configuration Operators|内置容器<br /><br />域本地安全组|此组的成员将授予权限，使他们可以管理网络功能的配置。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Performance Log Users|内置容器<br /><br />域本地安全组|此组的成员可以计划的性能计数器的日志记录、 启用跟踪提供程序和到计算机本地和远程访问通过收集事件跟踪。<br /><br />**直接的用户权限：**<br /><br />作为批处理作业登录<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Performance Monitor Users|内置容器<br /><br />域本地安全组|此组的成员可以从本地和远程访问性能计数器数据。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Pre-Windows 2000 Compatible Access|内置容器<br /><br />域本地安全组|此组已存在为了向后兼容 Windows 2000 Server 之前的操作系统，并提供成员读取用户和组信息的域中的功能。<br /><br />**直接的用户权限：**<br /><br />从网络访问此计算机<br /><br />跳过遍历检查<br /><br />**继承的用户权限：**<br /><br />将工作站添加到域<br /><br />增加进程工作集|  
|打印操作员|内置容器<br /><br />域本地安全组|此组的成员可以管理域打印机。<br /><br />**直接的用户权限：**<br /><br />允许本地登录<br /><br />加载和卸载设备驱动程序<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RAS 和 IAS 服务器|用户容器<br /><br />域本地安全组|此组中的服务器可以读取在域中的用户帐户上的远程访问属性。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 终结点的服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器运行的虚拟机和主机会话用户 RemoteApp 程序和个人虚拟桌面运行的位置。 需要运行 RD 连接代理的服务器上填充此组。 RD 会话主机服务器，部署中使用的 RD 虚拟化主机服务器必须在此组中。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 管理服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器可以运行远程桌面服务的服务器上执行日常管理操作。 需要在远程桌面服务部署中的所有服务器上填充此组。 运行 RDS 中央管理服务的服务器必须包含在该组中。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|RDS 的远程访问服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器启用 RemoteApp 程序和个人虚拟桌面访问这些资源的用户。 在面向 Internet 的部署中，这些服务器通常部署在边界网络中。 需要运行 RD 连接代理的服务器上填充此组。 RD 网关服务器和 RD Web 访问服务器部署中使用需要是此组中。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|只读域控制器|用户容器<br /><br />全局安全组|此组包含域中的所有只读域控制器。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Remote Desktop Users|内置容器<br /><br />域本地安全组|此组的成员授予远程使用 RDP 登录的权限。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|远程管理用户 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组的成员可以通过管理协议 （如 WS 管理通过 Windows 远程管理服务） 访问 WMI 资源。 这仅适用于向用户授予访问权限的 WMI 命名空间。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Replicator|内置容器<br /><br />域本地安全组|支持域中的旧文件复制。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|架构管理员 （仅在目录林根域中存在）|用户容器<br /><br />通用安全组|架构管理员都可以进行到 Active Directory 架构，且仅当修改架构的唯一用户是启用写操作的。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|Server Operators|内置容器<br /><br />域本地安全组|此组的成员可以管理域服务器。<br /><br />**直接的用户权限：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />更改系统时间<br /><br />更改时区<br /><br />从远程系统强制关机<br /><br />还原文件和目录<br /><br />关闭系统<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|终端服务器许可证服务器|内置容器<br /><br />域本地安全组|此组的成员可以使用许可证的颁发，用于跟踪和报告 TS 每用户 CAL 使用情况有关的信息来更新 Active Directory 中的用户帐户<br /><br />**默认直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|用户|内置容器<br /><br />域本地安全组|用户具有允许他们阅读许多对象和属性在 Active Directory，尽管它们不能更改大多数权限。 用户被阻止进行意外或故意系统范围的更改，并可以运行大多数应用程序。<br /><br />**直接的用户权限：**<br /><br />增加进程工作集<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查|  
|Windows Authorization Access Group|内置容器<br /><br />域本地安全组|此组的成员的用户对象上有权访问计算的 tokenGroupsGlobalAndUniversal 属性<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  
|WinRMRemoteWMIUsers_ (Windows Server 2012)|用户容器<br /><br />域本地安全组|此组的成员可以通过管理协议 （如 WS 管理通过 Windows 远程管理服务） 访问 WMI 资源。 这仅适用于向用户授予访问权限的 WMI 命名空间。<br /><br />**直接的用户权限：** 无<br /><br />**继承的用户权限：**<br /><br />从网络访问此计算机<br /><br />将工作站添加到域<br /><br />跳过遍历检查<br /><br />增加进程工作集|  

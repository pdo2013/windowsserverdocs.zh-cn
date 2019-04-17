---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: "附录 B-特权帐户，并在 Active Directory 中进行分组"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c4f98b1b68ebdf290ba15b75274d7a876912565b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 b：特权的帐户和 Active Directory 中的组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附录 b：特权的帐户和 Active Directory 中的组  
"权限"帐户和组 Active Directory 中的那些功能强大的权利、 权限和权限授予，允许他们在 Active Directory 和加入域的系统上执行的几乎任何操作。 此附录首先讨论权利，权限，并权限后, 跟信息的"最高权限"帐户和组 Active Directory，即最强大的帐户和组。  

信息还提供了有关内置和默认帐户和中 Active Directory，除了他们权限的组。 尽管单独附录提供了特定配置建议保护最高权限帐户和多个组，但此附录将提供帮助的用户和组你应该以此为重点保护找出你的背景信息。 你应该这样做，因为以供攻击者甚至销毁 Active Directory 安装和损害。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>权利、 权限和 Active Directory 的权限  
感到很困惑和清楚，甚至在文档中来自 Microsoft，可以是权利、 权限和特权之间的区别。 此部分中介绍的某些特征每个本文档中的使用。 这些说明不应该将视为权威的其他 Microsoft 文档，因为它可能会以不同的方式使用这些条款。  

#### <a name="rights-and-privileges"></a>权利和的权限  
权限和特权实际上是授予如用户、 服务、 计算机或组安全主体同一个系统范围内功能。 在接口通常由 IT 专业人员，这通常称为"权限"或"用户权限，"，并通常由组策略对象。 下面的屏幕截图显示了一些最常见的权利即可分配到安全主体 （表示默认域控制器 GPO Windows Server 2012 域中）。 这些权利的一部分将应用于 Active Directory，如**允许计算机与用户帐户被委派**用户权限，而其他权利适用于 Windows 操作系统，如**更改系统时间**。  

![特权的帐户和组](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

在组策略编辑器如接口中，所有这些转让功能称为广泛为用户的权利。 实际上但是，某些用户权限编程方式称为权利，其他人编程方式称为权限时。 表 B-1： 用户权限和特权提供了一些最常见的用户可分配权利以及他们编程常量。 虽然组策略和其他接口引用所有这些为用户的权利，但一些编程方式标识为权利，而其他被定义为权限。  

有关每个如下表中所列出的用户权利的详细信息，使用表中的链接，或查看[威胁并对策指南： 用户权限](https://technet.microsoft.com/library/hh125917(v=ws.10).aspx)中[威胁并漏洞缓解](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 网站上的 Windows Server 2008 R2 的指南。 适用于 Windows Server 2008 的信息，请参阅[用户权限](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)中[威胁并漏洞缓解](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 网站上的文档。 本文档写作，Windows Server 2012 对应的文档尚未发布。  

> [!NOTE]  
> 出于该文档，使用条款"权限"和"用户权限"找出的权利和权限，除非另有说明。  

##### <a name="table-b-1-user-rights-and-privileges"></a>表 B-1： 用户权限和特权  

|||  
|-|-|  
|**直接在组策略中的用户**|**常量的名称**|  
|[访问为受信任的调用方的凭据管理器](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[访问网络从这台计算机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
|[作为操作系统的一部分](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_3)|SeTcbPrivilege|  
|[添加工作站到域](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|  
|[调整进程的内存配额](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|  
|[允许本地登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|  
|[通过终端服务允许登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|  
|[备份文件和目录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_8)|SeBackupPrivilege|  
|[绕过遍历检查](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|  
|[更改系统时间](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|  
|[更改时区](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|  
|[创建页面文件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|  
|[创建标记对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|  
|[创建全球对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|  
|[创建永久共享的对象](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[创建符号链接](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[调试程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[拒绝该计算机的访问从网络](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[作为批量作业拒绝登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[作为一项服务拒绝登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[本地拒绝登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[拒绝终端服务通过登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[允许受信任委派计算机和用户帐户](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
|[从系统远程强制关机](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|  
|[生成安全审核](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_24)|SeAuditPrivilege|  
|[模拟后身份验证的客户端](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|  
|[增加进程工作集](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|  
|[增加计划优先级](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|  
|[安装和卸载设备驱动程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|  
|[在内存中的锁屏页面](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|  
|[作为批量作业登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_30)|SeBatchLogonRight|  
|[作为一项服务上登录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_31)|SeServiceLogonRight|  
|[管理审核和安全日志](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_32)|SeSecurityPrivilege|  
|[修改对象标签](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_33)|SeRelabelPrivilege|  
|[修改固件环境值](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|  
|[执行音量维护任务](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|  
|[个人资料单个进程](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[个人资料系统性能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[计算机中删除拓展坞](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[替换过程级别标记](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[还原文件和目录](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[关机系统](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[同步目录服务的数据](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[获得文件或其他对象的所有权](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>权限  
权限的应用于安全对象，如系统文件、 注册表、 服务和 Active Directory 对象的访问权限控件。 每个安全的对象有相关的访问控件列表 (ACL)，其中包含访问控制项 (Ace) 授予或拒绝安全主体 （用户、 服务、 计算机或组） 的能力，以执行各种操作对象上。 例如，在 Active Directory 对象多的 Acl 包含 Ace 允许验证用户读取常规信息对象，但不要授予他们阅读敏感信息或更改对象的功能。
除外每个域内置的来宾帐户，登录，并通过 Active Directory 森林或受信任的森林中的域控制器的身份验证每安全主体具有身份验证用户安全标识符 (SID) 添加到其访问令牌默认情况。 因此，用户、 服务或计算机帐户尝试阅读在域中的用户对象常规属性，是否已成功读取的操作。  

如果定义安全主要尝试不访问的任何 Ace 对象，包含在主体访问令牌存在一个 SID 主体无法访问对象。 此外，如果在对象 ACL A 为相匹配的用户访问令牌"拒绝"ACE SID 包含拒绝条目将通常替代一个冲突的"允许"ACE。 有关 Windows 中访问控制的详细信息，请参阅[访问控制](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx)MSDN 网站上。  

在本文中，权限指授予或拒绝安全主体的安全对象的功能。 只要有冲突用户向右和权限，用户权限通常优先。 例如，如果已使用所有读取和写入访问权限的对象都拒绝管理员 ACL 配置中 Active Directory 对象的域的管理员组成员的用户将无法查看剩余对象的信息。 但是，因为用户右"取得的所有权文件或其他对象"授予管理员组时，用户只需可以将拥有的对象有问题，，然后重写的对象 ACL 授予管理员完全控制的对象。  

出于此原因本文档鼓励你避免对于日常管理，使用强大的帐户和组而不是试图限制帐户和组的功能是。 不是实际上可能停止使用这些的凭据，以获取对任何安全资源访问从为强大的凭据有权决定的用户。  

### <a name="built-in-privileged-accounts-and-groups"></a>内置特权帐户和组  
Active Directory 旨在帮助管理委派和原则中指定的权利和权限最低权限。 拥有帐户 Active Directory 域中的"常规"用户在默认情况下，无法读取大部分目录中存储的内容，但无法更改仅非常有限的目录中的数据集。 需要额外的权限的用户可以授予内置目录，以便他们可以执行与他们的角色相关的特定的任务，但无法执行的任务不是为其关税相关的各种特权组中的成员。  

在 Active Directory，也有三种内置组构成目录中的最高权限组： 企业管理员 (EA) 组、 域管理员 (DA) 组中，并内置管理员 （栏） 组。  

第四个分组、 方案管理员 （索托） 组中，有的权限，如果滥用，可能会损坏或销毁整个的 Active Directory 森林，但此组详细受到的限制中其功能比 EA、 DA 和栏组。  

除了这些四个组中，有大量的其他内置默认帐户和中 Active Directory，其中的每个被授予权限和允许特定的管理任务，以执行的权限的组。 此附录不提供的 Active Directory 中每个内置或默认组详细的讨论，虽然它提供了组以及在你安装是最有可能看到的帐户。  

例如，如果你 Active Directory 森林中安装了 Microsoft Exchange Server，其他帐户和组可以创建在你的域中的内置功能和用户容器。 此附录描述组和基于本机角色和功能的 Active Directory 中的内置功能和用户容器中创建的帐户。 帐户和组创建的安装企业软件将不包括。  

#### <a name="enterprise-admins"></a>企业管理员  
企业管理员 (EA) 组位于森林根域中，并且默认情况下，它是内置的管理员组森林中的每个域中的成员。 森林根域中的内置管理员帐户是唯一默认的 EA 组成员。 EAs 被授予权限，使它们影响森林范围内的更改。 以下是会影响森林，如添加或删除域、 建立森林信任，或者提高森林功能级别中的所有域的更改。 在正常设计和实施委派模式中，EA 会员，才能仅当首次连建造森林或进行某些树林更改，例如建立站森林信任时。  

EA 组位于在用户容器森林根域中，在默认情况下，它是安全通用组中，除非森林根域搭载 Windows 2000 Server 混合模式下，用例组处于全局安全组中。 尽管某些权利直接授予 EA 组，但许多此组权利实际上由继承 EA 组因为它是管理员组森林中的每个域中的成员。 企业管理员已工作站或成员服务器没有默认权利。  

#### <a name="domain-admins"></a>域管理员  
森林中的每个域具有其自己的域管理员 (DA) 组中，即组成员的域的内置管理员 （栏） 除了已加入域的每台计算机上本地的成员。 唯一的域的 DA 组默认成员是该域内置管理员帐户。  

虽然 EAs 树林特权，DAs 是功能全面内它们的域。 在正常设计和实施委派模式中，应仅在"中断玻璃"方案中，其中的情况下在其中需要具有高级别权限的每台计算机在域中的帐户时，或在某些域宽必须更改所需 DA 会员。 尽管本机 Active Directory 委派机制允许的范围内就可能仅在紧急情况下使用 DA 帐户委派，连建造有效委派型号可能会很耗时，并许多组织都使用第三方应用以加快此过程。  

DA 组是位于域用户容器全局安全组。 每个域树林中的一个 DA 组，并且仅默认成员 DA 组的是域内置管理员帐户。 由于某个域 DA 组嵌入的域栏组和每个已加入域的系统的本地管理员组中，DAs 不仅有专门授予域管理员权限，但它们也将文件沿用文件所有的权利和的域管理员组和加入域的所有系统上的本地管理员组授予权限。  

#### <a name="administrators"></a>管理员  
内置的管理员 （栏） 组是域本地组某个域内置容器到其中嵌套 DAs 和 EAs 时，它是授予许多直接的权利和目录中和域控制器上的权限此组中。 但是，管理员组域没有任何特权或工作站成员服务器上。 域加入计算机的本地管理员组中的成员是其中本地特权被授予;并且讨论组，仅 DAs 所有已加入域的计算机的默认情况下的本地管理员组中的成员。  

管理员组是在域内置容器地域的组。 默认情况下，每个域栏组包含地域内置管理员帐户、 地域 DA 组中，并森林根域 EA 组。 专为管理员组不适用于 EAs 或 DAs 授予许多用户权限 Active Directory 中和域控制器上。 域栏组授予完全控制的大多数 directory 对象权限，并且可以 directory 对象的所有权。 EA 和 DA 组的权限某些特定于对象森林和域中，但许多 the power of 组实际上"从"其栏组中的成员。  

> [!NOTE]  
> 尽管这些这些特权组中的默认配置的任一三个组成员可以操作以获得任何其他组成员资格目录。 在某些情况下，能够轻而易举实现，而在其他人，它是更加困难，但从潜在特权的角度而言，这三组应该考虑相当有效。  

#### <a name="schema-admins"></a>方案管理员  
方案管理员 （索托） 组是一种通用组森林根域中的，并作为默认成员，类似于 EA 组已只有该域内置管理员帐户。 尽管索托组中的成员，则攻击危害 Active Directory 方案，这是整个 Active Directory 林框架，SAs 将拥有一些默认的权利和超出方案的权限。  

您应该仔细管理和监控会员索托组中，但在某些方面，该组"低限制"最高的三个比特权因为其特权范围是非常窄; 之前所述的组也就是说，SAs 具有任意位置以外的方案未管理权限。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>其他内置和 Active Directory 中的默认组  
若要协助委派目录中的管理，Active Directory 附带已被授予权限特定的各种内置和默认组。 下表中简要介绍了这些组。  

下表列出了 Active Directory 中的内置功能和默认组。 这两组组存在默认设置。但是，内置组位于 （默认） 中内置容器在 Active Directory 时的默认组 （默认） 放在用户容器 Active Directory 中。 内置容器中的组是所有域本地组中，在用户容器组都是本地域，世界各地，世界组中，除了三个每个用户帐户 （管理员，来宾，Krbtgt） 的组合。  

某些内置和默认帐户和组授予前面所述此附录最高权限组中，除了更高的权限和应还会保护并且仅在安全管理主机上使用。 可以找到这些组和帐户中显示为灰色行表 B-1： 内置默认和 Active Directory 中的帐户。 因为某些的这些组和帐户被授予权限，可以被滥用，从而危及 Active Directory 或域控制器，它们也为其他保护中所述[附录 c： 保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1： 内置默认帐户和 Active Directory 中的组  

||||  
|-|-|-|  
|**帐户或一组**|**默认容器、 组范围，然后键入**|**描述和默认用户权限**|  
|访问控制协助运营商 (在 Windows Server 2012 的 Active Directory)|内置容器<br /><br />域本地安全组|此组成员可以远程查询授权属性和这台计算机上的资源的权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|帐户运营商|内置容器<br /><br />域本地安全组|成员可以管理域用户和组帐户。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|管理员帐户|用户容器<br /><br />不是一组|管理域内置的帐户。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />调整进程的内存配额<br /><br />允许本地登录<br /><br />允许远程桌面服务通过登录<br /><br />备份文件和目录<br /><br />绕过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全球对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />允许受信任委派计算机和用户帐户<br /><br />从系统远程强制关机<br /><br />模拟后身份验证的客户端<br /><br />增加进程工作集<br /><br />增加计划优先级<br /><br />安装和卸载设备驱动程序<br /><br />作为批量作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行音量维护任务<br /><br />个人资料单个进程<br /><br />个人资料系统性能<br /><br />计算机中删除拓展坞<br /><br />还原文件和目录<br /><br />关机系统<br /><br />获得文件或其他对象的所有权|  
|管理员组|内置容器<br /><br />域本地安全组|管理员可以域完整且不受限制的访问。<br /><br />**直接用户权利：**<br /><br />访问网络从这台计算机<br /><br />调整进程的内存配额<br /><br />允许本地登录<br /><br />允许远程桌面服务通过登录<br /><br />备份文件和目录<br /><br />绕过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全球对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />允许受信任委派计算机和用户帐户<br /><br />从系统远程强制关机<br /><br />模拟后身份验证的客户端<br /><br />增加计划优先级<br /><br />安装和卸载设备驱动程序<br /><br />作为批量作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行音量维护任务<br /><br />个人资料单个进程<br /><br />个人资料系统性能<br /><br />计算机中删除拓展坞<br /><br />还原文件和目录<br /><br />关机系统<br /><br />获得文件或其他对象的所有权<br /><br />继承的用户权利：<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|允许的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员，可以让其复制到所有仅阅读域控制器在域中的密码。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|备份运营商|内置容器<br /><br />域本地安全组|备份运营商可以还原文件或备份的目的替代安全限制。<br /><br />**直接用户权利：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />作为批量作业登录<br /><br />还原文件和目录<br /><br />关机系统<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|证书发布者|用户容器<br /><br />域本地安全组|允许此组成员发布到的目录证书。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|证书服务 DCOM 访问|内置容器<br /><br />域本地安全组|如果证书服务域控制器 （不推荐） 上安装此组允许 DCOM 注册访问域用户和域计算机。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|克隆域控制器 (在 Windows Server 2012AD DS 广告 DS)|用户容器<br /><br />全球安全组|该组的有可能复制域控制器的成员。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|加密运营商|内置容器<br /><br />域本地安全组|成员权执行加密操作。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|调试程序用户|这是简默认设置以及内置组中，但当在广告 DS 存在，进一步调查的原因。|调试程序用户组存在指示，调试工具上已安装在有些时候，系统是否，可以通过 Visual Studio、 SQL、 Office、 或其他应用程序需要和支持调试环境。 此组允许远程访问调试计算机。 当此组存在域级别时，表示调试程序或包含调试程序中的应用程序已安装程序的域控制器上。|  
|已拒绝的 RODC 密码复制组|用户容器<br /><br />域本地安全组|此组中的成员能他们复制到任何只读域控制器在域中的密码。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|DHCP 管理员|用户容器<br /><br />域本地安全组|此组成员可以管理访问 DHCP 服务器的服务。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|DHCP 用户|用户容器<br /><br />域本地安全组|此组成员可以仅查看访问 DHCP 服务器的服务。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|分布式的 COM 用户|内置容器<br /><br />域本地安全组|允许此组成员的启动、 激活和这台计算机上使用分布式的 COM 对象。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|DnsAdmins|用户容器<br /><br />域本地安全组|此组成员可以管理访问 DNS 服务器服务。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|DnsUpdateProxy|用户容器<br /><br />全球安全组|此组成员是允许执行代表无法本身执行的动态更新的客户端的动态更新 DNS 客户端。 此组成员通常是 DHCP 服务器。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|域管理员|用户容器<br /><br />全球安全组|指定的域; 管理员域管理员的每个已加入域的计算机的本地管理员组成员，并且收到的权利和的本地管理员组中，除了域管理员组授予权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />调整进程的内存配额<br /><br />允许本地登录<br /><br />允许远程桌面服务通过登录<br /><br />备份文件和目录<br /><br />绕过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全球对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />允许受信任委派计算机和用户帐户<br /><br />从系统远程强制关机<br /><br />模拟后身份验证的客户端<br /><br />增加进程工作集<br /><br />增加计划优先级<br /><br />安装和卸载设备驱动程序<br /><br />作为批量作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行音量维护任务<br /><br />个人资料单个进程<br /><br />个人资料系统性能<br /><br />计算机中删除拓展坞<br /><br />还原文件和目录<br /><br />关机系统<br /><br />获得文件或其他对象的所有权|  
|域计算机|用户容器<br /><br />全球安全组|所有工作站和加入域的服务器都是默认此组成员。<br /><br />**默认直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|域控制器|用户容器<br /><br />全球安全组|在域中的所有域控制器。 注意： 域控制器未域计算机组中的成员。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|域来宾|用户容器<br /><br />全球安全组|所有来宾域中<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|用户域|用户容器<br /><br />全球安全组|在域中的所有用户<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|企业管理员 （存在仅在森林根域中）|用户容器<br /><br />通用安全组|企业管理员才有权更改树林配置设置;企业管理员是每个域管理员组中的成员，并且接收版权，然后为该组授予权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />调整进程的内存配额<br /><br />允许本地登录<br /><br />允许远程桌面服务通过登录<br /><br />备份文件和目录<br /><br />绕过遍历检查<br /><br />更改系统时间<br /><br />更改时区<br /><br />创建页面文件<br /><br />创建全球对象<br /><br />创建符号链接<br /><br />调试程序<br /><br />允许受信任委派计算机和用户帐户<br /><br />从系统远程强制关机<br /><br />模拟后身份验证的客户端<br /><br />增加进程工作集<br /><br />增加计划优先级<br /><br />安装和卸载设备驱动程序<br /><br />作为批量作业登录<br /><br />管理审核和安全日志<br /><br />修改固件环境值<br /><br />执行音量维护任务<br /><br />个人资料单个进程<br /><br />个人资料系统性能<br /><br />计算机中删除拓展坞<br /><br />还原文件和目录<br /><br />关机系统<br /><br />获得文件或其他对象的所有权|  
|企业仅阅读域控制器|用户容器<br /><br />通用安全组|此组包含森林中的所有仅阅读域控制器的帐户。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|事件日志阅读器|内置容器<br /><br />域本地安全组|在此组成员可以阅读域控制器上的事件日志。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|组策略 Creator 所有者|用户容器<br /><br />全球安全组|此组成员可以创建和修改域中的组策略对象。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|访客|用户容器<br /><br />不是一组|这是不具有添加到其访问令牌验证用户 SID 广告 DS 域中唯一的帐户。 因此，配置授予访问权限的用户身份验证组任何资源将无法访问该帐户。 此问题不是如此域来宾和来宾组成员的映像，但是-的这些组成员有验证用户 SID 添加到他们访问的标记。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|来宾|内置容器<br /><br />域本地安全组|来宾具有相同的访问权限的用户组成员作为默认情况下，除来宾帐户外的进一步限制，如上文所述。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|Hyper V 管理员 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组成员具有 HYPER-V 的所有功能的完整和无限制的访问权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|IIS_IUSRS|内置容器<br /><br />域本地安全组|Internet 信息服务使用内置的组。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|传入森林信任制造商 （存在仅在森林根域中）|内置容器<br /><br />域本地安全组|此组成员可以创建传入的单向信任到这片森林。 （站林信任创建保留的企业管理员中）。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|Krbtgt|用户容器<br /><br />不是一组|Krbtgt 帐户是在域中 Kerberos 密钥 Distribution 中心服务帐户。 此帐户有权访问 Active Directory 中存储的所有帐户凭据。 此帐户默认处于禁用状态，并且应永远不会启用<br /><br />**用户版权：**不适用|  
|网络配置运营商|内置容器<br /><br />域本地安全组|此组成员授予权限，使它们管理配置的网络功能。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|性能日志用户|内置容器<br /><br />域本地安全组|此组成员可以计划的性能计数器日志、 启用跟踪提供程序并到计算机通过远程访问本地收集跟踪事件。<br /><br />**直接用户权利：**<br /><br />作为批量作业登录<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|性能显示器用户|内置容器<br /><br />域本地安全组|此组成员可以在本地或远程访问性能计数器数据。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|Windows 2000 兼容访问|内置容器<br /><br />域本地安全组|此组存在 Windows 2000 Server、 之前的操作系统的后向兼容性，并提供成员阅读用户和组域中的信息的功能。<br /><br />**直接用户权利：**<br /><br />访问网络从这台计算机<br /><br />绕过遍历检查<br /><br />**继承的用户权利：**<br /><br />添加工作站到域<br /><br />增加进程工作集|  
|打印运营商|内置容器<br /><br />域本地安全组|此组成员可以管理域打印机。<br /><br />**直接用户权利：**<br /><br />允许本地登录<br /><br />安装和卸载设备驱动程序<br /><br />关机系统<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|RAS 和 IAS 服务器|用户容器<br /><br />域本地安全组|此组中的服务器可以阅读远程访问属性域中的用户帐户。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|RDS 端点服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器用户 RemoteApp 程序和个人的虚拟桌面了运行虚拟机和主机会话运行。 此组需要填充运行远程桌面连接的代理服务器上。 远程桌面会话主服务器和部署中使用的第虚拟化主服务器需要此组中。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|RDS 管理服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器可以运行远程桌面服务的服务器上执行日常的管理操作。 此组需要填充远程桌面服务部署中的所有服务器上。 此组中，必须包含运行 RDS 集中管理服务的服务器。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|RDS 远程访问服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组中的服务器使 RemoteApp 程序和个人的虚拟桌面访问这些资源的用户。 面向 Internet 部署在这些服务器通常在 edge 网络部署。 此组需要填充运行远程桌面连接的代理服务器上。 远程桌面网关服务器和部署中使用的第访问 Web 服务器需要此组中。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|仅阅读域控制器|用户容器<br /><br />全球安全组|此组包含在域中的所有仅阅读域控制器。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|远程桌面服务用户|内置容器<br /><br />域本地安全组|此组成员被授予使用远程 RDP 登录的权利。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|管理远程服务器 (Windows Server 2012)|内置容器<br /><br />域本地安全组|此组成员可以通过 （如 Ws-management 通过 Windows 远程管理服务) 管理协议访问 WMI 资源。 这仅适用于 WMI 命名空间的用户授予访问权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|复制程序|内置容器<br /><br />域本地安全组|在某个域中支持旧文件复制。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|方案管理员 （存在仅在森林根域中）|用户容器<br /><br />通用安全组|方案管理员是可以进行修改 Active Directory 方案，并且仅当模式仅用户可写。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|服务器运营商|内置容器<br /><br />域本地安全组|此组成员可以管理域服务器。<br /><br />**直接用户权利：**<br /><br />允许本地登录<br /><br />备份文件和目录<br /><br />更改系统时间<br /><br />更改时区<br /><br />从系统远程强制关机<br /><br />还原文件和目录<br /><br />关机系统<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|终端服务器许可服务器|内置容器<br /><br />域本地安全组|此组成员可以提供有关许可证发放，对于跟踪和报告 TS 每个用户 CAL 使用率信息更新中 Active Directory 的用户帐户<br /><br />**默认直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|用户|内置容器<br /><br />域本地安全组|用户必须允许他们阅读许多对象中和特性 Active Directory，尽管它们不能更改大多数的权限。 用户无法进行意外或有意系统范围内的更改，并可以运行大多数应用程序。<br /><br />**直接用户权利：**<br /><br />增加进程工作集<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查|  
|Windows 授权的访问权限的组|内置容器<br /><br />域本地安全组|此组成员的用户的对象有权访问计算的 tokenGroupsGlobalAndUniversal 属性<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  
|WinRMRemoteWMIUsers_ (Windows Server 2012)|用户容器<br /><br />域本地安全组|此组成员可以通过 （如 Ws-management 通过 Windows 远程管理服务) 管理协议访问 WMI 资源。 这仅适用于 WMI 命名空间的用户授予访问权限。<br /><br />**直接用户权利：**无<br /><br />**继承的用户权利：**<br /><br />访问网络从这台计算机<br /><br />添加工作站到域<br /><br />绕过遍历检查<br /><br />增加进程工作集|  

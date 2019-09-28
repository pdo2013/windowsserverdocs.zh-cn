---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: 动态访问控制概述
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 343e51f113f54c3965ef45d49f5d8fd64c260991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357509"
---
# <a name="dynamic-access-control-overview"></a>动态访问控制概述

>适用于：Windows Server 2012 R2、Windows Server 2012

此主题面向 IT 专业人员，概要介绍了在 Windows Server 2012 和 Windows 8 中引入的动态访问控制及其关联元素。  
  
基于域的动态访问控制允许管理员申请访问控制权限和基于定义良好的规则的限制，这些限制可能包括资源的敏感度、使用者的工作或角色和用于访问这些资源的设备的配置。  
  
例如，用户从办公室计算机访问资源，与通过虚拟专用网使用便携计算机进行访问时有着不同的权限。 或者，仅当设备满足由网络管理员定义的安全需求时，才允许访问。 当使用动态访问控制时，如果用户的工作或角色发生更改，则用户的权限会动态更改而无需额外的管理员干预（这会导致在 AD DS 中对用户的帐户属性进行更改）。  
  
Windows Server 2012 和 Windows 8 之前的 Windows 操作系统不支持动态控制访问。 在运行受支持和不受支持的 Windows 版本的环境中配置动态访问控制时，仅受支持的版本将实施所做更改。  
  
与动态访问控制关联的功能与概念包括：  
  
-   [中心访问规则](#BKMK_Rules)  
  
-   [中心访问策略](#BKMK_Policies)  
  
-   [支付](#BKMK_Claims)  
  
-   [表达](#BKMK_Expressions2)  
  
-   [建议的权限](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>中心访问规则  
中心访问规则是一个关于授权规则的表达，这个规则可能包括一个或多个涉及用户组、用户声明、设备声明和资源属性的条件。 可以将多个中心访问规则合并为一个中心访问策略。  
  
如果给一个域定义了一条或多条中心访问规则，文件共享管理员将特定的规则匹配给特定的资源和业务要求。  
  
### <a name="BKMK_Policies"></a>中心访问策略  
中央访问策略是包含条件表达式的授权策略。 例如，假设组织需要将文件中的个人身份信息（PII）的访问权限限制为仅允许查看 PII 信息的人力资源（HR）部门的文件所有者和成员。 这表示组织范围的策略适用于整个组织内在文件服务器上的任意位置的 PII 文件。 要实施这个策略，组织必须能够：  
  
-   确定并标记包含 PII 的文件。  
  
-   确定允许查看 PII 信息的人力资源组成员。  
  
-   将此中心访问策略添加到中心访问规则，然后将此中心访问规则应用到整个组织内在任何文件服务器上的所有包含 PII 的文件。  
  
中央访问策略可用作组织应用于其内部所有服务器的安全保护伞。 这些策略是应用于文件和文件夹的本地访问策略或自定义访问控制列表 (DACL) 的补充（并非替代）。  
  
### <a name="BKMK_Claims"></a>支付  
声明是关于域控制器发布的用户、设备或资源的一则独特信息。 用户的职位、文件的部门分类或计算机的健康状况都是声明的有效示例。 一个实体可以涉及不止一个声明，而声明的任何组合都可以被用来授权对资源的访问。 在受支持的 Windows 版本中提供了下列类型的声明︰  
  
-   **用户声明**：与某一特定用户关联的 Active Directory 属性。  
  
-   **设备声明**：与某一特定计算机对象关联的 Active Directory 属性。  
  
-   **资源属性**：被标记用于授权决策并在 Active Directory 中发布的全球资源属性。  
  
声明使管理员能够作出准确的组织，或企业范围内，关于用户、设备和资源的声明，它们可被纳入表达式、规则和策略当中。  
  
### <a name="BKMK_Expressions2"></a>表达  
条件表达式是访问控制管理的增强，它仅在特定条件满足的时候允许或拒绝对资源的访问，例如，组成员身份、位置或设备的安全性状态。 表达式通过 ACL 编辑器的高级安全设置对话框或 Active Directory 管理中心 (ADAC) 的中心访问规编辑器进行管理。  
  
表达式帮助管理员在日益复杂的公司环境中，使用灵活的条件管理对敏感性资源的访问。  
  
### <a name="BKMK_Permissions2"></a>建议的权限  
计划权限允许管理员更精确地塑造访问控制设置潜在变化的影响，而无需实际改变它们。  
  
预测对资源的有效访问有助于你在实施那些更改之前，为那些资源计划和配置权限。  
  
## <a name="additional-changes"></a>更多变化  
受支持的 Windows 版本中支持动态访问控制的其他增强功能包括：  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>支持Kerberos 身份验证协议以可靠地提供用户声明、设备声明和设备组。  
默认情况下，运行任何受支持 Windows 版本的设备都能够处理与动态访问控制关联的 Kerberos 票据，它包含有复合身份验证所需的数据。 域控制器能够以复合身份验证相关的信息发布和响应 Kerberos 票据。 当一个域被配置为识别动态访问控制时，设备在初始身份验证过程中从域控制器那里接收声明，并且它们在提交服务票证请求时，接收复合的身份验证票据。 复合身份验证会导致访问令牌包含有识别动态访问控制的资源上的用户和设备身份。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>支持使用密钥发行中心 (KDC) 组策略设置为域启用动态访问控制。  
每个域控制器都需要有同样的管理模板策略设置，它位于**计算机配置\策略\管理模板\系统\KDC\支持动态访问控制和 Kerberos 保护**。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>支持使用密钥发行中心 (KDC) 组策略设置为域启用动态访问控制。  
每个域控制器都需要有同样的管理模板策略设置，它位于**计算机配置\策略\管理模板\系统\KDC\支持动态访问控制和 Kerberos 保护**。  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>支持在 Active Directory 中存储用户和设备声明、资源属性和中心访问策略对象。  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>支持使用组策略部署中心访问策略对象。  
以下组策略设置使你能够将中心访问策略对象部署到组织中的文件服务器：**Computer Configuration\Policies\ Windows 设置 \ 安全设置 Settings\File System\Central 访问策略**。  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>支持通过使用组策略和全局对象访问审核对文件系统进行基于声明的文件授权和审核  
你必须通过使用计划权限，允许阶段性的中心访问策略审核，审核对中心访问策略的有效访问。 在组策略对象 (GPO) 的“**安全设置**”中的“**高级审核策略配置**”下，为计算机配置这一设置。 在 GPO 中配置安全设置之后，可将 GPO 部署到网络中的计算机上。  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>支持转换或筛选遍历 Active Directory 林信任的声明策略对象  
你可以筛选或转换传入或传出的遍历林信任的声明。 对于筛选或转换声明，有三种基本方案：  
  
-   **基于值的筛选**：筛选器可建立在声明的值的基础上。 这允许受信任林阻止某些特定值的声明被发送到信任林。 信任林中的域控制器可使用基于值的筛选，通过用来自受信任林的特定值筛选收到的声明，防止权限提升攻击。  
  
-   **基于声明类型的筛选**：基于声明类型而不是声明值的筛选器。 通过声明的名称识别声明类型。 你可以使用受信任林中基于声明类型的筛选阻止 Windows 发送向信任林披露信息的声明。  
  
-   **基于声明类型的转换**：在将声明发送给预定目标之前对其进行操作。 使用受信任林中基于声明类型的转换，生成一个包含特定信息的已知声明。 你可以使用转换生成声明类型、声明值或同时生成两者。  
  
## <a name="software-requirements"></a>软件要求  
由于动态访问控制的声明和复合身份验证要求有 Kerberos 身份验证扩展，因此，任何支持动态访问控制的域必须有足够的运行受支持 Windows 版本的域控制器，以支持来自动态访问控制感知的 Kerberos 客户端的身份验证。 默认情况下，设备必须使用其他站点中的域控制器。 如果这样的域控制器都不可用，身份验证将会失败。 因此，你必须支持下列情况中的一种：  
  
-   支持动态访问控制的每个域必须有足够的运行受支持 Windows 版本的域控制器，以支持来自运行受支持 Windows 或 Windows Server 版本的所有设备的身份验证。  
  
-   运行受支持 Windows 版本的设备或没有通过使用声明或复合识别保护资源的设备，应当对动态访问控制禁用 Kerberos 协议支持。  
  
对支持用户声明的域，每个运行受支持 Windows Server 版本的域控制器必须用合适的设置进行配置，以支持声明和复合身份验证，并提供 Kerberos 防御。 像下面这样配置“KDC 管理模板”策略中的设置：  
  
-   **始终提供声明**：如果所有域控制器都运行受支持的 Windows Server 版本，请使用这一设置。 此外，需要将域功能级别设置为 Windows Server 2012 或更高版本。  
  
-   **受支持**：当你使用这一设置时，请监视域控制器，以确保运行受支持 Windows Server 版本的域控制器数量，对于需要用来访问受“动态访问控制”保护的资源的客户端计算机的数量来说，是足够的。  
  
如果用户域和文件服务器域位于不同的林中，则必须在 Windows Server 2012 或更高版本的功能级别上设置文件服务器林根中的所有域控制器。  
  
如果用户不能识别“动态访问控制”，那么在两个林之间应该有双向信任关系。  
  
如果在声明离开林时转换声明，则必须在 Windows Server 2012 或更高版本的功能级别设置用户林根中的所有域控制器。  
  
运行 Windows Server 2012 或 Windows Server 2012 R2 的文件服务器必须具有一个组策略设置，该设置指定是否需要为不携带声明的用户令牌获取用户声明。 这一设置默认设为“**自动**”，这会导致当存在包含该文件服务器的用户和/或设备声明的中心策略时，“组策略”设置被“**开启**”。 如果文件服务器包含了包括用户声明的自定义 ACL，则必须将这个“组策略”设置为“**开启**”，这样服务器就知道代表访问服务器时不提供声明的用户请求声明。  
  
## <a name="additional-resource"></a>其他资源  
有关基于此技术实施解决方案的信息，请参阅 @no__t 0Dynamic Access Control：方案概述 @ no__t。  
  



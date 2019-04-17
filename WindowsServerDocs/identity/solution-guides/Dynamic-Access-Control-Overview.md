---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: "动态访问控制概述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5cf74042c9b511abb1fbeb88224dea0c7f2c8706
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="dynamic-access-control-overview"></a>动态访问控制概述

>适用于：Windows Server 2012 R2、Windows Server 2012

IT 专业人员的此概述主题介绍了动态访问控制及其关联，Windows Server 2012 和 Windows 8 中引入了元素。  
  
基于域动态访问控制使管理员可以访问控制权限和基于可能包括资源，作业或的角色用户，以及用于访问这些资源设备的配置敏感度的清晰规则限制将应用。  
  
例如，用户可能具有不同的权限，当他们使用的是笔记本电脑计算机在虚拟专用网络资源访问从与他们 office 计算机时。 或者可能会允许设备的唯一如果满足定义的网络管理员的安全要求的访问。 动态访问控制使用时，用户的权限动态更改无需额外管理员参与用户的工作或角色变动（导致更改用户帐户中的属性广告 DS）。  
  
动态访问控制之前 Windows Server 2012 和 Windows 8 的 Windows 操作系统中不支持。 当动态访问控制配置支持和非受支持版本的 Windows 的环境中时，仅受支持的版本将实现所做的更改。  
  
功能和概念，与动态访问控制包括：  
  
-   [中央使用规则](#BKMK_Rules)  
  
-   [中央访问策略](#BKMK_Policies)  
  
-   [索赔](#BKMK_Claims)  
  
-   [表情](#BKMK_Expressions2)  
  
-   [提出的权限](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>中央使用规则  
中央访问规则是将授权规则可以包含一个或多个用户组、用户索赔，设备索赔和资源属性涉及的条件。 可以将多个中心访问规则合并到中心访问策略。  
  
如果一个或多个中心访问规则已定义到某个域，文件共享管理员可以匹配特定规则与特定资源和业务需求。  
  
### <a name="BKMK_Policies"></a>中央访问策略  
中央访问策略是授权包含条件表情。 例如，假设的组织有业务要求限制访问的个人身份信息 (PII) 中对文件所有者和允许查看 PII 信息人工 (HR) 部成员文件。 这代表适用于 PII 文件中，它们位于文件服务器整个组织随时组织范围的策略。 若要实现此策略，需要将无法为组织：  
  
-   找出并标记包含 PII 的文件。  
  
-   标识小时成员可以查看 PII 信息的组。  
  
-   向中心访问规则中, 添加中心访问策略和将中心访问规则应用于所有文件包含 PII，位于何处十分文件服务器整个组织。  
  
中央访问策略作为组织适用于其服务器的安全 umbrellas。 这些策略于是（但不要替换）本地访问策略或随机访问控件列表 (Dacl) 应用的文件和文件夹。  
  
### <a name="BKMK_Claims"></a>索赔  
声明是信息的一个唯一的一的用户、设备或已由域控制器发布的资源。 用户的标题、部门分类的文件或计算机的运行状况的有效的示例包括的索赔。 实体可能会涉及多个索赔，并可以使用的索赔任意组合授权的访问权限的资源。 以下类型的索赔是适用于受支持版本的 Windows:  
  
-   **用户索赔**与特定用户的 Active Directory 属性。  
  
-   **设备索赔**与特定计算机对象关联的 Active Directory 属性。  
  
-   **资源属性**全局资源属性，标记为授权决定在使用 Active Directory 中发布。  
  
索赔使管理员进行准确的组织或企业版范围明细表有关用户、设备和即可合并的资源表情、规则和策略中。  
  
### <a name="BKMK_Expressions2"></a>表情  
条件表情都增强到访问控制管理允许或拒绝访问资源仅在某些情况时，例如组成员、位置或设备的安全状态。 通过 ACL 编辑器或中央访问规则编辑器 Active Directory 管理中心 (ADAC) 中的高级安全设置对话框中，表情进行管理。  
  
表情帮助管理访问灵活条件越来越复杂企业环境中的敏感资源的管理员。  
  
### <a name="BKMK_Permissions2"></a>提出的权限  
提出的权限启用管理员才能够得更加准确型号潜在的更改，而无需实际上更改它们访问控制设置的影响。  
  
预测对资源有效的访问，可帮助你计划，并在实现这些更改之前配置这些资源的权限。  
  
## <a name="additional-changes"></a>其他更改  
在受支持版本的 Windows 支持动态访问控制其他增强功能包括：  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Kerberos 验证协议用户索赔、设备索赔和设备组可靠提供支持。  
默认情况下，设备运行的任何受支持版本的 Windows 可处理动态访问控制相关 Kerberos 票证，其中包括所需的复合身份验证数据。 域控制器都可以发出和响应 Kerberos 门票复合身份验证相关信息。 当某个域配置识别动态访问控制时，设备接收索赔域控制器期间的初始身份验证，并且提交服务票证请求时，他们会收到复合身份验证门票。 复合中包含的用户和设备上识别动态访问控制资源身份一个访问令牌身份验证结果。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>使用到某个域中启用动态访问控制键 Distribution 中心 (KDC) 组策略设置支持。  
每个域控制器需要具有位于同一管理模板策略设置**计算机配置 Templates\System\KDC\Support 动态访问控制和保护 Kerberos 作用**。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>使用到某个域中启用动态访问控制键 Distribution 中心 (KDC) 组策略设置支持。  
每个域控制器需要具有位于同一管理模板策略设置**计算机配置 Templates\System\KDC\Support 动态访问控制和保护 Kerberos 作用**。  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>在存储用户和设备索赔，资源属性中心访问策略对象 Active Directory 的支持。  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>使用组策略部署中心访问策略对象的支持。  
下面的组策略设置使您能够部署到文件服务器，在你的组织的中央访问策略对象：**计算机 Configuration\Policies\ Windows \ 设置 \ 文件 System\Central 访问策略**。  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>基于索赔文件授权和文件系统的审核通过使用组策略全球对象访问审核支持  
你必须启用分阶段中心访问策略审核审核中心访问策略有效访问使用提出的权限。 配置为在计算机此设置**高级审查策略配置**中**安全设置**组策略对象 (GPO)。 在 GPO 配置安全设置后，你可以在你的网络到计算机部署 GPO。  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>支持转换或筛选遍历 Active Directory 林信任的索赔策略对象  
你可以筛选或转换传入和传出遍历森林信任的索赔。 有三个基本筛选和转变索赔的方案：  
  
-   **基于值筛选**筛选器可根据的值的索赔。 这允许受信任的森林，以防发送到信任林某些值的索赔。 域控制器在信任森林可用于值基于筛选抵御特权提升攻击通过过滤从受信任的森林传入索赔具有特定的值。  
  
-   **声称类型基于筛选**Filters 根据类型的索赔，而不是声明值。 你可以通过声明的名称识别索赔类型。 使用筛选在受信任的树林中，键入基于索赔，它会阻止 Windows 发送披露信任林信息的索赔。  
  
-   **声称类型基于转换**操作之前将其发送到所需目标索赔。 使用受信任的树林中索赔类型基于转换一般化已知的索赔包含的具体信息。 你可以使用转换一般化声明类型、该声明值，或两者都。  
  
## <a name="software-requirements"></a>软件要求  
由于索赔和动态访问控制复合身份验证需要 Kerberos 身份验证扩展，支持动态访问控制任何域必须运行 Windows，以支持动态访问控制感知 Kerberos 客户端的身份验证的受支持的版本足够域控制器。 默认情况下，必须设备使用域控制器中的其他站点。 是否提供此类的域控制器，身份验证也将失败。 因此，你必须支持下列情况之一：  
  
-   支持动态访问控制每个域必须具有足够域控制器在运行 Windows Server 支持从所有设备运行的 Windows 或 Windows Server 的受支持的版本的身份验证的受支持的版本。  
  
-   运行的受支持的版本的 Windows 或该设备通过使用索赔或复合身份不保护资源，应禁用动态访问控制 Kerberos 协议支持。  
  
对于支持用户声明的域，每个运行 Windows server 的受支持的版本的域控制器必须的相应的设置，以支持索赔和复合身份验证，并提供 Kerberos 程度配置。 在 KDC 管理模板策略配置设置，如下所示：  
  
-   **始终提供索赔**使用此设置，如果所有域控制器都运行的 Windows Server 的受支持的版本。 此外，设置域功能级别，对 Windows Server 2012 或更高版本。  
  
-   **受支持**当你使用此设置，监视器域控制器，以确保是足够数需要访问受动态访问控制资源的客户端计算机的运行 Windows Server 的受支持的版本的域控制器的数量。  
  
如果用户域和文件服务器域不同森林，必须设置文件服务器的森林根中的所有域控制器在 Windows Server 2012 或更高版本的正常工作级别。  
  
如果客户端无法识别动态访问控制，必须有双向信任关系两个林之间。  
  
如果在离开森林时将其转换索赔，必须在 Windows Server 2012 或更高版本的正常工作级别设置所有用户的森林根域控制器。  
  
文件服务器，在运行 Windows Server 2012 或 Windows Server 2012 R2 必须组策略设置指出是否需要获取用户的用户令牌不带有索赔提起的索赔。 此设置已设置为默认**自动**，这会导致此组策略设置以打开**上**是否中央策略，其中包含该文件服务器的用户或设备的索赔。 如果文件服务器包含自由 Acl 包含用户索赔，你将需要此组策略设置为**上**以便服务器知道请求代表不提供索赔访问服务器时的用户的索赔。  
  
## <a name="additional-resource"></a>其他资源  
有关实现解决方案根据该技术的信息，请参阅[动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)。  
  



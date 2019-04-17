---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: "如何配置受保护的帐户"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4de7b1c9e3d12556f67c4515467bccd124e5e73e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>如何配置受保护的帐户

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

通过 Pass--哈希 (PtH) 攻击攻击可以通过使用了用户密码 （或其他凭据衍生） 的基础 NTLM 哈希验证到远程服务器或服务。 Microsoft 之前已[发布指南](https://www.microsoft.com/download/details.aspx?id=36036)缓解 pass 哈希攻击。  Windows Server 2012 R2 包括新功能以帮助减少进一步此类攻击。 有关其他帮助抵御凭据被盗的安全功能的详细信息，请参阅[凭据保护和管理](https://technet.microsoft.com/library/dn408190.aspx)。 本主题介绍如何配置以下新功能：

-   [受保护的用户](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [身份验证策略](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [身份验证策略思](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

有其他缓解内置于 Windows 8.1 和 Windows Server 2012 R2 来帮助防止凭据被盗涵盖以下主题：

-   [远程桌面版的限制的管理员模式](http://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [LSA 保护](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>受保护的用户
受保护的用户是的你可以添加新的或现有的用户新全球安全组。 Windows 8.1 的设备和 Windows Server 2012 R2 的主机已特殊的行为，以提供更好防御凭据被盗此组成员的。 对于的组成员，Windows 8.1 的设备或 Windows Server 2012 R2 主机不缓存的受保护的用户均不支持的凭据。 此组成员的有没有任何额外的保护，如果用户登录到运行 Windows 8.1 的以前版本的 Windows 设备。

谁有登录打开到 Windows 8.1 设备组的受保护的用户成员和 Windows Server 2012 R2 主机可以*不再*使用：

-   默认委派凭据 (CredSSP) 的凭据不会缓存的纯文本，即便**允许委派默认凭据**启用策略

-   Windows 摘要-不都凭据纯文本缓存甚至时启用它们

-   不会 NTLM-NTOWF 缓存

-   Kerberos 长期键-Kerberos 授权票证 (TGT) 登录时获取和无法自动重新获取

-   登录脱机-缓存的登录验证程序则不会创建

如果 Windows Server 2012 R2 域功能级别的组成员都可以不再：

-   通过使用 NTLM 身份验证

-   使用预 Kerberos 的身份验证数据加密 Standard (DES) 或 RC4 密码套件

-   使用不受约束或受限制委派委派

-   续订初始 4 小时整个使用期限内之外的用户门票 (Tgt)

若要添加的组的用户，你可以使用[UI 工具](https://technet.microsoft.com/library/cc753515.aspx)如 Active Directory 管理中心 (ADAC) 或 Active Directory 用户和计算机或命令行工具如[现有组](https://technet.microsoft.com/library/cc732423.aspx)，或 Windows PowerShell[添加 ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet。 帐户的服务和计算机*不应*成为保护用户组的成员。 这些帐户的会员提供没有本地保护由于密码或证书始终在主机上可用。

> [!WARNING]
> 身份验证限制有没有的解决方法，这意味着的如企业管理员组或域管理员组高权限的组成员遵循作为保护用户组中的其他成员同样的限制。 如果此类组中的所有成员都添加到保护用户组时，很可能会被锁定这些帐户的所有。直到彻底测试潜在的影响，你应永远不会添加到保护用户组高权限的所有帐户。

保护用户组成员必须能够通过 Kerberos 高级加密标准 (AES) 进行身份验证。 此方法 Active Directory 中帐户要求 AES 键。 内置管理员没有 AES 键，除非密码已更改后，域控制器上运行 Windows Server 2008 或更高版本。 此外，锁定具有密码，已在运行较早版本的 Windows Server 的域控制器的任何帐户。因此，请按照这些的最佳实践操作：

-   除非不测试在域中**所有域控制器都运行 Windows Server 2008 或更高版本**。

-   **更改密码**所有域帐户创建的*之前*创建域。 否则，无法身份验证这些帐户。

-   **更改密码**针对每个之前将该帐户添加到受保护的用户的用户进行分组或确保密码已更改最近在运行 Windows Server 2008 域控制器上或更高版本。

### <a name="BKMK_Prereq"></a>要求使用受保护的帐户
受保护的帐户具有以下部署要求：

-   若要提供保护的用户的客户端限制，主机必须运行 Windows 8.1 或 Windows Server 2012 R2。 仅，必须登录的组成员的受保护的用户帐户。 在此情况下，可以通过创建保护用户组[传输主域控制器 (PDC) 模拟器角色](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到运行 Windows Server 2012 R2 域控制器。 该组对象复制到其他域控制器后，可以运行较早版本的 Windows Server 的域控制器上托管 PDC 模拟器角色。

-   若要提供受保护的用户，该限制种身份验证的使用情况是，域控制器端限制以及其他限制，域功能级别必须是 Windows Server 2012 R2。 有关功能级别的详细信息，请参阅[了解 Active Directory 域服务 (广告 DS) 功能级别](../active-directory-functional-levels.md)。

### <a name="BKMK_TrubleshootingEvents"></a>解决有关保护用户的事件
本部分介绍了新的日志，以帮助解决事件相关的受保护的用户和保护用户可能会如何影响更改任一票证授予 (TGT) 票证过期或委派的问题进行疑难解答。

#### <a name="new-logs-for-protected-users"></a>对于受保护的用户新日志

两个新操作管理日志，可帮助解决事件相关的受保护的用户： 保护的用户的客户端日志和保护用户故障-域控制器日志。 这些新日志位于事件查看器中，并且默认处于禁用状态。 若要启用日志，请单击**应用程序和服务日志**，单击**Microsoft**，单击**Windows**，单击**身份验证**，并单击日志的名称，然后单击**操作**（或右键单击日志），然后单击**启用日志**。

有关这些日志中的事件的详细信息，请参阅[身份验证的策略和身份验证策略思](https://technet.microsoft.com/library/dn486813.aspx)。

#### <a name="troubleshoot-tgt-expiration"></a>疑难解答 TGT 到期
通常，域控制器设置 TGT 生命周期内和续订基于域策略，如下面的窗口组策略管理编辑器中所示。

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

对于**保护用户**，以下设置均硬编码：

-   最大的生命周期内用户票证： 240 分钟

-   最大的生命周期内用户票证续订： 240 分钟

#### <a name="troubleshoot-delegation-issues"></a>疑难解答委派问题
以前，如果时无法使用 Kerberos 委派的技术的客户端帐户检查以查看是否**敏感帐户，不能委派**设置。 但是，该帐户是否**保护用户**，它可能不具有此设置，配置在 Active Directory 管理中心 (ADAC)。 因此，检查设置和组成员解决委派问题时。

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>审核身份验证尝试
成员明确审核身份验证尝试**保护用户**组中，你可以继续安全日志审核事件，或收集的新操作管理日志中的数据。 有关这些事件的详细信息，请参阅[身份验证的策略和身份验证策略思](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>提供的服务和计算机 DC 侧保护
不能成员的帐户的服务和计算机**保护用户**。 此部分中介绍的域控制器基于保护可以提供的这些帐户：

-   拒绝 NTLM 身份验证： 只可以通过配置[NTLM 阻止策略](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

-   拒绝中 Kerberos 预先身份验证数据加密 Standard (DES): Windows Server 2012 R2 域控制器不接受 DES 计算机帐户，除非将其配置为 DES 只是因为与 Kerberos 发布的 Windows 的每个版本还支持 RC4。

-   在 Kerberos 预身份验证拒绝 RC4： 无法配置。

    > [!NOTE]
    > 尽管到可以[更改支持的加密类型配置](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)，最好不要无需测试的目标环境中更改这些设置为计算机帐户。

-   到初始 4 小时生存期限制用户门票 (Tgt): 使用身份验证的策略。

-   拒绝无约束或受限制委派与委派： 若要限制帐户，请打开 Active Directory 管理中心 (ADAC)，然后选择**敏感帐户，不能委派**复选框。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>身份验证策略
身份验证策略是包含身份验证策略对象的广告 DS 中的新容器。 身份验证策略可以指定帮助减少遭受凭据被盗，如限制 TGT 生命周期内的帐户，或者添加其他索赔相关的条件的设置。

在 Windows Server 2012、 动态访问控制引入了 Active Directory 森林范围对象类称为中央访问策略提供简单的方式，在组织中配置文件服务器。 在 Windows Server 2012 R2，新的对象类称为身份验证的策略 (对象类 msDS-AuthNPolicies) 可用于适用于 Windows Server 2012 R2 域中的帐户类的身份验证配置。 Active Directory 帐户类是：

-   用户

-   计算机

-   托管服务帐户和组托管服务帐户 (GMSA)

### <a name="quick-kerberos-refresher"></a>快速 Kerberos 刷新程序
三种类型的交易所，也称为子 Kerberos 身份验证协议包含：

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   身份验证服务 (AS) 交换 （KRB_AS_ *）

-   票证许可服务 (TGS) 交换 （KRB_TGS_ *）

-   客户端月服务器 (AP) 交换 （KRB_AP_ *）

AS exchange 是其中客户端使用该帐户的密码或密钥创建预 authenticator 若要申请票证许可票证 (TGT)。 这种情况下在用户登录或服务票证需要第一次。

TGS exchange 是其中的帐户 TGT 用于创建请求服务票证 authenticator。 需要验证的连接时，将发生这种情况。

接入点 exchange 发生作为通常作为内应用协议的数据，并且不会受到身份验证的策略。

有关详细信息，请参阅[Kerberos 版 5 身份验证协议的工作原理](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx)。

### <a name="overview"></a>概述
身份验证策略补充受保护的用户，通过提供适用于帐户的可配置限制的方法以及提供有关服务和计算机的帐户的限制。 执行期间 AS exchange 或 TGS 身份验证策略更换费用。

你可以通过配置限制初始身份验证或 AS exchange:

-   TGT 生命周期

-   访问控制条件用户登录，必须满足 AS exchange 即将从其设备的限制

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

你可以通过配置限制通过票证许可服务 (TGS) exchange 票证请求提供服务：

-   必须满足客户端 （用户、 服务、 计算机） 访问控制条件或从中 TGS exchange 即将的设备

### <a name="BKMK_ReqForAuthnPolicies"></a>使用认证策略要求

|策略|要求|
|----------|----------------|
|提供自定义 TGT 生存期| Windows Server 2012 R2 域正常工作的级别帐户域|
|限制用户登录|Windows Server 2012 R2 域正常工作的级别帐户域动态访问控制支持<br />Windows 8、 Windows 8.1、 Windows Server 2012 或 Windows Server 2012 R2 动态访问控制设备的支持|
|限制在用户帐户和安全组基于服务票证颁发| Windows Server 2012 R2 域功能级别资源域|
|限制服务票证颁发根据用户索赔或设备的帐户、 安全性组或索赔| Windows Server 2012 R2 域功能级别资源域动态访问控制支持|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>将一个用户帐户限制为特定设备和主机
使用管理员权限的高值帐户应的成员**保护用户**组。 默认情况下，帐户不属于**保护用户**组。 将帐户添加到组之前，配置域控制器支持，并创建审核策略以确保未阻止的问题。

#### <a name="configure-domain-controller-support"></a>配置域控制器支持

在 Windows Server 2012 R2 域功能级别 (DFL) 必须用户的帐户域。 确保所有域控制器 Windows Server 2012 R2，并使用 Active Directory 域和到信任[筹集 DFL](https://technet.microsoft.com/library/cc753104.aspx)对 Windows Server 2012 R2。

**若要配置动态访问控制的支持**

1.  在默认域控制器策略中，单击**启用**启用**密钥 Distribution 中心 (KDC) 客户端支持索赔、 复合身份验证和 Kerberos 程度**配置计算机 |管理模板 |系统 |KDC。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  下**选项**的下拉列表中，选择**始终提供索赔**。

    > [!NOTE]
    > **受支持**还配置，但因为域，Windows Server 2012 R2 DFL，始终遇到 Dc 提供索赔将允许用户索赔基于访问检查使用非索赔注意到设备时，会出现和承载连接到声明感知服务。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > 配置**失败 unarmored 身份验证请求**将导致从任何操作系统，都不支持 Kerberos 程度，如 Windows 7 和之前的操作系统或开始与 Windows 8，尚未显式配置为支持它的操作系统的身份验证失败。

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>使用 ADAC 创建用户帐户审查身份验证策略

1.  管理中心打开的 Active Directory (ADAC)。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > 所选**身份验证**节点时是在 Windows Server 2012 R2 DFL 域可见。 如果未显示节点，再试一次通过 Windows Server 2012 R2 DFL 位于某个域从域管理员帐户。

2.  单击**认证策略**，然后单击**新建**创建新的策略。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    身份验证策略必须显示名称，并且默认实施。

3.  若要创建一个仅审核策略，请单击**仅审核策略限制**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    身份验证策略应用根据 Active Directory 的帐户类型。 一个策略可以通过将应用于所有三种的帐户类型配置每种类型的设置。 帐户类型是：

    -   用户

    -   计算机

    -   托管的服务帐户和组托管服务帐户

    如果您已具有可用密钥 Distribution 中心 (KDC) 的新主体扩展方案，新的帐户类型进行分类从最近的派生的帐户类型。

4.  若要配置为用户帐户 TGT 生命周期内，请选择**指定授权票证有效期为用户帐户**复选框，输入在几分钟内的时间。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    例如，如果你希望 10 小时最长 TGT 生命周期，输入**600**如下所示。 如果没有 TGT 生存期配置，然后帐户是否**保护用户**分组，TGT 整个使用期限内，并且续订 4 个小时。 否则，TGT 生命周期内和续订基于域策略某个域，使用默认设置为以下组策略编辑器中管理窗口中看到。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  若要限制选定设备上的用户帐户，请单击**编辑**来定义的设备所需的条件。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  在**编辑访问控制条件**窗口中，单击**添加条件**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>添加计算机帐户或一组条件

1.  若要配置计算机帐户或多个组，下拉列表中，选择的下拉列表中的框**每个成员**将更改为**的任何成员**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > 此访问控制定义的条件的设备或用户在登录从中主机。 在访问控制术语、 设备或主计算机帐户是用户，因此**用户**是唯一的选项。

2.  单击**添加项目**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  若要更改对象类型，请单击**对象类型**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  单击以选择 Active Directory 对象的计算机，**计算机**，然后单击**确定**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  键入以限制用户，然后单击计算机的名称**检查名称**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  单击确定，并创建任何其他计算机帐户条件。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  完成后，然后单击**确定**和定义的条件将显示为计算机帐户。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>添加计算机索赔条件

1.  若要配置计算机索赔，下拉列表以选择声明的组。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    才可用他们已预配林中索赔。

2.  键入 OU 的名称，应限制为登录的用户帐户。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  完成后，然后单击确定，并框将显示所定义的条件。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>解决了缺少的计算机索赔
如果声明已预配，但不可用，它可能仅为配置**计算机**类。

假设你想要限制基于部门 (OU) 身份验证的计算机，已配置，但只**计算机**类。

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

此声明可用来限制用户登录到设备，请选择**用户**复选框。

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>配置使用 ADAC 身份验证策略的用户帐户

1.  从**用户**帐户，请单击**策略**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  选择**分配到此帐户的身份验证策略**复选框。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  然后选择要适用于用户身份验证策略。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>配置设备和主机上的动态访问控制支持
您可以配置 TGT 生存期不配置动态访问控制 (DAC)。 检查 AllowedToAuthenticateFrom 和 AllowedToAuthenticateTo 只需要 DAC。

使用组策略或本地组策略编辑器，启用**Kerberos 客户端支持索赔、 复合身份验证和 Kerberos 程度**配置计算机 |管理模板 |系统 |Kerberos:

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>解决了身份验证策略

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>确定直接分配身份验证策略帐户
身份验证的策略中的帐户部分显示直接在应用策略的帐户。

![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>使用身份验证的策略故障-域控制器管理日志
一个新**身份验证策略故障-域控制器**下的管理日志**应用程序和服务日志** > **Microsoft** > **Windows** > **身份验证**已创建以使其更易于发现认证策略由于失败。 默认情况下将禁用日志。 若要启用它，请右键单击日志名称，然后单击**启用日志**。 新的事件是非常相似内容中的现有 Kerberos TGT 和服务票证审核事件。 有关这些事件的详细信息，请参阅[身份验证的策略和身份验证策略思](https://technet.microsoft.com/library/dn486813.aspx)。

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>使用 Windows PowerShell 管理身份验证策略
此命令创建的名为身份验证策略**TestAuthenticationPolicy**。 **UserAllowedToAuthenticateFrom**参数指定从中用户可以通过在名为 someFile.txt 文件 SDDL 字符串身份验证的设备。

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

此命令获取所有身份验证策略与的筛选器，**筛选器**参数指定。

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

此命令修改描述和**UserTGTLifetimeMins**指定的身份验证策略属性。

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

此命令删除身份验证的策略，**身份**参数指定。

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

使用此命令**获取 ADAuthenticationPolicy**与 cmdlet**筛选器**参数获取就不会执行的所有认证策略。 结果集传送到**删除 ADAuthenticationPolicy** cmdlet。

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>身份验证策略思
身份验证策略思是广告 DS 的用户的电脑，服务帐户中的新容器 (对象类 msDS-AuthNPolicySilos)。 帮助保护高价值帐户。 所有组织都需要保护企业管理员域管理员，架构管理员组中的成员，因为这些帐户可能由攻击访问森林中的任何内容，而其他帐户可能还都需要保护。

通过创建是唯一给他们的帐户并将应用组策略设置以限制本地边远交互式登录和管理权限，某些组织隔离工作负载。 身份验证策略思补充通过创建一种方法来定义用户的计算机，托管的服务帐户之间的关系的此项工作。 帐户只能属于一个思。 你可以控制以便配置每种类型的帐户的身份验证策略：

1.  非续订 TGT 生命周期

2.  访问控制条件退回 TGT (注意： 由于 Kerberos 程度需要无法适用于系统)

3.  访问控制条件退回服务票证

此外，身份验证策略思帐户具有思索赔，，如文件服务器的声明感知资源可用于来控制访问权限。

新的安全描述符可以控制发行根据 service 票证配置：

-   用户、 用户的安全组，请和/或用户的声明

-   设备、 设备的安全组中，和/或设备的声明

获取此信息到资源的 Dc 需要动态访问控制：

-   用户索赔：

    -   Windows 8 和更高版本的客户端支持动态访问控制

    -   帐户域支持动态访问控制和索赔

-   设备和/或设备安全组成：

    -   Windows 8 和更高版本的客户端支持动态访问控制

    -   配置为复合身份验证的资源

-   设备索赔：

    -   Windows 8 和更高版本的客户端支持动态访问控制

    -   设备域支持动态访问控制和索赔

    -   配置为复合身份验证的资源

可对到个人帐户，而不是身份验证策略思的所有成员身份验证的策略或可对不同类型的帐户中思单独的身份验证的策略。 例如，一个身份验证的策略可以对高权限的用户帐户，应用和服务帐户可用于不同的策略。 在至少有一个身份验证的策略必须之前创建了身份验证策略思创建。

> [!NOTE]
> 它可以应用独立于思限制特定帐户范围或身份验证策略可以应用于身份验证策略思的成员。 例如，要保护单个帐户或一组少的帐户，策略可以设置这些帐户未将帐户添加到思。

你可以通过使用 Active Directory 管理中心或 Windows PowerShell 创建身份验证策略思。 默认情况下，身份验证策略思仅审核思策略，相当于指定**WhatIf** Windows PowerShell cmdlet 中的参数。 在此情况下，不适用于策略思限制，但产生审核指示是否如果会发生故障应用限制。

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>若要使用 Active Directory 管理中心创建身份验证策略思

1.  打开**Active Directory 管理中心**，单击**身份验证**，右键单击**身份验证策略思**，单击**新建**，然后单击**身份验证策略思**。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  在**显示名称**，为思键入一个名称。 在**允许帐户**，单击**添加**、 键入的帐户名称，然后单击**确定**。 你可以指定用户、 计算机或服务的帐户。 然后指定是否使用单个策略为所有主体或单独策略每种类型的用户，以及的策略或策略的名称。

    ![受保护的帐户](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>管理身份验证策略思通过使用 Windows PowerShell
此命令创建身份验证策略思对象并强制。

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

此命令获取的身份验证与由指定的筛选器策略思**筛选器**参数。 输出然后传递到**格式表**cmdlet 显示该策略和的值为的名称**强制**有关每个策略。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

使用此命令**获取 ADAuthenticationPolicySilo**与 cmdlet**筛选器**参数以获取所有不起作用的身份验证策略思和管道到筛选结果**删除 ADAuthenticationPolicySilo** cmdlet。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

此命令授予访问权限名为身份验证策略思*思*给用户帐户名为*User01*。

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

此命令吊销名为身份验证策略思访问*思*用户帐户名为*User01*。 因为**确认**参数设置为**$False**，不显示任何确认消息。

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

此示例中首次使用**获取 ADComputer** cmdlet 获取筛选器匹配的所有计算机帐户的**筛选器**参数指定。 输出的此命令传递到**组 ADAccountAuthenticatinPolicySilo**要给其指定身份验证策略思名为*思*和名为身份验证策略*AuthenticationPolicy02*访问它们。

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```




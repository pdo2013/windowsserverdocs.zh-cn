---
title: 如何配置受保护的帐户
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f2effdb7a82a25c810bc041475e760145de492d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864608"
---
# <a name="how-to-configure-protected-accounts"></a>如何配置受保护的帐户

>适用于：Windows 服务器 （半年频道），Windows Server 2016

通过哈希传递 (PtH) 攻击，攻击者可以使用用户密码（或其他凭据派生对象）的基础 NTLM 哈希针对远程服务器或服务验证自己的身份。 Microsoft 之前 [发布了指南](https://www.microsoft.com/download/details.aspx?id=36036) 以缓解传递哈希攻击。  Windows Server 2012 R2 包括可帮助缓解此类攻击进一步的新功能。 有关其他帮助防止凭据被盗的安全功能的详细信息，请参阅 [凭据保护和管理](https://technet.microsoft.com/library/dn408190.aspx)。 本主题介绍了如何配置以下新功能：  
  
-   [受保护的用户](how-to-configure-protected-accounts.md#BKMK_AddtoProtectedUsers)  
  
-   [身份验证策略](how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicies)  
  
-   [身份验证策略接收器](how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicySilos)  
  
Windows 8.1 和 Windows Server 2012 R2 中内置了其他缓解措施，以帮助防止凭据被盗，如以下主题中所述：  
  
-   [远程桌面的受限的管理模式](http://blogs.technet.com/b/kfalde/archive/20../restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)  
  
-   [LSA 保护](https://technet.microsoft.com/library/dn408187)  
  
## <a name="BKMK_AddtoProtectedUsers"></a>受保护的用户  
Protected Users 是一个新的全局安全组，你可以向该组添加新用户或现有用户。 Windows 8.1 设备和 Windows Server 2012 R2 主机具有与此组以提供更好的防护以免凭据被盗的成员的特殊行为。 为组的成员，Windows 8.1 设备或 Windows Server 2012 R2 主机不会缓存不支持的受保护的用户凭据。 如果登录到运行早于 Windows 8.1 的 Windows 版本的设备，此组的成员具有任何额外的保护。  
  
谁是登录到 Windows 8.1 的设备组的受保护用户成员和 Windows Server 2012 R2 主机可以*不再*使用：  
  
-   默认凭据委派 (CredSSP) - 不会缓存纯文本凭据，即使在启用了**允许委派默认凭据**策略时也是如此  
  
-   Windows 摘要 - 不会缓存纯文本凭据，即使在启用了它们时也是如此  
  
-   NTLM - 不会缓存 NTOWF  
  
-   Kerberos 长期密钥 - Kerberos 票证授予票证 (TGT) 在登录时获取，无法自动重新获取  
  
-   脱机登录 - 不会创建缓存的登录验证程序  
  
如果域功能级别为 Windows Server 2012 R2，组的成员不再可以：  
  
-   使用 NTLM 身份验证进行身份验证  
  
-   在 Kerberos 预身份验证中使用数据加密标准 (DES) 或 RC4 密码套件  
  
-   使用不受约束或约束委派进行委派  
  
-   在超出最初的 4 小时生存期后续订用户票证 (TGT)  
  
若要将用户添加到组，可以使用[用户界面工具](https://technet.microsoft.com/library/cc753515.aspx)如 Active Directory 管理中心 (ADAC) 或 Active Directory 用户和计算机或之类的命令行工具[Dsmod 组](https://technet.microsoft.com/library/cc732423.aspx)，或 WindowsPowerShell[Add-adgroupmember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet。 服务和计算机的帐户 *不应* 是受保护用户组的成员。 这些帐户的成员身份不提供本地保护，因为密码或证书在主机上始终可用。  
  
> [!WARNING]  
> 身份验证限制没有规避方法，这意味着权限较高的组（例如 Enterprise Admins 组或 Domain Admins 组）的成员受到的限制与 Protected Users 组的其他成员一样。 如果此类组的所有成员都添加到 Protected Users 组，则可能不包含所有这些帐户。在已经全面测试潜在影响之前，永远不应将所有权限较高的帐户添加到 Protected Users 组。  
  
Protected Users 组的成员必须能够使用高级加密标准 (AES) 的 Kerberos 进行身份验证。 对于 Active Directory 中的帐户，此方法需要 AES 密钥。 内置管理员不具有 AES 密钥，除非密码已更改运行 Windows Server 2008 的域控制器上或更高版本。 此外，不包含已在域控制器上更改密码的任何帐户，该域控制器运行较早版本的 Windows Server。因此，请遵循以下最佳实践：  
  
-   不要测试，在域中除非**的所有域控制器都运行 Windows Server 2008 或更高版本**。  
  
-   针对创建该域**之前**创建的所有域帐户“更改密码”。 否则，这些帐户无法进行身份验证。  
  
-   **更改密码**为每个用户之前将帐户添加到受保护用户组，或确保密码已在运行 Windows Server 2008 的域控制器上最近已更改或更高版本。  
  
### <a name="BKMK_Prereq"></a>使用受保护的帐户的要求  
受保护帐户具有以下部署要求：  
  
-   若要提供受保护的用户的客户端的限制，主机必须运行 Windows 8.1 或 Windows Server 2012 R2。 用户仅需使用作为 Protected Users 组成员的帐户进行登录。 在这种情况下，可以通过创建受保护用户组[主域控制器 (PDC) 模拟器角色传输](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到运行 Windows Server 2012 R2 的域控制器。 将该组对象复制到其他域控制器后，PDC 模拟器角色可以托管在运行较早版本的 Windows Server 的域控制器上。  
  
-   若要提供域控制器端限制为受保护用户，即限制使用 NTLM 身份验证和其他限制，域功能级别必须是 Windows Server 2012 R2。 有关功能级别的详细信息，请参阅 [了解 Active Directory 域服务 (AD DS) 功能级别](../../identity/ad-ds/active-directory-functional-levels.md)。  
  
### <a name="BKMK_TrubleshootingEvents"></a>故障排除与受保护的用户相关的事件  
本部分介绍了有助于对受保护用户的相关事件进行疑难解答的新日志，并介绍了受保护用户如何影响更改以解决票证授予票证 (TGT) 过期或委派问题。  
  
#### <a name="new-logs-for-protected-users"></a>用于受保护用户的新日志  
提供两个新的操作管理日志，以帮助对受保护用户的相关事件进行疑难解答：受保护的用户-客户端日志和受保护的用户故障-域控制器日志。 这些新日志位于在事件查看器中，并且在默认情况下处于禁用状态。 若要启用日志，请依次单击“应用程序和服务日志” 、“Microsoft” 、“Windows” 、“身份验证” ，然后单击该日志的名称并单击“操作”  （或右键单击该日志）和“启用日志” 。  
  
有关这些日志中事件的详细信息，请参阅 [身份验证策略和身份验证策略接收器](https://technet.microsoft.com/library/dn486813.aspx)。  
  
#### <a name="troubleshoot-tgt-expiration"></a>解决 TGT 过期问题  
通常，域控制器根据域策略设置 TGT 生存期和续订，如下面“组策略管理编辑器”窗口中所示。  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
对于“Protected Users” ，以下设置是硬编码的：  
  
-   用户票证的最长生存期：240 分钟  
  
-   用户票证续订的最长生存期：240 分钟  
  
#### <a name="troubleshoot-delegation-issues"></a>解决委派问题  
以前，如果使用 Kerberos 委派的技术失败，则会检查客户端帐户，以查看是否设置了“敏感帐户，不能被委派”  。 但是，如果帐户是“Protected Users” 的成员，它可能不会在 Active Directory 管理中心 (ADAC) 中配置此设置。 因此，在解决委派问题时，请检查设置和组成员身份。  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
### <a name="BKMK_AuditAuthNattempts"></a>审核身份验证尝试  
若要为“Protected Users”组的成员显式审核身份验证尝试，你可以继续收集安全日志审核事件或在新操作管理日志中收集数据。 有关这些事件的详细信息，请参阅 [身份验证策略和身份验证策略接收器](https://technet.microsoft.com/library/dn486813.aspx)  
  
### <a name="BKMK_ProvidePUdcProtections"></a>服务和计算机提供 DC 端保护  
服务和计算机的帐户不能是“Protected Users”的成员。 本部分说明了可以向这些帐户提供哪些基于域控制器的保护：  
  
-   拒绝 NTLM 身份验证：仅可通过 [NTLM 块策略](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)进行配置  
  
-   在 Kerberos 预身份验证中拒绝数据加密标准 (DES)：Windows Server 2012 R2 域控制器针对计算机帐户接受 DES，除非它们针对 DES 配置只是因为每个版本的 Windows 与 Kerberos 一起发布还支持 RC4。  
  
-   在 Kerberos 预身份验证中拒绝 RC4：不可配置。  
  
    > [!NOTE]  
    > 尽管可以 [更改支持的加密类型的配置](http://blogs.msdn.com/b/openspecification/archive/20../windows-configurations-for-kerberos-supported-encryption-type.aspx)，但如果未在目标环境中进行测试，不建议针对计算机帐户更改这些设置。  
  
-   将用户票证 (TGT) 限制为初始 4 小时生存期：使用身份验证策略。  
  
-   使用不受约束或约束委派拒绝委派：若要限制帐户，请打开 Active Directory 管理中心 (ADAC)，然后选中“敏感帐户，不能被委派”复选框。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
## <a name="BKMK_CreateAuthNPolicies"></a>身份验证策略  
身份验证策略是 AD DS 中包含身份验证策略对象的一个新容器。 身份验证策略可以指定帮助减少凭据被盗风险的设置，例如限制帐户的 TGT 生存期或添加其他与声明相关的条件。  
  
在 Windows Server 2012 动态访问控制引入了名为中央访问策略，可以方便地跨组织配置文件服务器的 Active Directory 林作用域对象类。 在 Windows Server 2012 R2 中，名为身份验证策略 (objectClass Msds-authnpolicies) 的新对象类可用于将身份验证配置应用到 Windows Server 2012 R2 的域中的帐户类。 Active Directory 帐户类包括：  
  
-   “用户”  
  
-   计算机  
  
-   托管服务帐户和组托管服务帐户 (GMSA)  
  
### <a name="quick-kerberos-refresher"></a>快速 Kerberos 刷新程序  
Kerberos 身份验证协议包括三种类型的交换（也称为子协议）：  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbRefresher.gif)  
  
-   身份验证服务 (AS) 交换 (KRB_AS_*）  
  
-   票证授予服务 (TGS) 交换 (KRB_TGS_*）  
  
-   客户端/服务器 (AP) 交换 (KRB_AP_*）  
  
在 AS 交换中，其中客户端使用该帐户的密码或私钥创建预身份验证器，以请求票证授予票证 (TGT)。 此情况在用户登录或首次需要服务票证时发生。  
  
在 TGS 交换中，其中该帐户的 TGT 用于创建身份验证器，以请求服务票证。 此情况在需要经过验证的连接时发生。  
  
AP 交换的发生频率与应用程序协议的中数据相同，并且不受应用程序协议的影响。  
  
有关详细信息，请参阅 [Kerberos 版本 5 身份验证协议的工作原理] (https://technet.microsoft.com/library/cc772815(v=WS.10.aspx。  
  
### <a name="overview"></a>概述  
通过提供一种用于将可配置限制应用到帐户的方法，并且通过为服务和计算机的帐户提供限制，身份验证策略可补充受保护用户。 在 AS 交换或 TGS 交换期间强制执行身份验证策略。  
  
通过配置以下内容可限制初始身份验证或 AS 交换：  
  
-   TGT 生存期  
  
-   用于限制用户登录的访问控制条件，从其中进行 AS 交换的设备必须满足这些条件  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictAS.gif)  
  
通过配置以下内容，可通过票证授予服务 (TGS) 交换来限制服务票证请求：  
  
-   访问控制条件，从其中进行 TGS 交换的客户端（用户、服务、计算机）或设备必须满足这些条件  
  
### <a name="BKMK_ReqForAuthnPolicies"></a>使用身份验证策略的要求  
  
|策略|要求|  
|-----|--------|  
|提供自定义 TGT 生存期| Windows Server 2012 R2 域功能级别帐户域|  
|限制用户登录|的支持动态访问控制 Windows Server 2012 R2 域功能级别帐户域<br />Windows 8、 Windows 8.1、 Windows Server 2012 或 Windows Server 2012 R2 与动态访问控制的设备支持|  
|限制基于用户帐户和安全组的服务票证颁发| Windows Server 2012 R2 域功能级别资源域|  
|限制基于用户声明或设备帐户、安全组或者声明的服务票证分发| 支持动态访问控制的 Windows Server 2012 R2 域功能级别资源域|  
  
### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>将用户帐户限制到特定设备和主机  
具有管理权限的高价值帐户应该是“Protected Users”组的成员。 默认情况下，所有帐户都不是“Protected Users”  组的成员。 将帐户添加到该组之前，配置域控制器支持并创建审核策略，以确保不存在阻止问题。  
  
#### <a name="configure-domain-controller-support"></a>配置域控制器支持  
用户的帐户域必须是在 Windows Server 2012 R2 域功能级别 (DFL)。 确保所有域控制器是 Windows Server 2012 R2，以及如何将 Active Directory 域和信任关系[将 dfl 提升](https://technet.microsoft.com/library/cc753104.aspx)到 Windows Server 2012 R2。  
  
**若要配置动态访问控制的支持**  
  
1.  在默认域控制器策略中，单击“已启用”，以在计算机配置 | 管理模板 | 系统 | KDC 中启用“密钥发行中心 (KDC) 客户端支持声明、复合身份验证和 Kerberos 保护”。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)  
  
2.  在“选项”下的下拉列表框中，选择“始终提供声明”。  
  
    > [!NOTE]  
    > **支持**也可以进行配置，但因为域位于 Windows Server 2012 R2 dfl 上，因此使 Dc 始终提供声明将允许用户基于声明的访问权限检查时使用非声明感知设备和主机连接到声明感知服务。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)  
  
    > [!WARNING]  
    > 配置**未保护身份验证请求失败**将导致不支持 Kerberos 保护，如 Windows 7 和早期操作系统，或运行任何操作系统中的身份验证失败系统从 Windows 8 开始具有未显式配置为支持它。  
  
#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>使用 ADAC 为身份验证策略创建用户帐户审核  
  
1.  打开 Active Directory 管理中心 (ADAC)。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_OpenADAC.gif)  
  
    > [!NOTE]  
    > 所选**身份验证**节点是位于 Windows Server 2012 R2 DFL 的域。 如果未出现该节点，然后重试是在 Windows Server 2012 R2 DFL 的域中使用域管理员帐户。  
  
2.  单击“身份验证策略”，然后单击“新建”以创建新策略。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)  
  
    身份验证策略必须具有一个显示名称，并且在默认情况下强制执行。  
  
3.  若要创建仅用于审核的策略，请单击“仅审核策略限制” 。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)  
  
    根据 Active Directory 帐户类型应用身份验证策略。 通过为每种类型配置设置，可将单个策略应用到全部三个帐户类型。 帐户类型包括：  
  
    -   “用户”  
  
    -   计算机  
  
    -   托管服务帐户和组托管服务帐户  
  
    如果你已通过由密钥发行中心 (KDC) 使用的新主体扩展架构，则从最近的派生帐户类型中分类出新帐户类型。  
  
4.  若要配置用户帐户的 TGT 生存期，请选择“为用户帐户指定票证授予票证生存期”  复选框，然后输入时间（以分钟为单位）。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTLifetime.gif)  
  
    例如，如果你需要的 TGT 生存期上限为 10 小时，请按显示输入 **600** 。 如果没有配置 TGT 生存期，则当帐户是 **Protected Users** 组的成员时，TGT 生存期和续订时间为 4 小时。 否则，对于具有默认设置的域，TGT 生存期和续订时间基于域策略，如以下“组策略管理编辑器”窗口中所示。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
5.  若要限制用户帐户选择设备，请单击“编辑”以定义设备所需的条件。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)  
  
6.  在“编辑访问控制条件”窗口中，单击“添加条件”。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCondition.png)  
  
##### <a name="add-computer-account-or-group-conditions"></a>添加计算机帐户或组条件  
  
1.  若要配置计算机帐户或组，请在下拉列表中，选中下拉列表框“每个组的成员”并更改为“任何组的成员”。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompMember.png)  
  
    > [!NOTE]  
    > 此访问控制定义用户从中登录的设备或主机的条件。 在访问控制术语中，设备或主机的计算机帐户是用户，这就是“用户”是唯一选项的原因。  
  
2.  单击“添加项目” 。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddItems.png)  
  
3.  若要更改对象类型，请单击“对象类型” 。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjects.gif)  
  
4.  若要在 Active Directory 中选择计算机对象，请单击“计算机”，然后单击“确定”。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)  
  
5.  键入计算机的名称以限制该用户，然后单击“检查名称” 。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)  
  
6.  单击“确定”，并为计算机帐户创建任何其他条件。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddConditions.png)  
  
7.  完成后，单击“确定”  ，将为计算机帐户显示定义的条件。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompDone.png)  
  
##### <a name="add-computer-claim-conditions"></a>添加计算机声明条件  
  
1.  若要配置计算机声明，下拉“组”以选择该声明。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaim.gif)  
  
    声明仅在林中进行了设置后才可用。  
  
2.  键入 OU 的名称，应该限制登录用户帐户。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimOUName.gif)  
  
3.  完成后，单击“确定”，该框将显示所定义的条件。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimComplete.gif)  
  
##### <a name="troubleshoot-missing-computer-claims"></a>解决丢失计算机声明的问题  
如果已设置该声明，但不可用，则可能仅针对“计算机”类配置了它。  
  
假设你想要限制基于组织单位 (OU) 的身份验证的计算机，已配置，但只适用于**计算机**类。  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictComputers.gif)  
  
为了使声明可用于限制用户登录设备，请选择“用户”复选框。  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)  
  
#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>使用 ADAC 设置具有身份验证策略的用户帐户  
  
1.  从“用户”帐户中，单击“策略”。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicy.gif)  
  
2.  选中“将身份验证策略分配给此帐户”复选框。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)  
  
3.  然后选择要应用到该用户的身份验证策略。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicySelect.png)  
  
#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>在设备和主机上配置动态访问控制支持  
你可以在不配置动态访问控制 (DAC) 的情况下配置 TGT 生存期。 只有在检查 AllowedToAuthenticateFrom 和 AllowedToAuthenticateTo 时需要 DAC。  
  
通过使用组策略或本地组策略编辑器，在计算机配置 | 管理模板 | 系统 | Kerberos 中启用“Kerberos 客户端支持声明、复合身份验证和 Kerberos 保护”：  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)  
  
### <a name="BKMK_TroubleshootAuthnPolicies"></a>对身份验证策略进行故障排除  
  
#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>确定直接分配身份验证策略的帐户  
身份验证策略中的帐户部分显示了已直接应用该策略的帐户。  
  
![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AccountsAssigned.gif)  
  
#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>使用身份验证策略失败-域控制器管理日志  
一个新**身份验证策略失败-域控制器**下的管理日志**应用程序和服务日志** > **Microsoft**  >  **Windows** > **身份验证**已创建，以使其更轻松地发现由于身份验证策略导致的失败。 默认情况下，该日志处于禁用状态。 若要启用它，请右键单击日志名称，然后单击“启用日志”。 新事件中的内容与现有 Kerberos TGT 和服务票证审核事件中的内容非常相似。 有关这些事件的详细信息，请参阅 [身份验证策略和身份验证策略接收器](https://technet.microsoft.com/library/dn486813.aspx)。  
  
### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>使用 Windows PowerShell 管理身份验证策略  
此命令创建名为 **TestAuthenticationPolicy**的身份验证策略。 **UserAllowedToAuthenticateFrom** 参数可指定一些设备，用户可从中通过名为 someFile.txt 的文件中的 SDDL 字符串进行身份验证。  
  
```  
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl  
```  
  
此命令将获取与 **Filter** 参数指定的筛选器相匹配的所有身份验证策略。  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com  
  
```  
  
此命令将修改指定身份验证策略的说明和 **UserTGTLifetimeMins** 属性。  
  
```  
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45  
```  
  
此命令将删除 **Identity** 参数指定的身份验证策略。  
  
```  
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1  
```  
  
此命令将结合使用 **Get-ADAuthenticationPolicy** cmdlet 和 **Filter** 参数，以获取不会强制执行的所有身份验证策略。 该结果集通过管道传送给 **Remove-ADAuthenticationPolicy** cmdlet。  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy  
```  
  
## <a name="BKMK_CreateAuthNPolicySilos"></a>身份验证策略接收器  
身份验证策略接收器是 AD DS 中用于用户、计算机和服务帐户的一个新容器 (objectClass msDS-AuthNPolicySilos）。 它们可帮助保护高价值帐户。 当所有组织需要保护 Enterprise Admins、Domain Admins 和 Schema Admins 组中的成员（因为攻击者可能使用这些帐户访问林中的任何内容）时，可能还需要保护其他帐户。  
  
通过创建独特于工作负载的帐户，并通过应用组策略来限制本地和远程交互式登录和管理权限，某些组织可隔离这些工作负载。 通过创建一种用于定义用户、计算机和托管服务帐户之间的关系的方式，身份验证策略接收器可补充此工作。 帐户只能属于一个接收器。 你可以为每种类型的帐户配置身份验证策略，以便控制以下内容：  
  
1.  不可续订的 TGT 生存期  
  
2.  用于返回 TGT 的访问控制条件（注意：无法应用到系统，因为需要 Kerberos 保护）  
  
3.  用于返回服务票证的访问控制条件  
  
此外，身份验证策略接收器中的帐户具有接收器声明，声明感知资源（例如文件服务器）可使用该声明来控制访问权限。  
  
可以配置新的安全描述符以基于以下内容控制服务票证的分发：  
  
-   用户、 用户的安全组和/或用户的声明  
  
-   设备、 设备的安全组和/或设备的声明  
  
获取此信息对资源的 Dc 需要动态访问控制：  
  
-   用户声明：  
  
    -   支持动态访问控制的 Windows 8 和更高版本客户端  
  
    -   帐户域支持动态访问控制和声明  
  
-   设备和/或设备安全组：  
  
    -   支持动态访问控制的 Windows 8 和更高版本客户端  
  
    -   为复合身份验证配置的资源  
  
-   设备声明：  
  
    -   支持动态访问控制的 Windows 8 和更高版本客户端  
  
    -   设备域支持动态访问控制和声明  
  
    -   为复合身份验证配置的资源  
  
身份验证策略可应用到身份验证策略接收器的所有成员（而非单个帐户），或者单独身份验证策略可应用到接收器内不同类型的帐户。 例如，一个身份验证策略可应用到权限较高的用户帐户，而另一个策略可应用到服务帐户。 必须至少创建一个身份验证策略，然后才能创建身份验证策略接收器。  
  
> [!NOTE]  
> 身份验证策略可应用到身份验证策略接收器的成员，或者可以独立于接收器应用该策略以限制特定帐户作用域。 例如，若要保护单个帐户或一个小的帐户集，可以在这些帐户上设置策略，而无需将帐户添加到接收器。  
  
可以使用 Active Directory 管理中心或 Windows PowerShell 来创建身份验证策略接收器。 默认情况下，身份验证策略接收器仅审核接收器策略，这等同于指定**WhatIf**在 Windows PowerShell cmdlet 中的参数。 在这种情况下，不应用策略接收器限制，但生成审核以指示应用限制时是否发生故障。  
  
#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心创建身份验证策略接收器的步骤  
  
1.  打开“Active Directory 管理中心” ，单击“身份验证” ，右键单击“身份验证策略接收器” ，单击“新建” ，然后单击“身份验证策略接收器” 。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)  
  
2.  在“显示名称”中，键入接收器的名称。 在“允许的帐户”中，单击“添加”、键入帐户的名称，然后单击“确定”。 你可以指定用户、计算机或服务帐户。 然后指定是针对所有主体使用单个策略，还是针对每个类型的主体使用单独的策略，并指定单个或多个策略的名称。  
  
    ![受保护的帐户](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)  
  
### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>使用 Windows PowerShell 管理身份验证策略接收器  
此命令将创建身份验证策略接收器对象并强制执行它。  
  
```  
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce  
```  
  
此命令将获取与由 **Filter** 参数指定的筛选器相匹配的所有身份验证策略接收器。 输出随后将传递到 **Format-Table** cmdlet，以显示该策略的名称以及每个策略上 **Enforce** 的值。  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize  
  
Name  Enforce  
--  ----  
silo     True  
silos   False  
  
```  
  
此命令将结合使用 **Get-ADAuthenticationPolicySilo** cmdlet 和 **Filter** 参数，以获取不会强制执行的所有身份验证策略接收器，并通过管道将筛选器结果传送给 **Remove-ADAuthenticationPolicySilo** cmdlet。  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo  
```  
  
此命令将向名为 *User01* 的用户帐户授予对名为 *Silo*的身份验证策略接收器的访问权限。  
  
```  
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01  
```  
  
此命令将吊销名为 *User01* 的用户帐户对名为 *Silo*的身份验证策略接收器的访问权限。 因为 **Confirm** 参数已设置为 **$False**，因此将不显示任何确认消息。  
  
```  
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False  
```  
  
此示例首先使用 **Get-ADComputer** cmdlet 来获取与 **Filter** 参数指定的筛选器相匹配的所有计算机帐户。 此命令的输出将传递给 **Set-ADAccountAuthenticatinPolicySilo**，以将名为 *Silo* 的身份验证策略接收器和名为 *AuthenticationPolicy02* 的身份验证策略分配给它们。  
  
```  
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02  
```  
  


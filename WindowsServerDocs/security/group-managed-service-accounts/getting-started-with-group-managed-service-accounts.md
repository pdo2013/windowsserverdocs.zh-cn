---
title: "入门组托管服务帐户"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7130ad73-9688-4f64-aca1-46a9187a46cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cd1e92f93701e5b430d1425fcf9a2f3590d1fe5d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-group-managed-service-accounts"></a>入门组托管服务帐户

>适用于：Windows Server（半年通道），Windows Server 2016


本指南提供的启用以及在 Windows Server 2012 使用组托管服务帐户的分步说明和背景信息。

**本文档**

-   [先决条件](#BKMK_Prereqs)

-   [简介](#BKMK_Intro)

-   [部署新的服务器电场的日落](#BKMK_DeployNewFarm)

-   [添加到现有 server 场成员主机](#BKMK_AddMemberHosts)

-   [更新组托管服务帐户属性](#BKMK_Update_gMSA)

-   [停止使用成员主机从现有服务器电场的日落](#BKMK_DecommMemberHosts)


> [!NOTE]
> 本主题包含示例 Windows PowerShell cmdlet，你可以使用自动所述的过程的一部分。 有关详细信息，请参阅[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。

## <a name="BKMK_Prereqs"></a>先决条件
在上看到本主题中的相关部分[要求组托管服务帐户](#BKMK_gMSA_Req)。

## <a name="BKMK_Intro"></a>简介
当客户端计算机连接到在使用网络负载平衡 (NLB) 或其他方法所有服务器都似乎能相同的客户端服务服务器场托管的服务时，然后身份验证支持相互身份验证，如 Kerberos 无法使用协议除非服务的所有这些都使用的相同的原则。 这意味着每个服务都使用同一密码键证明自己的身份。

> [!NOTE]
> 故障转移群集 gMSAs 不支持。 但是，在群集服务顶部运行的服务可以使用 gMSA 或 sMSA 它们是 Windows 服务、 应用池、 计划的任务中，还是本地支持 gMSA 或 sMSA。

服务有从中可以选择，下面原则，并且每有一些限制。

|主体|范围|支持服务|管理密码|
|-------|-----|-----------|------------|
|帐户的 Windows 的计算机系统|域|Limited 一个域加入服务器|计算机管理|
|如果没有 Windows 系统计算机帐户|域|任何域连接的服务器|无|
|虚拟帐户|本地|一台服务器限于|计算机管理|
|Windows 7 独立托管服务帐户|域|Limited 一个域加入服务器|计算机管理|
|用户帐户|域|任何域连接的服务器|无|
|组托管的服务帐户|域|任何 Windows Server 2012 已加入域的服务器|管理域控制器，并主机检索|

Windows 计算机帐户或在 Windows 7 独立托管服务帐户 (sMSA)，或不能在多个系统共享虚拟的帐户。 如果您要共享的服务器场上配置服务的一个帐户，你必须选择用户帐户或 Windows 系统除了计算机帐户。 不论采取何种方式，这些帐户不具有管理单点控制密码的功能。 这将创建每一个组织需要创建更新中 Active Directory 的服务的键，然后将这些服务的所有实例键分发便宜解决方案的问题。

与 Windows Server 2012、 服务或服务管理员不必管理密码同步服务实例时使用组托管服务帐户 (gMSA) 之间。 在广告配置 gMSA，然后配置支持托管服务帐户的服务。 您可以配置 gMSA 使用 *-ADServiceAccount cmdlet 的 Active Directory 模块的一部分。 在主机上的服务身份配置为受支持：

-   作为 sMSA，以便支持产品的支持 sMSA gMSA 相同 Api

-   服务，该服务控制管理器用于配置身份登录

-   使用应用程序池 IIS 经理配置标识该服务

-   使用任务计划程序中的任务。

### <a name="BKMK_gMSA_Req"></a>要求组托管服务帐户
下表列出了 Kerberos 身份验证服务使用 gMSA 处理的操作系统系统要求。 后的表列出了 Active Directory 要求。

64 位体系结构需运行的 Windows PowerShell 命令，用于管理组托管服务帐户。

**操作系统系统要求**

|元素|要求|操作系统|
|------|--------|----------|
|客户端应用程序主机|RFC 兼容 Kerberos 客户端|At 最少的 Windows XP|
|用户帐户域域控制器|RFC 兼容 KDC|At 至少 Windows Server 2003|
|共享的服务成员主机|| Windows Server 2012 |
|成员主机域域控制器|RFC 兼容 KDC|At 至少 Windows Server 2003|
|gMSA 帐户域域控制器| Windows Server 2012 Dc 适用于主机检索密码|与 Windows Server 2012 拥有某些系统早于 Windows Server 2012 的域 |
|后端服务主机|符合 RFC Kerberos 应用程序服务器|At 至少 Windows Server 2003|
|后端服务帐户域域控制器|RFC 兼容 KDC|At 至少 Windows Server 2003|
|对于 Active Directory 的 Windows PowerShell|有关本地安装在计算机支持 64 位体系结构或远程管理计算机 （例如，使用远程服务器管理工具包） 上的 Active Directory 的 Windows PowerShell| Windows Server 2012 |

**Active Directory 域服务要求**

-   林中 gMSA 域的 Active Directory 方案需要更新与 Windows Server 2012 创建 gMSA。

    你可以安装运行 Windows Server 2012 的域控制器，或运行 Windows Server 2012 的计算机中运行的版本 adprep.exe 更新方案。 对象 CN 对象版本特性值 = 方案，CN = 配置，直流 = Contoso 特区 = Com 必须 52。

-   预配新 gMSA 帐户

-   如果你使用组中，然后将新的或现有安全组 gMSA 的服务主机权限管理

-   如果管理通过组，然后新的或现有安全组服务访问控制

-   如果 Active Directory 的第一个主根键未部署在域中，或者未创建，然后创建它。 事件 ID 4004 KdsSvc 运营日志中，可验证其创建的结果。

有关如何创建键，查看[创建键分发服务 KDS 根键](create-the-key-distribution-services-kds-root-key.md)。 Microsoft 键分发服务 (kdssvc.dll) 根密钥广告。

**生命周期**

生命周期网站通常使用 gMSA 功能涉及以下任务：

-   部署新的服务器电场的日落

-   添加到现有 server 场成员主机

-   停止使用成员主机从现有服务器电场的日落

-   停止使用现有服务器电场的日落

-   如有必要，请从 server 场删除受到威胁的成员主机。

## <a name="BKMK_DeployNewFarm"></a>部署新的服务器电场的日落
部署新的服务器电场的日落时, 需要确定的服务管理员：

-   如果该服务将支持使用 gMSAs

-   如果该服务所需传入或传出经过身份验证的连接

-   使用 gMSA 送修成员主机计算机帐户名称

-   服务 NetBIOS 名称

-   该服务 DNS 主机名称

-   该服务服务主要名称 (Spn)

-   更改密码的时间间隔 （默认为 30 天）。

### <a name="BKMK_Step1"></a>第 1 步： 预配组托管服务帐户
仅当森林架构已更新到 Windows Server 2012、 部署了 Active Directory 的主根键，并且至少一个 Windows Server 2012 DC 将创建 gMSA 域中，则可以创建 gMSA。

在会员**域管理员**，**帐户运算符**或能够创建 msDS GroupManagedServiceAccount 对象的最低要求完成以下过程。

#### <a name="BKMK_CreateGMSA"></a>若要创建使用新 ADServiceAccount cmdlet gMSA

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  为 Windows PowerShell 命令提示符下，键入以下命令，然后再次按 ENTER。 （Active Directory 模块将自动加载。）

    **新 ADServiceAccount [的姓名] <string> -仅<string>[-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < 可以为 Null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>]-SamAccountName <string> -ServicePrincipalNames < 字符串 [>**

    |参数|字符串|示例|
    |-------|-----|------|
    |名称|帐户名称|ITFarm1|
    |仅|DNS 服务主机名称|ITFarm1.contoso.com|
    |KerberosEncryptionType|支持主服务器的任何加密类型|RC4，AES128，AES256|
    |ManagedPasswordIntervalInDays|密码更改在 （默认为 30 天。 如果不提供） 天的时间间隔|90|
    |PrincipalsAllowedToRetrieveManagedPassword|计算机成员主机或安全组成员的成员主机的帐户|ITFarmHosts|
    |SamAccountName|如果不使用的服务 NetBIOS 名称相同名称|ITFarm1|
    |ServicePrincipalNames|对于服务服务主要名称 (Spn)|http/ITFarm1.contoso.com/contoso.com http/ITFarm1.contoso.com/contoso、 http/ITFarm1/contoso.com，http/ITFarm1/contoso|

    > [!IMPORTANT]
    > 仅可以在创建设置密码更改时间间隔。 如果你需要更改的时间间隔，你必须创建新 gMSA，并将其设置每次创建。

    **示例**

    在同一行中，输入命令，即使他们可能会显示换跨以下几个行由于格式化约束。

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

在会员**域管理员**，**帐户运算符**，或创建 msDS GroupManagedServiceAccount 对象的功能是的最低要求才能完成此过程。 有关使用相应的帐户和组成员的详细信息，请参阅[本地和域默认组](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>若要仅使用新 ADServiceAccount cmdlet 创建 gMSA 站身份验证

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **新 ADServiceAccount [的姓名] <string> -RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays < 为空 [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>]**

    |参数|字符串|示例|
    |-------|-----|------|
    |名称|帐户名称|ITFarm1|
    |ManagedPasswordIntervalInDays|密码更改在 （默认为 30 天。 如果不提供） 天的时间间隔|75|
    |PrincipalsAllowedToRetrieveManagedPassword|计算机成员主机或安全组成员的成员主机的帐户|ITFarmHosts|

    > [!IMPORTANT]
    > 仅可以在创建设置密码更改时间间隔。 如果你需要更改的时间间隔，你必须创建新 gMSA，并将其设置每次创建。

**示例**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>第 2 步： 配置服务身份应用程序的服务
在 Windows Server 2012 配置服务，请参阅以下功能的文档：

-   IIS 应用程序池

    有关详细信息，请参阅[应用程序池 (IIS 7) 中指定的标识](https://technet.microsoft.com/library/cc771170(WS.10).aspx)。

-   Windows 的服务

    有关详细信息，请参阅[服务](https://technet.microsoft.com/library/cc772408.aspx)。

-   任务

    有关详细信息，请参阅[任务计划程序概述](https://technet.microsoft.com/library/cc721871.aspx)。

其他服务可支持 gMSA。 有关如何配置这些服务，请参阅适用产品文档，了解详细信息。

## <a name="BKMK_AddMemberHosts"></a>添加到现有 server 场成员主机
如果使用安全组管理成员主机，请将计算机帐户添加安全组 （的 gMSA 成员主机的成员） 向新成员主机使用以下方法之一。

在会员**域管理员**，或将成员添加到安全组对象的功能是的最低要求才能完成这些步骤。

-   方法 1: Active Directory 的用户和计算机

    有关过程如何使用此方法，请参阅[计算机帐户添加到某个组](https://technet.microsoft.com/library/cc733097.aspx)使用的 Windows 界面、 和[管理 Active Directory 管理中心在不同的域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2： 现有

    有关过程如何使用此方法，请参阅[计算机帐户添加到某个组](https://technet.microsoft.com/library/cc733097.aspx)使用命令行。

-   方法 3: Active Directory 的 Windows PowerShell cmdlet 添加 ADPrincipalGroupMembership

    有关过程如何使用此方法，请参阅[添加 ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx)。

如果使用的帐户的计算机，查找已有的帐户，然后添加新的计算机帐户。

在会员**域管理员**，**帐户运算符**，或管理 msDS GroupManagedServiceAccount 对象的功能是的最低要求才能完成此过程。 有关使用相应的帐户和组成员的详细信息，请参阅本地和域默认组。

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>若要添加使用一组 ADServiceAccount cmdlet 成员主机

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **获取 ADServiceAccount [的姓名] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **设置 ADServiceAccount [的姓名] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>**

|参数|字符串|示例|
|-------|-----|------|
|名称|帐户名称|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|计算机成员主机或安全组成员的成员主机的帐户|Host1，Host2，Host3|

**示例**

例如，若要添加成员主机键入以下命令，，然后按 enter 键。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1 Host2 Host3

```

## <a name="BKMK_Update_gMSA"></a>更新组托管服务帐户属性
在会员**域管理员**，**帐户运算符**，或写入 msDS GroupManagedServiceAccount 对象的功能是的最低要求才能完成这些步骤。

打开 Active Directory 模块的 Windows PowerShell、 使用和设置的任何属性设置 ADServiceAccount cmdlet。

有关详细信息如何设置这些属性，请参阅[组 ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) TechNet 库中，或通过键入**获取帮助的一组 ADServiceAccount** Active Directory 的 Windows PowerShell 模块在命令提示符并按 ENTER。

## <a name="BKMK_DecommMemberHosts"></a>停止使用成员主机从现有服务器电场的日落
在会员**域管理员**，或从安全组对象，删除成员的功能是的最低要求才能完成这些步骤。

### <a name="step-1-remove-member-host-from-gmsa"></a>第 1 步： 从 gMSA 删除成员主机
如果使用安全组管理成员主机，请删除已停止使用的成员主机安全组 gMSA 成员主机是使用以下方法之一会员的计算机的帐户。

-   方法 1: Active Directory 的用户和计算机

    有关过程如何使用此方法，请参阅[删除计算机帐户](https://technet.microsoft.com/library/cc754624.aspx)使用的 Windows 界面、 和[管理 Active Directory 管理中心在不同的域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2: drsm

    有关过程如何使用此方法，请参阅[删除计算机帐户](https://technet.microsoft.com/library/cc754624.aspx)使用命令行。

-   方法 3: Active Directory 的 Windows PowerShell cmdlet 删除 ADPrincipalGroupMembership

    有关详细信息如何执行此操作，请参阅[删除 ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) TechNet 库中，或通过键入**获取帮助删除-ADPrincipalGroupMembership** Active Directory 的 Windows PowerShell 模块在命令提示符并按 ENTER。

如果列出计算机帐户，检索已有的帐户，然后再将其添加除的计算机已删除的帐户。

在会员**域管理员**，**帐户运算符**，或管理 msDS GroupManagedServiceAccount 对象的功能是的最低要求才能完成此过程。 有关使用相应的帐户和组成员的详细信息，请参阅本地和域默认组。

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>若要使用组 ADServiceAccount cmdlet 成员主机中删除

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **获取 ADServiceAccount [的姓名] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **设置 ADServiceAccount [的姓名] <string> -PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal [>**

|参数|字符串|示例|
|-------|-----|------|
|名称|帐户名称|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|计算机成员主机或安全组成员的成员主机的帐户|Host1 Host3|

**示例**

例如，若要删除成员主机键入以下命令，，然后按 enter 键。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1 Host3

```

### <a name="BKMK_RemoveGMSA"></a>第 2 步： 从系统中删除某个组托管服务帐户
在主机系统上使用卸载 ADServiceAccount 或 NetRemoveServiceAccount API 成员主机中删除缓存的 gMSA 凭据。

在会员**管理员**，或等效的最低要求才能完成这些步骤。

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>若要删除使用卸载 ADServiceAccount cmdlet gMSA

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Active Directory 的 Windows PowerShell 模块的命令提示符下，键入以下命令，然后按 ENTER:

    **卸载 ADServiceAccount < ADServiceAccount >**

    **示例**

    例如，若要删除的缓存 gMSA 凭据命名的 ITFarm1 键入以下命令，，然后按 enter 键：

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

有关详细信息卸载 ADServiceAccount cmdlet、 Active Directory 模块的 Windows PowerShell 命令提示符下，在键入**获取帮助的卸载 ADServiceAccount**，然后按 enter 键，或看到 TechNet 网站上的信息[卸载 ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx)。



## <a name="BKMK_Links"></a>请参阅

-   [组托管的服务帐户概述](group-managed-service-accounts-overview.md)




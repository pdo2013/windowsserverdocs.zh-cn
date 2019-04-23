---
title: 托管服务帐户入门
description: Windows Server 安全
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
ms.openlocfilehash: 6da212185eb47d3f30f81ca6f1eb322bc232e229
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881448"
---
# <a name="getting-started-with-group-managed-service-accounts"></a>托管服务帐户入门

>适用于：Windows 服务器 （半年频道），Windows Server 2016


本指南提供用于启用和使用组托管服务帐户在 Windows Server 2012 中的背景信息和分步说明。

**本文档中**

-   [必备条件](#BKMK_Prereqs)

-   [简介](#BKMK_Intro)

-   [部署新的服务器场](#BKMK_DeployNewFarm)

-   [添加到现有服务器场的成员主机](#BKMK_AddMemberHosts)

-   [更新组托管服务帐户属性](#BKMK_Update_gMSA)

-   [解除授权从现有服务器场的成员主机](#BKMK_DecommMemberHosts)


> [!NOTE]
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。

## <a name="BKMK_Prereqs"></a>系统必备组件
请参阅 [组托管服务帐户的要求](#BKMK_gMSA_Req)上本主题中的部分。

## <a name="BKMK_Intro"></a>简介
当客户端计算机使用网络负载平衡 (NLB) 或其他某些方法（其中所有服务器对于客户端而言似乎是相同的服务）连接到在服务器场中托管的某项服务时，无法使用支持相互身份验证的身份验证协议（如 Kerberos），除非服务的所有实例都使用同一主体。 这意味着每种服务必须使用相同的密码/密钥以证明它们的身份。

> [!NOTE]
> 故障转移群集不支持 gMSA。 但是，如果在群集服务上运行的服务是 Windows 服务、应用池、计划任务或是本机支持的 gMSA 或 sMSA，则它们可以使用 gMSA 或 sMSA。

服务具有以下主体可供选择，并且每个主体存在某些限制。

|主体|范围|支持的服务|密码管理|
|-------|-----|-----------|------------|
|Windows 系统的计算机帐户|域|限于一个加入域的服务器|计算机管理|
|没有 Windows 系统的计算机帐户|域|任何加入域的服务器|无|
|虚拟帐户|本地|限于一台服务器|计算机管理|
|Windows 7 独立托管服务帐户|域|限于一个加入域的服务器|计算机管理|
|用户帐户|域|任何加入域的服务器|无|
|组托管服务帐户|域|任何 Windows Server 2012 加入域的服务器|域控制器管理，以及主机检索|

Windows 计算机帐户或 Windows 7 独立托管服务帐户 (sMSA) 或虚拟帐户无法在多个系统之间进行共享。 如果你为服务器场上的服务配置一个帐户以进行共享，则除了 Windows 系统以外，还必须选择一个用户帐户或计算机帐户。 无论哪种方式，这些帐户不具有单点控制密码管理功能。 这样做会带来麻烦，因为每个组织需要创建一个昂贵的解决方案，以便更新 Active Directory 中服务的密钥，然后将密钥分发给这些服务的所有实例。

与 Windows Server 2012 服务或服务管理员不需要管理服务实例时使用组托管服务帐户 (gMSA) 之间的密码同步。 在 AD 中设置 gMSA，然后配置支持托管服务帐户的服务。 可以使用作为 Active Directory 模块一部分的 *-ADServiceAccount cmdlet 来设置 gMSA。 主机上的服务标识配置受以下内容的支持：

-   与 sMSA 相同的 API，以便支持 sMSA 的产品将支持 gMSA

-   使用服务控制管理器配置登录标识的服务

-   将 IIS 管理器用于应用程序池以配置标识的服务

-   使用任务计划程序的任务。

### <a name="BKMK_gMSA_Req"></a>组托管服务帐户要求
下表列出了要与使用 gMSA 的服务搭配使用的针对 Kerberos 身份验证的操作系统要求。 在表后列出了 Active Directory 的要求。

若要运行用于管理组托管服务帐户的 Windows PowerShell 命令，需要 64 位体系结构。

**操作系统要求**

|元素|要求|操作系统|
|------|--------|----------|
|客户端应用程序主机|符合 RFC 的 Kerberos 客户端|Windows XP 及更高版本|
|用户帐户的域 Dc|符合 RFC 的 KDC|Windows Server 2003 及更高版本|
|共享的服务成员主机|| Windows Server 2012 |
|成员主机的域 Dc|符合 RFC 的 KDC|Windows Server 2003 及更高版本|
|gMSA 帐户的域 Dc| Windows Server 2012 Dc 主机可用于检索密码|这会产生某些系统早于 Windows Server 2012 的 Windows Server 2012 的域 |
|后端服务主机|符合 RFC 的 Kerberos 应用程序服务器|Windows Server 2003 及更高版本|
|后端服务帐户的域 Dc|符合 RFC 的 KDC|Windows Server 2003 及更高版本|
|Active Directory 的 Windows PowerShell|Active Directory 的 Windows PowerShell 安装在本地支持 64 位体系结构的计算机上或安装在你的远程管理计算机上（例如，使用远程服务器管理工具包）| Windows Server 2012 |

**Active Directory 域服务要求**

-   需要更新到 Windows Server 2012，若要创建 gMSA，gMSA 域的林中的 Active Directory 架构。

    您可以通过安装运行 Windows Server 2012 的域控制器，或通过从运行 Windows Server 2012 的计算机运行 adprep.exe 版本来更新的架构。 对象“CN=Schema”、“CN=Configuration”、“DC=Contoso”、“DC=Com”的对象版本属性值必须为 52。

-   已设置的新 gMSA 帐户

-   如果你正在管理服务主机权限以按照组使用 gMSA，则为新的或现有安全组

-   如果按照组管理服务访问控制，则为新的或现有安全组

-   如果 Active Directory 的第一个主根密钥未在域中部署，或者尚未创建，则创建它。 可以在 KdsSvc 运行日志的事件 ID 4004 中验证其创建结果。

有关说明如何创建密钥，请参阅[创建密钥分发服务 KDS 根密钥](create-the-key-distribution-services-kds-root-key.md)。 Microsoft 密钥分发服务 (kdssvc.dll) AD 的根密钥。

**生命周期**

使用 gMSA 功能的服务器场的生命周期通常涉及以下任务：

-   部署新服务器场

-   将成员主机添加到现有服务器场

-   从现有服务器场解除成员主机的授权

-   解除现有服务器场的授权

-   如果需要，请从服务器场中删除已遭到破坏的成员主机。

## <a name="BKMK_DeployNewFarm"></a>部署新的服务器场
在部署新服务器场时，服务管理员将需要确定：

-   该服务是否支持使用 gMSA

-   该服务是否需要入站或出站身份验证连接

-   使用 gMSA 的服务的成员主机的计算机帐户名称

-   该服务的 NetBIOS 名称

-   该服务的 DNS 主机名

-   该服务的服务主体名称 (SPN)

-   密码更改间隔（默认值为 30 天）。

### <a name="BKMK_Step1"></a>步骤 1:设置组托管服务帐户
仅当在林架构已更新为 Windows Server 2012 中，已经部署了 Active Directory 的主根密钥，且将在其中创建 gMSA 的域中没有至少一个 Windows Server 2012 DC 时，可以创建 gMSA。

必须至少具有“域管理员” 、“帐户操作员”  中的成员身份或能够创建 msDS-GroupManagedServiceAccount 对象才能完成下列过程。

#### <a name="BKMK_CreateGMSA"></a>若要创建 gMSA 使用 New-adserviceaccount cmdlet

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell 命令提示符下键入以下命令，然后按 ENTER。 （Active Directory 模块将自动加载。）

    **新 ADServiceAccount [-名称] <string> -DNSHostName <string> [-KerberosEncryptionType <ADKerberosEncryptionType>] [-ManagedPasswordIntervalInDays < 可以为 Null [Int32] >] [-PrincipalsAllowedToRetrieveManagedPassword < ADPrincipal]>]-SamAccountName <string> -ServicePrincipalNames < 字符串 [] >**

    |参数|字符串|示例|
    |-------|-----|------|
    |名称|帐户的名称|ITFarm1|
    |DNSHostName|服务的 DNS 主机名称|ITFarm1.contoso.com|
    |KerberosEncryptionType|任何由主机服务器支持的加密类型|RC4、AES128、AES256|
    |ManagedPasswordIntervalInDays|密码更改时间间隔，以天为单位（如果不提供，则默认值为 30 天）|90|
    |PrincipalsAllowedToRetrieveManagedPassword|成员主机或成员主机是其成员的安全组的计算机帐户|ITFarmHosts|
    |SamAccountName|该服务的 NetBIOS 名称（如果与“名称”不相同）|ITFarm1|
    |ServicePrincipalNames|该服务的服务主体名称 (SPN)|http/ITFarm1.contoso.com/contoso.com、http/ITFarm1.contoso.com/contoso、http/ITFarm1/contoso.com、http/ITFarm1/contoso|

    > [!IMPORTANT]
    > 仅可以在创建过程中设置密码更改时间间隔。 如果需要更改时间间隔，你必须创建新的 gMSA 并在创建时对它进行设置。

    **示例**

    在一个单独的行中输入命令，即使此处可能因格式限制而出现自动换行为多行。

    ```
    New-ADServiceAccount ITFarm1 -DNSHostName ITFarm1.contoso.com -PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts -KerberosEncryptionType RC4, AES128, AES256 -ServicePrincipalNames http/ITFarm1.contoso.com/contoso.com, http/ITFarm1.contoso.com/contoso, http/ITFarm1/contoso.com, http/ITFarm1/contoso

    ```

必须至少具有“域管理员”、“帐户操作员”中的成员身份或能够创建 msDS-GroupManagedServiceAccount 对象才能完成此过程。 有关使用适当帐户和组成员身份的详细信息，请参阅 [本地和域默认组](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

##### <a name="to-create-a-gmsa-for-outbound-authentication-only-using-the-new-adserviceaccount-cmdlet"></a>仅使用 New-ADServiceAccount cmdlet 创建出站身份验证的 gMSA

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **New-ADServiceAccount [-Name] <string> -RestrictToOutboundAuthenticationOnly [-ManagedPasswordIntervalInDays <Nullable[Int32]>] [-PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>]**

    |参数|字符串|示例|
    |-------|-----|------|
    |名称|命名帐户|ITFarm1|
    |ManagedPasswordIntervalInDays|密码更改时间间隔，以天为单位（如果不提供，则默认值为 30 天）|75|
    |PrincipalsAllowedToRetrieveManagedPassword|成员主机或成员主机是其成员的安全组的计算机帐户|ITFarmHosts|

    > [!IMPORTANT]
    > 仅可以在创建过程中设置密码更改时间间隔。 如果需要更改时间间隔，你必须创建新的 gMSA 并在创建时对它进行设置。

**示例**

```
New-ADServiceAccount ITFarm1 -RestrictToOutboundAuthenticationOnly - PrincipalsAllowedToRetrieveManagedPassword ITFarmHosts

```

### <a name="BKMK_ConfigureServiceIdentity"></a>步骤 2:配置服务标识应用程序服务
若要在 Windows Server 2012 中配置服务，请参阅以下功能文档：

-   IIS 应用程序池

    有关详细信息，请参阅 [为应用程序池 (IIS 7) 指定标识](https://technet.microsoft.com/library/cc771170(WS.10).aspx)。

-   Windows 服务

    有关详细信息，请参阅 [服务](https://technet.microsoft.com/library/cc772408.aspx)。

-   任务

    有关详细信息，请参阅 [任务计划程序概述](https://technet.microsoft.com/library/cc721871.aspx)。

其他服务可以支持 gMSA。 请参阅相应的产品文档以了解有关如何配置这些服务的详细信息。

## <a name="BKMK_AddMemberHosts"></a>添加到现有服务器场的成员主机
如果使用安全组来管理成员主机，将计算机帐户添加新成员主机的安全组 （gMSA 的成员主机是其成员的） 到使用以下方法之一。

必须至少具有“域管理员” 中的成员身份或能够将成员添加到安全组对象才能完成这些过程。

-   方法 1：Active Directory 用户和计算机

    有关如何使用此方法的过程，请参阅使用 Windows 界面 [将计算机帐户添加到组](https://technet.microsoft.com/library/cc733097.aspx) 以及 [在 Active Directory 管理中心内管理不同域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2：dsmod

    有关如何使用此方法的过程，请参阅使用命令行 [将计算机帐户添加到组](https://technet.microsoft.com/library/cc733097.aspx) 。

-   方法 3：Windows PowerShell Active Directory cmdlet Add-ADPrincipalGroupMembership

    有关如何使用此方法的过程，请参阅 [Add-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617203.aspx)。

如果使用计算机帐户，请查找现有帐户，然后再添加新的计算机帐户。

必须至少具有“域管理员” 、“帐户操作员” 中的成员身份或能够管理 msDS-GroupManagedServiceAccount 对象才能完成此过程。 有关使用适当帐户和组成员身份的详细信息，请参阅“本地和域默认组”。

#### <a name="to-add-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>使用 Set-ADServiceAccount cmdlet 添加成员主机

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Get-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Set-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>**

|参数|字符串|示例|
|-------|-----|------|
|名称|命名帐户|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|成员主机或成员主机是其成员的安全组的计算机帐户|Host1、Host2、Host3|

**示例**

例如，若要添加成员主机类型，请键入以下命令，然后按 ENTER。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1-PrincipalsAllowedToRetrieveManagedPassword Host1,Host2,Host3

```

## <a name="BKMK_Update_gMSA"></a>更新组托管服务帐户属性
必须至少具有“域管理员” 、“帐户操作员” 中的成员身份或能够写入 msDS-GroupManagedServiceAccount 对象才能完成这些过程。

请打开 Windows PowerShell 的 Active Directory 模块，并通过使用 Set-ADServiceAccount cmdlet 设置任何属性。

有关如何设置这些属性的详细信息，请参阅 TechNet 库中的 [Set-ADServiceAccount](https://technet.microsoft.com/library/ee617252.aspx) 或通过在 Windows PowerShell 的 Active Directory 模块的命令提示符下输入 **Get-Help Set-ADServiceAccount** 并按 ENTER 来查看。

## <a name="BKMK_DecommMemberHosts"></a>解除授权从现有服务器场的成员主机
必须至少具有“域管理员”中的成员身份或能够将成员从安全组对象删除才能完成这些过程。

### <a name="step-1-remove-member-host-from-gmsa"></a>第 1 步：从 gMSA 删除成员主机
如果使用安全组来管理成员主机，请从 gMSA 的成员主机是使用以下方法之一的成员的安全组的已停用的成员主机删除计算机帐户。

-   方法 1：Active Directory 用户和计算机

    有关如何使用此方法的过程，请参阅使用 Windows 界面 [删除计算机帐户](https://technet.microsoft.com/library/cc754624.aspx) 以及 [在 Active Directory 管理中心内管理不同域](manage-different-domains-in-active-directory-administrative-center.md)。

-   方法 2：drsm

    有关如何使用此方法的过程，请参阅使用命令行 [删除计算机帐户](https://technet.microsoft.com/library/cc754624.aspx) 。

-   方法 3：Windows PowerShell Active Directory cmdlet Remove-ADPrincipalGroupMembership

    有关如何执行此操作的详细信息，请参阅 TechNet 库中的  [Remove-ADPrincipalGroupMembership](https://technet.microsoft.com/library/ee617243.aspx) 或通过在 Windows PowerShell 的 Active Directory 模块的命令提示符下输入 **Get-Help Remove-ADPrincipalGroupMembership** 并按 ENTER 来查看。

如果列出计算机帐户，请检索现有帐户，然后添加除已删除计算机帐户以外的所有帐户。

必须至少具有“域管理员” 、“帐户操作员” 中的成员身份或能够管理 msDS-GroupManagedServiceAccount 对象才能完成此过程。 有关使用适当帐户和组成员身份的详细信息，请参阅“本地和域默认组”。

##### <a name="to-remove-member-hosts-using-the-set-adserviceaccount-cmdlet"></a>使用 Set-ADServiceAccount cmdlet 删除成员主机

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Get-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword**

3.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Set-ADServiceAccount [-Name] <string> -PrincipalsAllowedToRetrieveManagedPassword <ADPrincipal[]>**

|参数|字符串|示例|
|-------|-----|------|
|名称|命名帐户|ITFarm1|
|PrincipalsAllowedToRetrieveManagedPassword|成员主机或成员主机是其成员的安全组的计算机帐户|Host1、Host3|

**示例**

例如，若要删除成员主机类型，请键入以下命令，然后按 ENTER。

```
Get-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword

```

```
Set-ADServiceAccount [-Name] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1,Host3

```

### <a name="BKMK_RemoveGMSA"></a>步骤 2:从系统中删除组托管服务帐户
在主机系统上使用 Uninstall-ADServiceAccount 或 NetRemoveServiceAccount API 从成员主机删除缓存的 gMSA 凭据。

必须至少具有“管理员”的成员身份或同等身份才能完成这些过程。

##### <a name="to-remove-a-gmsa-using-the-uninstall-adserviceaccount-cmdlet"></a>使用 Uninstall-ADServiceAccount cmdlet 删除 gMSA

1.  在 Windows Server 2012 域控制器上，从任务栏中运行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模块的命令提示符下键入以下命令，然后按 ENTER：

    **Uninstall-ADServiceAccount < ADServiceAccount>**

    **示例**

    例如，若要删除名为 ITFarm1 的 gMSA 的缓存凭据，请键入以下命令，然后按 ENTER：

    ```
    Uninstall-ADServiceAccount ITFarm1
    ```

有关 Uninstall-ADServiceAccount cmdlet 的详细信息，可通过在 Windows PowerShell 的 Active Directory 模块的命令提示符下输入 **Get-Help Uninstall-ADServiceAccount**并按 ENTER 进行查看，或者参阅 TechNet Web 上的 [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/ee617202.aspx)信息。



## <a name="BKMK_Links"></a>另请参阅

-   [组托管的服务帐户概述](group-managed-service-accounts-overview.md)




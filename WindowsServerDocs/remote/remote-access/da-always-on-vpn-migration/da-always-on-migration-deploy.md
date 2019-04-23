---
title: 迁移的 DirectAccess 添加到 Always On VPN
description: 将 DirectAccess 迁移到 Always On VPN 要求特定的进程以迁移客户端，这有助于最大程度减少执行迁移步骤顺序从出现的争用条件。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854578"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>迁移到始终启用 VPN 并解除 DirectAccess 授权

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

&#171;[**前面：** 规划 DirectAccess 添加到 Always On VPN 迁移](da-always-on-migration-planning.md)<br>

将 DirectAccess 迁移到 Always On VPN 要求特定的进程以迁移客户端，这有助于最大程度减少执行迁移步骤顺序从出现的争用条件。 在高级别中，迁移过程包括以下四个主要步骤：

1.  **部署由并行 VPN 基础结构。** 已确定迁移阶段和想要在部署中包含的功能后，你将部署与现有的 DirectAccess 基础结构并行 VPN 基础结构。  

2.  **部署到客户端证书和配置。**  VPN 基础结构准备就绪后，创建并发布到客户端所需的证书。 当客户端收到了证书时，还会部署 VPN_Profile.ps1 配置脚本。 或者，可以使用 Intune 配置 VPN 客户端。 使用 Microsoft System Center Configuration Manager 或 Microsoft Intune 成功 VPN 配置部署的监视。

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

在开始之前迁移过程从 DirectAccess 到 Always On VPN，请确保规划了迁移。  如果不打算迁移，请参阅[规划 DirectAccess 添加到 Always On VPN 迁移](da-always-on-migration-planning.md)。

>[!TIP] 
>本部分不是 Always On VPN 的分步部署指南，但而是旨在补充[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)并提供特定于迁移的部署指南。

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>部署由并行 VPN 基础结构

部署 VPN 基础结构，与现有的 DirectAccess 基础结构。  有关分步详细信息，请参阅[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)来安装和配置 Always On VPN 基础结构。 

通过并行部署包括以下高级任务：

1.  创建 VPN 用户、 VPN 服务器和 NPS 服务器组。

2.  创建并发布所需的证书模板。

3.  注册服务器证书。

4.  安装和配置对于 Always On VPN 远程访问服务。

5.  安装和配置 NPS。

6.  为 Always On VPN 配置 DNS 和防火墙规则。

下图提供 DirectAccess-到 – 始终打开 VPN 迁移整个基础结构更改的视觉参考。

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>部署到客户端的证书和 VPN 配置脚本

尽管 VPN 客户端配置中的大容量[部署 Always On VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)部分中，两个额外已成功完成到 Always On VPN 从 DirectAccess 迁移所需的步骤。 

您必须确保**VPN_Profile.ps1**涉及_后_，以便 VPN 客户端不会尝试连接，而它已颁发了证书。 为此，请在执行将在证书中只有这些已注册的用户添加到 VPN 部署准备组中，可用于部署 Always On VPN 配置的脚本。

>[!NOTE] 
>Microsoft 建议你在执行任何用户迁移响铃次数之前测试此过程。

1.  **创建和发布该 VPN 证书，并启用自动注册组策略对象 (GPO)。** 对于传统的、 基于证书的 Windows 10 VPN 部署，证书被颁发给设备或用户，以便它可以连接的身份验证。 如果创建新的身份验证证书并将其自动注册为发布，必须创建并部署配置为 VPN 用户组的自动注册设置的 GPO。 若要配置证书和自动注册的步骤，请参阅[配置服务器基础结构](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)。

2.  **将用户添加到 VPN 用户组。** 添加到 VPN 用户组迁移任何用户。 这些用户，以便他们可以在将来收到任何证书更新迁移它们之后保持该安全组。 继续将用户添加到此组，直到您已从 DirectAccess 的每个用户移到 Always On VPN。 

3.  **标识已接收 VPN 身份验证证书的用户。** 要从迁移 DirectAccess，因此将需要添加用于标识了客户端已收到所需的证书，并已准备好接收 VPN 配置信息。 运行**GetUsersWithCert.ps1**脚本，以添加当前颁发 nonrevoked 证书从指定的模板名称发出到指定的 AD DS 安全组的用户。 例如，在运行**GetUsersWithCert.ps1**脚本中，任何用户颁发有效证书从 VPN 身份验证证书模板添加到 VPN 部署准备就绪的组。

    >[!NOTE] 
    >如果还没有方法来确定客户端已收到所需的证书时，该证书已颁发给用户，从而导致 VPN 连接失败之前可以部署 VPN 配置。 若要避免这种情况下，运行**GetUsersWithCert.ps1**脚本在证书颁发机构或按计划同步已接收到的 VPN 部署准备就绪的组的证书的用户。 然后，将使用该安全组以面向 System Center Configuration Manager 或 Intune，可确保托管客户端不会收到证书之前收到的 VPN 配置中的 VPN 配置部署。
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. 部署 Always On VPN 配置。 为 VPN 颁发身份验证证书，以及运行**GetUsersWithCert.ps1**脚本中，用户添加到 VPN 部署准备就绪的安全组。


| 如果使用的...  | 则... |
| ---- | ---- |
| System Center Configuration Manager | 创建基于该安全组的成员资格用户集合。<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)运算符|
| Intune | 只需为目标的安全组直接后同步。 |
|
    
每次运行**GetUsersWithCert.ps1**配置脚本，则还必须运行更新的安全组成员身份在 System Center Configuration Manager 中的 AD DS 发现规则。 此外，请确保部署集合的成员身份更新频繁发生足够 （脚本和发现规则与对齐）。

有关使用 System Center Configuration Manager 或 Intune 部署到 Windows 客户端的 Always On VPN 的其他信息，请参阅[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。 请务必，但是，若要将合并这些特定于迁移的任务。

>[!NOTE] 
>将合并这些特定于迁移的任务是一个简单的 Always On VPN 部署和迁移至 Always On VPN 之间的关键差异。 请务必正确定义要针对的安全组，而不是使用部署指南 》 中的方法的集合。


## <a name="remove-devices-from-the-directaccess-security-group"></a>从 DirectAccess 安全组中删除设备

用户收到身份验证证书并**VPN_Profile.ps1**配置脚本，您将看到相应的 System Center Configuration Manager 或 Intune 中的成功 VPN 配置脚本部署。 每个部署后删除该用户的设备从 DirectAccess 安全组，以便你可稍后删除 DirectAccess。 Intune 和 System Center Configuration Manager 包含可帮助您确定每个用户的设备的用户设备分配信息。

>[!NOTE] 
>如果您要应用 DirectAccess Gpo 通过组织单位 (Ou) 而不计算机组移出 OU 的用户的计算机对象。

## <a name="decommission-the-directaccess-infrastructure"></a>取消配置 DirectAccess 基础结构

完成后将所有 DirectAccess 客户端都迁移到 Always On VPN 后，可以取消配置 DirectAccess 基础结构，并从组策略中删除的 DirectAccess 设置。 Microsoft 建议执行以下步骤以从环境中正常删除 DirectAccess:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **清除 DNS。** 请务必从内部 DNS 服务器中删除任何记录，公共 DNS 服务器与 DirectAccess，例如，DA.contoso.com，DAGateway.contoso.com。

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **从 Active Directory 证书服务中删除任何 DirectAccess 证书。** 如果你将计算机证书用于 DirectAccess 实现中，从证书颁发机构控制台中的证书模板文件夹中删除已发布的模板。

## <a name="next-step"></a>下一步
完成从 DirectAccess 迁移到 Always On VPN。 

---
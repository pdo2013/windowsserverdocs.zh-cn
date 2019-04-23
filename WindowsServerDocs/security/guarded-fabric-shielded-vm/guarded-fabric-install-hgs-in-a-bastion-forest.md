---
title: 在现有的堡垒林中安装 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842218"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>在现有的堡垒林中安装 HGS 

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>HGS 服务器加入现有域

在现有的堡垒林中，HGS 必须添加到根域中。 使用服务器管理器或[Add-computer](https://go.microsoft.com/fwlink/?LinkId=821564)将 HGS 服务器加入到根域。

## <a name="add-the-hgs-server-role"></a>添加 HGS 服务器角色

在提升的 PowerShell 会话中运行本主题中的所有命令。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

如果你的数据中心具有你想要加入 HGS 的节点的安全堡垒林，请执行以下步骤。
这些步骤还可用于配置已加入到同一个域的 2 个或更多独立 HGS 群集。

## <a name="join-the-hgs-server-to-the-existing-domain"></a>HGS 服务器加入现有域

使用服务器管理器或[Add-computer](https://go.microsoft.com/fwlink/?LinkId=821564) HGS 服务器加入所需的域。

## <a name="prepare-active-directory-objects"></a>准备 Active Directory 对象

创建组托管服务帐户和 2 个安全组。
您也可以预暂存群集对象如果正在初始化与 HGS 的帐户不具有在域中创建计算机对象的权限。

## <a name="group-managed-service-account"></a>组托管服务帐户

组托管服务帐户 (gMSA) 是由 HGS 来检索，并使用其证书的标识。 使用[New-adserviceaccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount)创建 gMSA。
如果这是第一个 gMSA 的域中，您需要添加一个密钥分发服务根密钥。

HGS 的每个节点将需要允许将其用于访问 gMSA 密码。
此配置的最简单方法是创建一个包含所有 HGS 节点的安全组，并授予该安全组访问权限，以检索 gMSA 密码。

将其添加到安全组，以确保获取其新的组成员身份后，必须重启 HGS 服务器。

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

GMSA 将需要在每个 HGS 服务器上的安全日志中生成事件的权限。
如果您使用组策略配置用户权限分配，请确保 gMSA 帐户授予[生成审核事件特权](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29)HGS 服务器上。

> [!NOTE]
> 组托管的服务帐户是 Windows Server 2012 Active Directory 架构开始可用。
> 有关详细信息，请参阅[组托管服务帐户要求](https://technet.microsoft.com/library/jj128431.aspx)。

## <a name="jea-security-groups"></a>JEA 安全组

当设置 HGS， [Just Enough Administration (JEA)](https://aka.ms/JEAdocs) PowerShell 终结点配置为允许管理员能够管理 HGS 而无需完整的本地管理员权限。
不需要使用 JEA 管理 HGS，但它仍必须配置运行初始化 HgsServer 时。
指定包含你的 HGS 管理员和 HGS 审阅者的 2 个安全组包含的 JEA 终结点配置。
属于管理员组的用户可以添加、 更改或删除 HGS; 上的策略审阅者只能查看当前配置。

创建两个安全组为使用 Active Directory 管理员工具这些 JEA 组或[New-adgroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup)。

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>群集对象

如果您用来设置 HGS 的帐户没有在域中创建新的计算机对象的权限，需要预暂存群集对象。
中说明了这些步骤[Active Directory 域服务中预留群集计算机对象](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx)。

若要设置你的第一个 HGS 节点，您需要创建一个群集名称对象 (CNO) 和一个虚拟计算机对象 (VCO)。
CNO 表示群集的名称，并主要由故障转移群集在内部使用。
VCO 表示 HGS 服务驻留在群集之上，并且将注册到 DNS 服务器的名称。

> [!IMPORTANT]
> 将运行的用户`Initialize-HgsServer`需要**完全控制**通过 Active Directory 中的 CNO 或 VCO 对象。

若要快速预安排 CNO 和 VCO，具有 Active Directory 管理员运行以下 PowerShell 命令：

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>安全基线异常

如果到高锁定环境部署 HGS，某些组策略设置可能会阻止 HGS 正常运行。
检查以下设置在组策略对象，并遵循的指导，如果您的影响：

### <a name="network-logon"></a>网络登录

**策略路径：** 计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配

**策略名称：** 拒绝从网络访问这台计算机

**必需的值：** 请确保值不会阻止所有本地帐户的网络登录。 但是可以安全地阻止本地管理员帐户。

**原因：** 故障转移群集依赖于调用 CLIUSR 管理群集的节点的非管理员本地帐户。 为此用户的阻止网络登录将阻止群集正常运行。

### <a name="kerberos-encryption"></a>Kerberos 加密

**策略路径：** 计算机配置\Windows 设置\安全设置\本地策略\安全选项

**策略名称：** 网络安全：配置 Kerberos 允许的加密类型

**操作**:如果配置此策略，则必须更新使用 gMSA 帐户[Set-adserviceaccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps)要在此策略中使用仅支持的加密类型。 例如，如果您的策略只允许 AES128\_HMAC\_SHA1 和 AES256\_HMAC\_SHA1，应运行`Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`。



## <a name="next-steps"></a>后续步骤

- 若要设置基于 TPM 的认证的下一个步骤，请参阅[初始化 HGS 群集在现有的堡垒林中使用 TPM 模式](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)。
- 有关设置主机密钥证明的下一个步骤，请参阅[初始化 HGS 群集在现有的堡垒林中使用键模式](guarded-fabric-initialize-hgs-key-mode-bastion.md)。
- 在接下来的步骤来设置基于管理员的证明 （不推荐在 Windows Server 2019），请参阅[初始化 HGS 群集使用现有的堡垒林中的 AD 模式](guarded-fabric-initialize-hgs-ad-mode-bastion.md)。


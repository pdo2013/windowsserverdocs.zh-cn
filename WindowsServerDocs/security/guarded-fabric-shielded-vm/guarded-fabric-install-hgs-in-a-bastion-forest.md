---
title: 在现有堡垒林中安装 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5c503331893dbea65c99d79eb7444893d5f3b657
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403598"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>在现有堡垒林中安装 HGS 

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>将 HGS 服务器加入到现有域中

在现有的堡垒林中，必须将 HGS 添加到根域。 使用服务器管理器或[添加计算机](https://go.microsoft.com/fwlink/?LinkId=821564)将 HGS 服务器加入到根域。

## <a name="add-the-hgs-server-role"></a>添加 HGS 服务器角色

在已提升权限的 PowerShell 会话中运行本主题中的所有命令。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

如果你的数据中心有要加入 HGS 节点的安全堡垒林，请执行以下步骤。
你还可以使用这些步骤配置2个或更多个连接到同一域的独立 HGS 群集。

## <a name="join-the-hgs-server-to-the-existing-domain"></a>将 HGS 服务器加入到现有域中

使用服务器管理器或[添加计算机](https://go.microsoft.com/fwlink/?LinkId=821564)将 HGS 服务器联接到所需的域。

## <a name="prepare-active-directory-objects"></a>准备 Active Directory 对象

创建组托管服务帐户和2个安全组。
如果用来初始化 HGS 的帐户不具有在域中创建计算机对象的权限，则还可以预暂存群集对象。

## <a name="group-managed-service-account"></a>组托管服务帐户

组托管服务帐户（gMSA）是 HGS 用来检索和使用其证书的标识。 使用[uninstall-adserviceaccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount)创建 gMSA。
如果这是域中的第一个 gMSA，则需要添加密钥分发服务根密钥。

需要允许每个 HGS 节点访问 gMSA 密码。
配置此配置的最简单方法是创建一个安全组，其中包含所有 HGS 节点并授予该安全组对检索 gMSA 密码的访问权限。

将 HGS 服务器添加到安全组后，必须重新启动它，以确保其获得新的组成员身份。

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
如果使用组策略来配置用户权限分配，请确保在你的 HGS 服务器上为 gMSA 帐户授予 "[生成审核事件" 特权](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29)。

> [!NOTE]
> 组托管服务帐户从 Windows Server 2012 Active Directory 架构开始可用。
> 有关详细信息，请参阅[组托管服务帐户要求](https://technet.microsoft.com/library/jj128431.aspx)。

## <a name="jea-security-groups"></a>JEA 安全组

设置 HGS 后，就会将[足够多的管理（JEA）](https://aka.ms/JEAdocs) PowerShell 终结点配置为允许管理员管理 HGS，无需具有完全的本地管理员权限。
不需要使用 JEA 来管理 HGS，但必须在运行 HgsServer 时进行配置。
JEA 终结点的配置包括指定包含 HGS 管理员和 HGS 审阅者的2个安全组。
属于管理员组的用户可以在 HGS 上添加、更改或删除策略;审阅者只能查看当前配置。

使用 Active Directory 管理工具或[new-adgroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup)为这些 JEA 组创建2个安全组。

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>群集对象

如果用于设置 HGS 的帐户不具有在域中创建新计算机对象的权限，则需要预先暂存群集对象。
[在 Active Directory 域服务中预留群集计算机对象](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx)中介绍了这些步骤。

若要设置你的第一个 HGS 节点，你将需要创建一个群集名称对象（CNO）和一个虚拟计算机对象（VCO）。
CNO 表示群集的名称，主要由故障转移群集内部使用。
VCO 表示位于群集顶层的 HGS 服务，它将是注册到 DNS 服务器的名称。

> [!IMPORTANT]
> 将运行 `Initialize-HgsServer` 的用户需要**完全控制**Active Directory 中的 CNO 和 VCO 对象。

若要快速预留 CNO 和 VCO，请 Active Directory 管理员运行以下 PowerShell 命令：

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

如果要将 HGS 部署到高度锁定的环境中，某些组策略设置可能会阻止 HGS 正常运行。
检查组策略对象中是否有以下设置，如果受到影响，请遵循指导：

### <a name="network-logon"></a>网络登录

**策略路径：** 计算机配置 \Windows 设置 \ 安全设置 \ 本地策略 \ 权限分配

**策略名称：** 拒绝从网络访问这台计算机

**必需的值：** 确保该值不会阻止所有本地帐户的网络登录。 不过，你可以安全地阻止本地管理员帐户。

**在于**故障转移群集依赖于名为 CLIUSR 的非管理员本地帐户来管理群集节点。 阻止此用户的网络登录将阻止群集正常运行。

### <a name="kerberos-encryption"></a>Kerberos 加密

**策略路径：** 计算机配置\Windows 设置\安全设置\本地策略\安全选项

**策略名称：** 网络安全：配置 Kerberos 允许的加密类型

**操作**:如果配置了此策略，则必须使用[uninstall-adserviceaccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps)将 gMSA 帐户更新为仅在此策略中使用受支持的加密类型。 例如，如果你的策略仅允许 AES128 @ no__t-0HMAC @ no__t-1SHA1 and AES256 @ no__t-2HMAC @ no__t-3SHA1，则应运行 @no__t。



## <a name="next-steps"></a>后续步骤

- 有关设置基于 TPM 的证明的后续步骤，请参阅[在现有堡垒林中使用 TPM 模式初始化 HGS 群集](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)。
- 有关设置主机密钥证明的后续步骤，请参阅[使用现有堡垒林中的密钥模式初始化 HGS 群集](guarded-fabric-initialize-hgs-key-mode-bastion.md)。
- 有关设置基于管理员的证明的后续步骤（在 Windows Server 2019 中不推荐使用），请参阅[在现有堡垒林中使用 AD 模式初始化 HGS 群集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)。


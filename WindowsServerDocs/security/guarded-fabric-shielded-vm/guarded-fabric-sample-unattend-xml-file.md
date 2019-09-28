---
title: 创建操作系统专用化答案文件
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4920f9a90bd0190d390a9d35b3d265023d69efac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386502"
---
# <a name="create-os-specialization-answer-file"></a>创建操作系统专用化答案文件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

准备部署受防护的 Vm 时，可能需要创建操作系统专用化应答文件。 在 Windows 上，这通常称为 "unattend.xml" 文件。 **ShieldingDataAnswerFile** Windows PowerShell 函数可帮助你执行此操作。 然后，在使用 System Center Virtual Machine Manager （或任何其他结构控制器）通过模板创建受防护的 Vm 时，可以使用应答文件。

有关受防护的 Vm 的无人参与文件的一般准则，请参阅[创建应答文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>下载 ShieldingDataAnswerFile 函数

可以从[PowerShell 库](https://aka.ms/gftools)获取**ShieldingDataAnswerFile**函数。 如果你的计算机具有 Internet 连接，则可以通过以下命令从 PowerShell 安装它：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

@No__t-0 输出可以打包到防护数据和其他项目，以便可以使用它从模板创建受防护的 Vm。

以下部分说明了如何对包含各种选项的 @no__t 0 文件使用函数参数：

- [基本 Windows 应答文件](#basic-windows-answer-file)
- [带有域加入的 Windows 应答文件](#windows-answer-file-with-domain-join)
- [具有静态 IPv4 地址的 Windows 应答文件](#windows-answer-file-with-static-ipv4-addresses)
- [带有自定义区域设置的 Windows 应答文件](#windows-answer-file-with-a-custom-locale)
- [基本 Linux 答案文件](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>基本 Windows 应答文件

以下命令将创建一个 Windows 应答文件，只需设置管理员帐户密码和主机名。
VM 网络适配器将使用 DHCP 获取 IP 地址，并且不会将 VM 加入 Active Directory 域。
当系统提示输入管理员凭据时，请指定所需的用户名和密码。
如果要配置内置管理员帐户，请使用 "管理员" 作为用户名。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>带有域加入的 Windows 应答文件

以下命令将创建一个 Windows 应答文件，用于将受防护的 VM 加入到 Active Directory 域。
VM 网络适配器将使用 DHCP 来获取 IP 地址。

第一个凭据提示将要求提供本地管理员帐户信息。
如果要配置内置管理员帐户，请使用 "管理员" 作为用户名。

第二个凭据提示将要求具有将计算机加入到 Active Directory 域的权限的凭据。

请确保将 "-DomainName" 参数的值更改为 Active Directory 域的 FQDN。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>具有静态 IPv4 地址的 Windows 应答文件

以下命令将创建一个 Windows 应答文件，该文件使用由结构管理器在部署时提供的静态 IP 地址，如 System Center Virtual Machine Manager。

Virtual Machine Manager 通过使用 IP 池向静态 IP 地址提供三个组件：IPv4 地址、IPv6 地址、网关地址和 DNS 地址。 如果希望包括任何其他字段或需要自定义网络配置，则需要手动编辑脚本生成的答案文件。

以下屏幕截图显示了可在 Virtual Machine Manager 中配置的 IP 池。 如果要使用静态 IP，则这些池是必需的。

目前，此函数仅支持一个 DNS 服务器。 DNS 设置如下所示：

![为 DNS 服务器配置静态 IP 池](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

下面是创建静态 IP 地址池时的摘要。 简而言之，你必须只有一个网络路由、一个网关和一个 DNS 服务器，并且你必须指定你的 IP 地址。

![静态 IP 池创建摘要](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

需要为虚拟机配置网络适配器。 以下屏幕截图显示了设置该配置的位置，以及如何将其切换为静态 IP。

![将硬件配置为使用静态 IP](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

然后，可以使用 @no__t 参数将静态 IP 元素包含在答案文件中。 然后，应答文件中的参数 `@IPAddr-1@`、`@NextHop-1-1@` 和 `@DNSAddr-1-1@` 将替换为在部署时 Virtual Machine Manager 中指定的实际值。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>带有自定义区域设置的 Windows 应答文件

以下命令使用自定义区域设置创建 Windows 应答文件。

当系统提示输入管理员凭据时，请指定所需的用户名和密码。
如果要配置内置管理员帐户，请使用 "管理员" 作为用户名。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>基本 Linux 答案文件

从 Windows Server 版本1709开始，你可以在受防护的 Vm 中运行特定的 Linux 来宾操作系统。
如果使用 System Center Virtual Machine Manager Linux 代理来专用化这些 Vm，ShieldingDataAnswerFile cmdlet 可为其创建兼容的答案文件。

在 Linux 应答文件中，通常会包括 root 密码、根 SSH 密钥和（可选）静态 IP 池信息。
运行以下脚本之前，请将路径替换为 SSH 密钥的公共一半。

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>请参阅

- [部署受防护的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)

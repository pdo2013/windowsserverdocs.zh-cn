---
title: 创建操作系统专用化答案文件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1d9e91ec8f4c998f34e324b5d551a387eba5a310
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823628"
---
# <a name="create-os-specialization-answer-file"></a>创建操作系统专用化答案文件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在准备部署受防护的 Vm，可能需要创建的操作系统专用化答案文件。 在 Windows 中，这通常称为"unattend.xml"文件。 **新建 ShieldingDataAnswerFile** Windows PowerShell 函数可帮助你执行此操作。 你可以然后使用答案文件时要创建受防护的 Vm 模板从使用 System Center Virtual Machine Manager （或任何其他结构控制器）。

受防护的 Vm 的无人参与文件的常规指南，请参阅[创建一个答案文件](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>下载新建 ShieldingDataAnswerFile 函数

你可以获取**新建 ShieldingDataAnswerFile**函数从[PowerShell 库](https://aka.ms/gftools)。 如果您的计算机具有 Internet 连接，则可以安装它从 PowerShell 使用以下命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

`unattend.xml` ，以便它可以用于从模板创建受防护的 Vm，可以为屏蔽的数据，以及其他项目一起打包的输出。

以下部分说明如何使用的函数参数`unattend.xml`文件包含各种选项：

- [基本 Windows 答案文件](#basic-windows-answer-file)
- [Windows 答案文件与域加入](#windows-answer-file-with-domain-join)
- [Windows 答案文件与静态 IPv4 地址](#windows-answer-file-with-static-ipv4-addresses)
- [使用自定义区域设置的 Windows 答案文件](#windows-answer-file-with-custom-locale)
- [基本 Linux 答案文件](#basic-linux-answer-file)

此外可以查看[函数参数](#function-parameters)，本主题中更高版本。

## <a name="basic-windows-answer-file"></a>基本 Windows 答案文件

以下命令创建只需设置的管理员帐户密码和主机名的 Windows 答案文件。
VM 网络适配器将使用 DHCP 获取 IP 地址和 VM 将未加入到 Active Directory 域。
当系统提示输入管理员凭据，指定所需的用户名和密码。
使用"Administrator"作为用户名，如果你想要配置的内置管理员帐户。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Windows 答案文件与域加入

以下命令创建受防护的虚拟机加入到 Active Directory 域的 Windows 答案文件。
VM 网络适配器将使用 DHCP 获取 IP 地址。

第一次凭据提示将要求提供本地管理员帐户信息。
使用"Administrator"作为用户名，如果你想要配置的内置管理员帐户。

第二个凭据提示将要求提供有权将计算机加入到 Active Directory 域的凭据。

请务必更改的值"-DomainName"参数为你的 Active Directory 域的 FQDN。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"
$domainCred = Get-Credential -Prompt "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Windows 答案文件与静态 IPv4 地址

以下命令创建使用静态 IP 地址结构管理器，如 System Center Virtual Machine Manager 在部署时提供的 Windows 答案文件。

Virtual Machine Manager 使用的 IP 池提供静态 IP 地址的三个组件：IPv4 地址、 IPv6 地址、 网关地址和 DNS 地址。 如果你想要包含或需要自定义网络配置的任何其他字段，你将需要手动编辑应答文件脚本生成。

以下屏幕截图显示的 IP 池，您可以配置在 Virtual Machine Manager。 这些池是必需的如果你想要使用静态 IP。

目前，此函数支持只有一台 DNS 服务器。 下面是在 DNS 设置将如下所示：

![使用静态 IP 池配置 DNS 服务器](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

下面是用于创建静态 IP 地址池将摘要将如下所示。 简单地说，您必须具有只有一个网络路由、 一个网关，以及一台 DNS 服务器的并且必须指定你的 IP 地址。

![静态 IP 池创建摘要](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

需要配置为虚拟机网络适配器。 以下屏幕截图显示设置该配置的位置以及如何切换到静态 IP。

![配置为使用静态 IP 的硬件](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

然后，可以使用`-StaticIPPool`参数在答案文件中包含的静态 IP 元素。 参数`@IPAddr-1@`， `@NextHop-1-1@`，和`@DNSAddr-1-1@`在答案文件将替换为在部署时指定 Virtual Machine Manager 中的实际值。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>使用自定义区域设置的 Windows 答案文件

以下命令创建了自定义区域设置 Windows 应答文件。

当系统提示输入管理员凭据，指定所需的用户名和密码。
使用"Administrator"作为用户名，如果你想要配置的内置管理员帐户。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"
$domainCred = Get-Credential -Prompt "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>基本 Linux 答案文件

从 Windows Server 1709 版开始，可以运行受防护的 Vm 中的某些 Linux 来宾操作系统。
如果使用 System Center Virtual Machine Manager Linux 代理以使这些 Vm 的专用化，则新建 ShieldingDataAnswerFile cmdlet 可以为其创建兼容答案文件。

在 Linux 答案文件中，将通常包括根密码、 根 SSH 密钥，并根据需要静态 IP 池信息。
运行下面的脚本之前将为你的 SSH 密钥的公共部分的路径。

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>请参阅

- [部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)

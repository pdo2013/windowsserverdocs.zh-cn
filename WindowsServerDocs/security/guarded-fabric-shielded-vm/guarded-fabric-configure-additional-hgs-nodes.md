---
title: 配置其他 HGS 节点
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: fe9cd63f08da849c2cb002ab853501370d736f0f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870589"
---
# <a name="configure-additional-hgs-nodes"></a>配置其他 HGS 节点

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在生产环境中，应在高可用性群集中设置 HGS，以确保即使在 HGS 节点出现故障的情况下，也可以打开受防护的 Vm。 对于测试环境，不需要辅助 HGS 节点。

使用其中一种方法添加 HGS 节点，最适合自己的环境。

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|新建 HGS 林  | [使用 PFX 文件](#dedicated-hgs-forest-with-pfx-certificates) | [使用证书指纹](#dedicated-hgs-forest-with-certificate-thumbprints) |
|现有堡垒林 |  [使用 PFX 文件](#existing-bastion-forest-with-pfx-certificates) | [使用证书指纹](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>先决条件

请确保每个附加节点： 
- 与主节点具有相同的硬件和软件配置 
- 连接到与其他 HGS 服务器相同的网络
- 可以按其 DNS 名称解析其他 HGS 服务器

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>具有 PFX 证书的专用 HGS 林

1. 将 HGS 节点提升为域控制器
2. 初始化 HGS 服务器

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>将 HGS 节点提升为域控制器

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>具有证书指纹的专用 HGS 林
 
1. 将 HGS 节点提升为域控制器
2. 初始化 HGS 服务器
3. 安装证书的私钥

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>将 HGS 节点提升为域控制器

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>安装证书的私钥

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>具有 PFX 证书的现有堡垒林

1. 将节点加入到现有域中
2. 授予计算机检索 gMSA 密码的权限，并运行 Uninstall-adserviceaccount
3. 初始化 HGS 服务器

### <a name="join-the-node-to-the-existing-domain"></a>将节点加入到现有域中

1. 确保该节点上至少有一个 NIC 配置为使用第一个 HGS 服务器上的 DNS 服务器。
2. 将新的 HGS 节点加入到与第一个 HGS 节点相同的域中。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授予计算机检索 gMSA 密码的权限，并运行 Uninstall-adserviceaccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>具有证书指纹的现有堡垒林

1. 将节点加入到现有域中
2. 授予计算机检索 gMSA 密码的权限，并运行 Uninstall-adserviceaccount
3. 初始化 HGS 服务器
4. 安装证书的私钥

### <a name="join-the-node-to-the-existing-domain"></a>将节点加入到现有域中

1. 确保该节点上至少有一个 NIC 配置为使用第一个 HGS 服务器上的 DNS 服务器。
2. 将新的 HGS 节点加入到与第一个 HGS 节点相同的域中。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授予计算机检索 gMSA 密码的权限，并运行 Uninstall-adserviceaccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

将第一个 HGS 服务器中的加密和签名证书复制到此节点最多需要10分钟时间。

### <a name="install-the-private-keys-for-the-certificates"></a>安装证书的私钥

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>为 HTTPS 通信配置 HGS

如果要使用 SSL 证书保护 HGS 终结点，则必须在此节点上以及 HGS 群集中的每个其他节点上配置 SSL 证书。
SSL 证书*不会*由 HGS 复制，并且不需要为每个节点使用相同的密钥（即每个节点都有不同的 SSL 证书）。

请求 SSL 证书时，请确保群集完全限定的域名（如的输出`Get-HgsServer`中所示）是证书的使用者公用名，或包含为使用者可选 DNS 名称。
从证书颁发机构获取证书后，可以将 HGS 配置为使用[HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver)。

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

如果已将证书安装到本地证书存储中，并且想要按指纹引用证书，请改为运行以下命令：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS 将始终公开用于通信的 HTTP 和 HTTPS 端口。
不支持在 IIS 中删除 HTTP 绑定，但是可以使用 Windows 防火墙或其他网络防火墙技术来阻止端口80上的通信。

## <a name="decommission-an-hgs-node"></a>停止 HGS 节点

若要解除 HGS 节点的授权：

1. [清除 "HGS 配置"](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)。

   这会从群集中删除节点并卸载证明和密钥保护服务。 
   如果这是群集中的最后一个节点，则需要强制要求删除最后一个节点并销毁 Active Directory 中的群集。 
   
   如果在堡垒林中部署了 HGS （默认值），则这是唯一的步骤。 
   你可以选择性地从域中脱离计算机，并从 Active Directory 中删除 gMSA 帐户。

1. 如果 HGS 创建了自己的域，则还应[卸载 hgs](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)来脱离域并降级域控制器。



## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [验证 HGS 配置](guarded-fabric-verify-hgs-configuration.md)


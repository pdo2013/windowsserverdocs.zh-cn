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
ms.openlocfilehash: c0741845bdbd8bfbea00df21d1fe810a27fc6a3b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443859"
---
# <a name="configure-additional-hgs-nodes"></a>配置其他 HGS 节点

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在生产环境中，HGS 应设置高可用性群集中以确保即使 HGS 节点出现故障可以在支持受防护的 Vm。 对于测试环境，不需要 HGS 的辅助节点。

使用下列方法之一添加 HGS 节点，为最适合您的环境。

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|新的 HGS 林  | [使用 PFX 文件](#dedicated-hgs-forest-with-pfx-certificates) | [使用证书指纹](#dedicated-hgs-forest-with-certificate-thumbprints) |
|现有堡垒林 |  [使用 PFX 文件](#existing-bastion-forest-with-pfx-certificates) | [使用证书指纹](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>先决条件

请确保每个其他节点： 
- 具有与主节点相同的硬件和软件配置 
- 连接到其他 HGS 服务器所在的网络
- 可以通过其 DNS 名称来解决其他 HGS 服务器

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>使用 PFX 证书的专用的 HGS 林

1. 将提升到域控制器的 HGS 节点
2. 初始化 HGS 服务器

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>将提升到域控制器的 HGS 节点

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>使用证书指纹的专用的 HGS 林
 
1. 将提升到域控制器的 HGS 节点
2. 初始化 HGS 服务器
3. 安装证书的私钥

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>将提升到域控制器的 HGS 节点

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>安装证书的私钥

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>使用 PFX 证书的现有堡垒林

1. 将节点加入到现有域
2. 授予检索 gMSA 密码并运行 Install-adserviceaccount 计算机权限
3. 初始化 HGS 服务器

### <a name="join-the-node-to-the-existing-domain"></a>将节点加入到现有域

1. 确保在节点上的至少一个 NIC 配置为在第一个 HGS 服务器上使用 DNS 服务器。
2. 将新的 HGS 节点加入到与你的第一个 HGS 节点相同的域。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授予检索 gMSA 密码并运行 Install-adserviceaccount 计算机权限

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>使用证书指纹的现有堡垒林

1. 将节点加入到现有域
2. 授予检索 gMSA 密码并运行 Install-adserviceaccount 计算机权限
3. 初始化 HGS 服务器
4. 安装证书的私钥

### <a name="join-the-node-to-the-existing-domain"></a>将节点加入到现有域

1. 确保在节点上的至少一个 NIC 配置为在第一个 HGS 服务器上使用 DNS 服务器。
2. 将新的 HGS 节点加入到与你的第一个 HGS 节点相同的域。 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>授予检索 gMSA 密码并运行 Install-adserviceaccount 计算机权限

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>初始化 HGS 服务器

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

从复制到此节点的第一个 HGS 服务器需要最多 10 分钟进行加密和签名证书。

### <a name="install-the-private-keys-for-the-certificates"></a>安装证书的私钥

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>对于 HTTPS 通信配置 HGS

如果你想要保护 HGS 使用 SSL 证书的终结点，您必须在此节点，以及 HGS 群集中的每个其他节点上配置 SSL 证书。
SSL 证书*不是*HGS 的复制，不需要为 （即可以具有不同的 SSL 证书，每个节点） 的每个节点使用相同的密钥。

在请求的 SSL 证书，请确保群集的完全限定的域名 (如的输出中所示`Get-HgsServer`) 为的使用者公用名称的证书，或作为使用者可选 DNS 名称。
如果您已从证书颁发机构获取证书，你可以配置 HGS 与其一起使用[集 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver)。

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

如果已将证书安装到本地证书存储区，并想要引用的指纹，请改为运行以下命令：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS 始终将公开用于通信的 HTTP 和 HTTPS 端口。
它不支持将在 IIS 中，删除 HTTP 绑定，但是您可以使用 Windows 防火墙或其他网络防火墙技术来阻止通过端口 80 的通信。

## <a name="decommission-an-hgs-node"></a>取消配置 HGS 节点

若要取消配置 HGS 节点：

1. [清除 HGS 配置](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)。

   这将从群集删除节点，并卸载证明和密钥保护服务。 
   如果它是在群集中的最后一个节点，-Force 需要以表示要删除的最后一个节点，并销毁 Active Directory 中的群集。 
   
   如果 HGS 部署在堡垒林中 （默认值），这是唯一的步骤。 
   （可选） 可以脱离域的计算机，并从 Active Directory 中删除 gMSA 帐户。

1. 如果 HGS 创建其自己的域，还应该[卸载 HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration)脱离域并降级域控制器。



## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [验证 HGS 配置](guarded-fabric-verify-hgs-configuration.md)


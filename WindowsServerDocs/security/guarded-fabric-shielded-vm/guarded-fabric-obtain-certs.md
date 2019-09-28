---
title: 获取 HGS 证书
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b3e6aadbcbf2f2b826ca97d4ebb58c3736528b59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386517"
---
# <a name="obtain-certificates-for-hgs"></a>获取 HGS 证书

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

部署 HGS 时，系统将要求你提供签名和加密证书，这些证书用于保护启动受防护的 VM 所需的敏感信息。
这些证书永远不会离开 HGS，并且仅在运行它们的主机已证实其运行正常时用于解密受防护的 VM 密钥。
租户（VM 所有者）使用公用一半证书来授权你的数据中心运行其受防护的 Vm。
本部分介绍为 HGS 获取兼容的签名和加密证书所需的步骤。

## <a name="request-certificates-from-your-certificate-authority"></a>申请证书颁发机构颁发的证书

虽然不是必需的，但强烈建议从受信任的证书颁发机构获取证书。
这样做有助于 VM 所有者验证是否正在授权正确的 HGS 服务器（即服务提供商或数据中心）来运行其受防护的 Vm。
在企业方案中，可以选择使用自己的企业 CA 颁发这些证书。
托管商和服务提供商应考虑改用众所周知的公共 CA。

签名和加密证书必须使用以下 certificiate 属性颁发（除非标记为 "推荐"）：

证书模板属性 | 必需的值 
------------------------------|----------------
加密提供程序               | 任何密钥存储提供程序（KSP）。 **不**支持旧的加密服务提供程序（csp）。
密钥算法                 | RSA
最小密钥大小              | 2048位
签名算法           | 建议：SHA256
密钥用法                     | 数字签名*和*数据加密
增强型密钥用法            | 服务器身份验证
密钥续订策略            | 用相同的密钥续订。 续订包含不同密钥的 HGS 证书会阻止受防护的 Vm 启动。
使用者名称                  | 建议：你的公司名称或 web 地址。 此信息将显示在 "防护数据文件" 向导中的 "VM 所有者"。

无论你使用的是硬件还是软件支持的证书，这些要求都适用。
出于安全原因，建议你在硬件安全模块（HSM）中创建 HGS 密钥，以防止私钥从系统复制。
按照 HSM 供应商提供的指导请求具有上述属性的证书，并确保在每个 HGS 节点上安装并授权 HSM KSP。

每个 HGS 节点将需要访问相同的签名证书和加密证书。
如果使用的是软件支持的证书，则可将证书导出到具有密码的 PFX 文件，并允许 HGS 管理证书。
你还可以选择在每个 HGS 节点上的本地计算机证书存储中安装证书，并将指纹提供给 HGS。
在[初始化 HGS 群集](guarded-fabric-initialize-hgs.md)主题中介绍了这两个选项。

## <a name="create-self-signed-certificates-for-test-scenarios"></a>为测试方案创建自签名证书

如果创建的是 HGS 实验室环境，而不想使用证书颁发机构，则可以创建自签名证书。
导入防护数据文件向导中的证书信息时，您将收到警告，但所有功能都将保持不变。

若要创建自签名证书并将其导出到 PFX 文件，请在 PowerShell 中运行以下命令：

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>请求一个 SSL 证书

在 Hyper-v 主机和 HGS 之间传输的所有密钥和敏感信息都在消息级别进行加密，也就是说，信息是通过名为 HGS 或 Hyper-v 的密钥进行加密的，这是为了防止别人探查网络流量和偷窃密钥到 Vm。
但是，如果你具有相容性 reqiurements，或者只是想要对 Hyper-v 与 HGS 之间的所有通信进行加密，则可以使用 SSL 证书配置 HGS，该证书将加密传输级别的所有数据。

Hyper-v 主机和 HGS 节点都需要信任你提供的 SSL 证书，因此建议你从企业证书颁发机构申请 SSL 证书。 请求证书时，请务必指定以下各项：

SSL 证书属性 | 必需的值
-------------------------|---------------
使用者名称             | HGS 群集的名称（分布式网络名称）。 这将是提供给 `Initialize-HgsServer` 和你的 HGS 域名的 HGS 服务名称的串联。
使用者可选名称 | 如果要使用不同的 DNS 名称来访问 HGS 群集（例如，如果它位于负载均衡器后面），请确保在证书请求的 SAN 字段中包含这些 DNS 名称。

在[配置第一个 hgs 节点](guarded-fabric-initialize-hgs.md)时，会介绍用于指定此证书的选项。
你还可以在以后使用[HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet 添加或更改 SSL 证书。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [安装 HGS](guarded-fabric-choose-where-to-install-hgs.md)
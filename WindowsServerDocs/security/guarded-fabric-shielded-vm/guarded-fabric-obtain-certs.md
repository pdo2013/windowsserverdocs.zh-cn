---
title: 获取用于 HGS 证书
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2dc232eb7aeb8b0807a8e9989ae3dc893f925f66
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447362"
---
# <a name="obtain-certificates-for-hgs"></a>获取用于 HGS 证书

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

当部署 HGS 时，将需要提供用于保护敏感信息启动受防护的 VM 所需的签名和加密证书。
这些证书永远不会保留 HGS，并仅解密受防护的 VM 的键时使用它们运行所在的主机已被证明处于正常状态。
租户 （VM 所有者） 使用公共证书的下半部分授权你的数据中心运行受防护的 Vm。
本部分介绍获取 HGS 兼容的签名和加密证书所需的步骤。

## <a name="request-certificates-from-your-certificate-authority"></a>从证书颁发机构请求证书

尽管不要求这样做，强烈建议您从受信任的证书颁发机构获取证书。
这样做可帮助验证它们将授权正确 HGS 服务器 （即服务提供商或数据中心） 运行其受防护的 Vm 的 VM 所有者。
在企业方案中，您可能选择使用您自己的企业 CA 来颁发这些证书。
宿主和服务提供商应考虑改为使用已知的公共 CA。

（除非标记为"建议"），必须具有以下端证书属性颁发的签名和加密证书：

证书模板属性 | 所需的值 
------------------------------|----------------
加密提供程序               | 任何密钥存储提供程序 (KSP)。 旧加密服务提供程序 (Csp) 是**不**支持。
密钥算法                 | RSA
最小密钥大小              | 2048 位
签名算法           | 建议：SHA256
密钥用法                     | 数字签名*和*数据加密
增强型密钥用法            | 服务器身份验证
密钥续订策略            | 使用相同密钥续订。 使用不同的密钥续订 HGS 证书将阻止受防护的 Vm 正在启动。
使用者名称                  | 建议： 你公司的名称或 web 地址。 向 VM 防护数据文件向导中的所有者，将显示此信息。

这些要求应用是否正在使用受硬件或软件的证书。
出于安全原因，建议创建 HGS 密钥在硬件安全模块 (HSM) 以防止复制系统关闭的私钥。
请遵循的指导从 HSM 供应商的上述属性具有请求证书并确保进行安装和授权 HSM KSP HGS 的每个节点上。

HGS 的每个节点将需要访问相同的签名和加密证书。
如果要使用支持软件的证书，可以将证书导出到具有密码的 PFX 文件，并允许 HGS 来为你管理的证书。
您还可以选择将证书安装到每个 HGS 节点上的本地计算机的证书存储区并向 HGS 提供指纹。
中介绍了这两个选项[初始化 HGS 群集](guarded-fabric-initialize-hgs.md)主题。

## <a name="create-self-signed-certificates-for-test-scenarios"></a>创建测试方案的自签名的的证书

如果您要创建的 HGS 实验室环境和执行操作或想要使用的证书颁发机构，可以创建自签名的证书。
导入防护数据文件向导中的证书信息时，将收到一条警告，但所有功能将保持都不变。

若要创建自签名的证书并将其导出到 PFX 文件，请在 PowerShell 中运行以下命令：

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>请求 SSL 证书

所有密钥和敏感信息的 HYPER-V 主机之间都传输和 HGS 加密在消息级别--即，使用已知到 HGS 或 HYPER-V，防止有人探查您的网络通信和窃取密钥的密钥进行加密信息到 Vm。
但是，如果你有符合性 reqiurements 或者只是偏好于 HYPER-V 和 HGS 之间的所有通信进行加密，你可以配置 HGS 使用 SSL 证书进行加密所有数据在传输级别的。

HYPER-V 主机和 HGS 节点将需要信任您提供的 SSL 证书，因此建议您从企业证书颁发机构请求 SSL 证书。 在请求证书时，务必指定下列各项：

SSL 证书属性 | 所需的值
-------------------------|---------------
使用者名称             | HGS 群集 （分布式的网络名称） 的名称。 这将为你提供给的 HGS 服务名称的串联`Initialize-HgsServer`和 HGS 域名。
使用者可选名称 | 如果您将使用不同的 DNS 名称来访问 HGS 群集 (例如负载均衡器后面时)，请确保您的证书申请的 SAN 字段中包含这些 DNS 名称。

中介绍了用于初始化 HGS 服务器时指定此证书的选项[配置的第一个 HGS 节点](guarded-fabric-initialize-hgs.md)。
此外可以添加或更改在更高版本时使用的 SSL 证书[集 HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [安装 HGS](guarded-fabric-choose-where-to-install-hgs.md)
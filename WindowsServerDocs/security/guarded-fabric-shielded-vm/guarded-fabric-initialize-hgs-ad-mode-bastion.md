---
title: 初始化 HGS 群集使用在堡垒林中的 AD 模式
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 98003745823cf780a38487dff997798ebef12fc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870088"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>初始化 HGS 群集使用现有的堡垒林中的 AD 模式

>适用于：Windows 服务器 （半年频道），Windows Server 2016


>[!IMPORTANT]
>受信任的管理员证明 （AD 模式） 与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode-bastion.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 

Active Directory 域服务将在计算机上安装，但应保留未配置。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)] 

在继续之前，确保你已为主机保护者服务预备群集对象并授予已登录用户**完全控制**Active Directory 中 VCO 和 CNO 对象。
虚拟计算机对象名称需要传递给`-HgsServiceName`参数与群集名称`-ClusterName`参数。

> [!TIP]
> 仔细检查你的 AD 域控制器，以确保您的群集对象已被复制到所有域控制器然后再继续。

如果要使用基于 PFX 的证书，HGS 服务器上运行以下命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

如果使用 （如、 由 HSM 支持的证书和非可导出的证书） 在本地计算机上安装的证书，使用`-SigningCertificateThumbprint`和`-EncryptionCertificateThumbprint`参数相反。

## <a name="next-step"></a>下一步

>[!div class="nextstepaction"]
[配置 DNS 结构](guarded-fabric-configuring-fabric-dns-ad.md)


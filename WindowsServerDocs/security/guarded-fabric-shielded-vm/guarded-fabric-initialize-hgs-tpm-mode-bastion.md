---
title: 使用堡垒林中的 TPM 模式初始化 HGS 群集
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b360f0f5195bea3c61f9a181b4b75a681f7e29b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403602"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>在现有堡垒林中使用 TPM 模式初始化 HGS 群集

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

Active Directory 域服务将安装在计算机上，但应保留未配置。

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

继续之前，请确保已为主机保护者服务预留群集对象，并授予已登录的用户对 Active Directory 中的 VCO 和 CNO 对象的**完全控制**权限。
需要将虚拟计算机对象名称传递到 @no__t 参数，并将群集名称传递到 `-ClusterName` 参数。

> [!TIP]
> 请仔细检查 AD 域控制器，确保群集对象已复制到所有 Dc，然后再继续。

如果使用的是基于 PFX 的证书，请在 HGS 服务器上运行以下命令：

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

如果使用的是安装在本地计算机上的证书（例如 HSM 支持的证书和不可导出的证书），请改用 `-SigningCertificateThumbprint` 和 `-EncryptionCertificateThumbprint` 参数。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [安装 TPM 根证书](guarded-fabric-install-trusted-tpm-root-certificates.md)
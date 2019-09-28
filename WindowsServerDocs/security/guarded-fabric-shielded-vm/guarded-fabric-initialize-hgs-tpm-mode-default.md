---
title: 在新的专用林中使用 TPM 模式初始化 HGS 群集（默认值）
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 56971f029dc8caa3b0d399230b75285396551390
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403625"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>在新的专用林中使用 TPM 模式初始化 HGS 群集（默认值）

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  在已提升权限的 PowerShell 窗口中，在第一个 HGS 节点上运行[HgsServer](https://technet.microsoft.com/library/mt652185.aspx) 。 此 cmdlet 的语法支持多种不同的输入，但最常见的两个调用如下：

    -   如果使用 PFX 文件进行签名和加密证书，请运行以下命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   如果你使用的是本地证书存储中安装的不可导出的证书，请运行以下命令。 如果你不知道证书的指纹，则可通过运行 `Get-ChildItem Cert:\LocalMachine\My` 列出可用证书。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [安装 TPM 根证书](guarded-fabric-install-trusted-tpm-root-certificates.md)
  
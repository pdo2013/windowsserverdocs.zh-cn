---
title: 初始化 HGS 群集中的新的专用林 （默认值） 使用 TPM 模式
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6d4232b0b248aba8b64f31ac28db1480c14db53d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863868"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>初始化 HGS 群集中的新的专用林 （默认值） 使用 TPM 模式

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  运行[Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)中第一个 HGS 节点上提升的 PowerShell 窗口。 此 cmdlet 的语法支持许多不同的输入，但 2 个最常见的调用如下：

    -   如果使用的签名和加密证书的 PFX 文件，运行以下命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   如果使用不可导出的证书安装在本地证书存储区中，运行以下命令。 如果不知道您的证书的指纹，则可以通过运行列出可用证书`Get-ChildItem Cert:\LocalMachine\My`。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>下一步

>[!div class="nextstepaction"]
[安装 TPM 根证书](guarded-fabric-install-trusted-tpm-root-certificates.md)
  
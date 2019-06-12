---
title: 初始化 HGS 群集在新的专用林 （默认值） 中使用 AD 模式
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7a3d38818bfdaa48f53ca7a54bf10b68e4e4a7d3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447441"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>初始化 HGS 群集在新的专用林 （默认值） 中使用 AD 模式

>适用于：Windows 服务器 （半年频道），Windows Server 2016

>[!IMPORTANT]
>受信任的管理员证明 （AD 模式） 与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode-default.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  运行[Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx)中第一个 HGS 节点上提升的 PowerShell 窗口。 此 cmdlet 的语法支持许多不同的输入，但 2 个最常见的调用如下：

    -   如果使用的签名和加密证书的 PFX 文件，运行以下命令：

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   如果使用不可导出的证书安装在本地证书存储区中，运行以下命令。 如果不知道您的证书的指纹，则可以通过运行列出可用证书`Get-ChildItem Cert:\LocalMachine\My`。

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [配置结构 DNS](guarded-fabric-configuring-fabric-dns-ad.md)
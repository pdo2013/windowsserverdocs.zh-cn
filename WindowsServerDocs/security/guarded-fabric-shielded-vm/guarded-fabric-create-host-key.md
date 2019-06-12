---
title: 创建主机密钥，并将其添加到 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0526831fb0648e7f8f6fb1a081180f2e2aa9f09f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447496"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>创建主机密钥，并将其添加到 HGS

>适用于：Windows Server 2019


本主题介绍如何准备 HYPER-V 主机，成为受保护的主机使用主机密钥证明 （密钥模式）。 你将创建一个主机密钥对 （或使用现有证书），并添加到 HGS 密钥的公共部分。

## <a name="create-a-host-key"></a>创建主机密钥

1.  在 HYPER-V 主机计算机上安装 Windows Server 2019。
2.  安装 HYPER-V 和主机保护者 HYPER-V 支持功能：

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  主机密钥自动生成，或选择现有证书。 如果使用自定义证书，应至少为 2048年位 RSA 密钥、 客户端身份验证 EKU 和数字签名的密钥用法。

    ```powershell
    Set-HgsClientHostKey
    ```

    或者，可以指定一个指纹，如果想要使用你自己的证书。 
    这可以是你想要在多台计算机之间共享证书或使用证书绑定到 TPM 或 HSM 的情况下很有用。 下面是创建 TPM 绑定证书 （这可以防止被盗和另一台计算机上使用私钥并需要仅 TPM 1.2） 的示例：

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  获取公共密钥向 HGS 服务器提供的密钥。 您可以使用以下 cmdlet 或者，如果您有存储在其他位置，该证书提供包含公钥的.cer 一半键。 请注意，我们仅存储和验证 HGS; 上的公钥我们不会保留任何证书信息也不一定要我们验证证书链或到期日期。

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  将.cer 文件复制到 HGS 服务器。

## <a name="add-the-host-key-to-the-attestation-service"></a>将主机密钥添加到证明服务

此步骤的 HGS 服务器上完成，并允许主机运行受防护的 Vm。 建议的名称设置为 FQDN 或上安装的主机，因此你可以轻松地参考的主机密钥的资源标识符。

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认主机可以证明成功](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>请参阅

- [部署主机保护者服务的受保护的主机和受防护的 Vm](guarded-fabric-deploying-hgs-overview.md)

---
title: 安装受信任的 TPM 根证书
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 15614ce1065170bc557fad10a168b3dda6a5b05a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386547"
---
# <a name="install-trusted-tpm-root-certificates"></a>安装受信任的 TPM 根证书

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

将 HGS 配置为使用 TPM 证明时，还需要将 HGS 配置为信任服务器中 Tpm 的供应商。
这一额外的验证过程仅确保可信 Tpm 可以通过您的 HGS 证明。
如果尝试注册 `Add-HgsAttestationTpmHost` 的不受信任的 TPM，你将收到一条错误消息，指示 TPM 供应商不受信任。

要信任你的 Tpm，需要在 HGS 上安装用于签署服务器 Tpm 中认可密钥的根和中间签名证书。
如果在数据中心中使用多个 TPM 模型，则可能需要为每个模型安装不同的证书。
HGS 将在 "TrustedTPM_RootCA" 和 "TrustedTPM_IntermediateCA" 证书存储中查找供应商证书。

> [!NOTE]
> TPM 供应商证书不同于 Windows 中默认安装的证书，表示 TPM 供应商使用的特定根证书和中间证书。

为了方便起见，Microsoft 发布了受信任的 TPM 根证书和中间证书的集合。
你可以使用以下步骤来安装这些证书。
如果以下包中未包含您的 TPM 证书，请与您的 TPM 供应商或服务器 OEM 联系，以获取特定 TPM 型号的根证书和中间证书。

在**每个 HGS 服务器**上重复以下步骤：

1.  从[@no__t](https://go.microsoft.com/fwlink/?linkid=2097925)下载最新的包。

2.  验证 cab 文件的签名，以确保其真实性。 如果签名无效，请不要继续操作。

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    下面是一些示例输出：
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  展开 cab 文件。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  默认情况下，配置脚本将为每个 TPM 供应商安装证书。 如果你只想要为特定 TPM 供应商导入证书，请删除你的组织不信任的 TPM 供应商文件夹。

4.  通过在扩展文件夹中运行安装脚本来安装受信任的证书包。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

若要在之前的安装过程中添加新证书或有意跳过的证书，只需在 HGS 群集中的每个节点上重复上述步骤。
现有证书将保持受信任，但扩展的 cab 文件中找到的新证书将添加到受信任的 TPM 存储中。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [配置结构 DNS](guarded-fabric-configuring-fabric-dns-tpm.md)




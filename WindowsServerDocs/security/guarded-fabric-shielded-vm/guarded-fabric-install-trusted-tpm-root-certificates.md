---
title: 安装受信任的 TPM 根证书
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/11/2018
ms.openlocfilehash: 65c5eb25f7a98e4db6a44e705a2968f185f3f962
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447380"
---
# <a name="install-trusted-tpm-root-certificates"></a>安装受信任的 TPM 根证书

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在配置 HGS 使用 TPM 证明时，还需要配置 HGS 信任您的服务器中 Tpm 的供应商。
此额外的验证过程确保只有可信、 高信度的 Tpm 可与你 HGS 证明。
如果你尝试注册不受信任的 TPM 和`Add-HgsAttestationTpmHost`，您将收到一个错误，指出不受信任的 TPM 供应商。

若要信任你的 Tpm，根和中间用于对在服务器的 Tpm 认可密钥进行签名的签名证书需要安装 HGS 上。
如果在你的数据中心中使用多个 TPM 模型，您可能需要安装每个模型的不同证书。
HGS 看"TrustedTPM_RootCA"和"TrustedTPM_IntermediateCA"证书存储区的供应商证书。

> [!NOTE]
> TPM 供应商证书不同于在 Windows 中默认情况下安装和表示特定的根和中间证书由 TPM 供应商使用。

Microsoft 发布的受信任的 TPM 根和中间证书集合为方便起见。
可以使用以下步骤来安装这些证书。
如果 TPM 证书不包含在以下程序包，请联系您的 TPM 供应商或 OEM 服务器以获取根和中间证书为特定的 TPM 模型。

在重复以下步骤**的每个 HGS 服务器**:

1.  从最新的包下载[ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

2.  展开 cab 文件。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  默认情况下，配置脚本将为每个 TPM 供应商安装证书。 如果只想要导入特定的 TPM 供应商的证书，删除你的组织不信任的 TPM 供应商的文件夹。

4.  通过在扩展的文件夹中运行安装脚本安装受信任的证书包。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

若要添加新的证书或早期版本的安装过程中有意跳过的只需重复上述步骤 HGS 群集中每个节点上。
现有证书将保持不受信任，但在展开的 cab 文件中找到的新证书将添加到受信任的 TPM 存储。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [配置结构 DNS](guarded-fabric-configuring-fabric-dns-tpm.md)




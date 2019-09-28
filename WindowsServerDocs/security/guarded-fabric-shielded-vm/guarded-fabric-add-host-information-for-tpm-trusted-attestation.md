---
title: 为受 TPM 信任的证明添加主机信息
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 35efca71278c288189819d6c9fc49ba8195d18a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386817"
---
>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>为受 TPM 信任的证明添加主机信息

对于 TPM 模式，构造管理员捕获三种类型的主机信息，其中每个信息需要添加到 HGS 配置中：

- 每个 Hyper-v 主机的 TPM 标识符（EKpub）
- 代码完整性策略，Hyper-v 主机允许的二进制文件的列表
- TPM 基线（启动度量值），表示在相同的硬件类上运行的一组 Hyper-v 主机

构造管理员捕获信息后，将其添加到 HGS 配置中，如以下过程中所述。

1. 获取包含 EKpub 信息的 XML 文件，并将其复制到 HGS 服务器。 每个主机将有一个 XML 文件。 然后，在 HGS 服务器上已提升权限的 Windows PowerShell 控制台中运行以下命令。 为每个 XML 文件重复此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果在添加有关不受信任的认可密钥证书（EKCert）的 TPM 标识符时遇到错误，请确保已将[受信任的 TPM 根证书添加](guarded-fabric-install-trusted-tpm-root-certificates.md)到了 HGS 节点。
    > 此外，某些 TPM 供应商不使用 EKCerts。
    > 可以通过在记事本（如记事本）中打开 XML 文件来检查是否缺少 EKCert，并检查错误消息，指出找不到任何 EKCert。
    > 如果是这种情况，并且你相信计算机上的 TPM 是可信的，则可以使用 `-Force` 标志覆盖此安全检查，并将主机标识符添加到 HGS。

2. 获取构造管理员为主机创建的代码完整性策略，采用二进制格式（@no__t 旁1/-0）。 将其复制到 HGS 服务器。 然后运行以下命令。

    对于 `<PolicyName>`，请指定 CI 策略的名称，该名称描述应用于的主机类型。 最佳做法是在您的计算机的品牌/型号以及在其上运行的任何特殊软件配置后将其命名为。<br>对于 `<Path>`，指定代码完整性策略的路径和文件名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```
    
    > [!NOTE]
    > 如果使用的是已签名的代码完整性策略，请使用 HGS 注册同一策略的无符号副本。
    > 代码完整性策略上的签名用于控制对策略的更新，但不会度量到主机 TPM，因此不能由 HGS 证明。

3. 获取构造管理员从引用主机捕获的 TCGlog 文件。 将该文件复制到 HGS 服务器。 然后运行以下命令。 通常情况下，将策略命名为它所表示的硬件的类（例如，"制造商型号版本"）。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

这将完成为 TPM 模式配置 HGS 群集的过程。 构造管理员可能需要在为主机完成配置之前，先从 HGS 提供两个 Url。 若要获取这些 Url，请在 HGS 服务器上运行[HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

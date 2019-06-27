---
title: 添加受信任的 TPM 证明的主机信息
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 99be11bfec02924f93d9f759676e1eea364daa18
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407630"
---
>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

### <a name="add-host-information-for-tpm-trusted-attestation"></a>添加受信任的 TPM 证明的主机信息

对于 TPM 模式，构造管理员捕获三种类型的主机信息，每个要添加到 HGS 配置哪些需求：

- 每个 HYPER-V 主机的 TPM 标识符 (EKpub)
- 代码完整性策略，允许列表的允许二进制文件的 HYPER-V 主机
- 相同的类的硬件上运行的 TPM 基线 （启动量化指标），表示一系列的 HYPER-V 主机

在构造之后管理员捕获的信息，将其添加到 HGS 配置，如下面的过程中所述。

1. 获取包含 EKpub 信息并将其复制到 HGS 服务器的 XML 文件。 将每个主机的一个 XML 文件。 然后，在提升的 Windows PowerShell 控制台 HGS 服务器上，运行以下命令。 每个 XML 文件重复该命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果添加不受信任的认可密钥证书 (EKCert) 有关的 TPM 标识符时遇到错误，请确保[受信任的 TPM 根证书已添加](guarded-fabric-install-trusted-tpm-root-certificates.md)到 HGS 节点。
    > 此外，某些 TPM 供应商不使用 EKCerts。
    > 您可以检查是否 EKCert 缺少通过如记事本编辑器中打开 XML 文件，检查错误消息指示没有 EKCert 未找到。
    > 如果这种情况，并且在你的计算机 TPM 是否可信，则可以使用信任`-Force`重写此安全检查，并将主机标识符添加到 HGS 的标志。

2. 获取的二进制格式中的主机，构造管理员创建的代码完整性策略 (\*.p7b)。 将其复制到一个 HGS 服务器。 然后运行以下命令。

    有关`<PolicyName>`，指定描述适用于主机的类型的 CI 策略的名称。 最佳做法是在你的计算机和在其上运行的任何特殊软件配置的制造商/型号后将其命名。<br>有关`<Path>`，指定的路径和文件名的代码完整性策略。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```
    
    > [!NOTE]
    > 如果您使用的已签名的代码完整性策略，向 HGS 注册相同的策略的未签名的副本。
    > 上代码完整性策略的签名用于控制更新策略，但不是到主机 TPM 测量，因此不能证明到由 HGS。

3. 获取引用主机中的构造管理员捕获 TCGlog 文件。 将文件复制到一个 HGS 服务器。 然后运行以下命令。 通常情况下，将 （例如，"制造商模型修订版本"） 的硬件，它表示类进行仿造命名策略。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

这将完成配置 TPM 模式下的 HGS 群集的过程。 构造管理员可能需要你从 HGS 提供两个 Url，然后可以为主机完成配置。 若要获取这些 Url，HGS 服务器上，运行[Get HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

---
title: 捕获所需的 HGS 的 TPM 模式信息
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: 24d81e3d2c31b3493563f3f3e2ab3f92afff2c06
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284129"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>授权受保护的主机使用基于 TPM 的认证

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

TPM 模式下使用 TPM 标识符 (也称为平台标识符或认可密钥\[EKpub\]) 开始确定特定主机是否被授权为"受保护"。 证明该模式使用安全启动和代码完整性度量值以确保给定的 HYPER-V 主机处于正常状态并正在运行受信任的代码。 为了使证明以了解什么是和不正常，必须捕获以下项目：

1.  TPM 标识符 (EKpub)

    -  此信息仅适用于每个 HYPER-V 主机

2.  TPM 基线 （启动量化指标）

    -  这是硬件的适用于相同的类运行的所有 HYPER-V 主机

3.  代码完整性策略 （允许二进制文件的允许列表）

    -  这是适用于共享通用硬件和软件的所有 HYPER-V 主机

我们建议您捕获的基线和 CI 策略从"引用主机"，它是代表你的数据中心内的 HYPER-V 硬件配置的每个唯一的类。 从 Windows Server 1709 版开始，示例 CI 策略是包括在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies。 

## <a name="versioned-attestation-policies"></a>版本控制证明策略

Windows Server 2019 中引入新方法进行证明，称为*v2 证明*，TPM 证书必须存在才能添加到 HGS EKPub。 使用 Windows Server 2016 中的 v1 证明方法允许你重写此安全检查通过指定-Force 标志添加 HgsAttestationTpmHost 或 TPM 证明的其他 cmdlet 用来捕获项目运行时。 从 Windows Server 2019 开始，默认情况下使用 v2 证明，需要指定-PolicyVersion v1 标志，当你运行添加 HgsAttestationTpmHost，如果你需要注册 TPM 而无需证书。 -Force 标志并不适用于 v2 证明。 

主机仅可证明，如果所有项目 (EKPub + TPM 基线 + CI 策略) 使用相同版本的证明。 首先，尝试 V2 证明，如果该操作失败，则使用 v1 证明。 这意味着如果你需要使用 v1 证明注册 TPM 标识符，您需要同时指定-PolicyVersion v1 标志来捕获 TPM 基线并创建 CI 策略时使用 v1 证明。 如果 TPM 基线和 CI 策略创建的使用 v2 证明，则更高版本需要添加受保护的主机，如果没有 TPM 的证书时，需要重新创建每个项目-PolicyVersion v1 标志。 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>捕获每个主机的 TPM 标识符 （平台标识符或 EKpub）

1.  在构造域中，确保每个主机上的 TPM 准备好用于-即，对 TPM 进行初始化并获取所有权。 您可以通过打开 TPM 管理控制台 (tpm.msc) 或通过运行检查 TPM 状态**Get Tpm**提升的 Windows PowerShell 窗口中。 如果你的 TPM 不处于**准备**状态中时，将需要对其进行初始化并设置其所有权。 这可以在 TPM 管理控制台或通过运行**初始化 Tpm**。

2.  每个受保护的主机，若要获取其 EKpub 提升的 Windows PowerShell 控制台中运行以下命令。 有关`<HostName>`，替换内容适合此主机标识唯一的主机名称-这可以是其主机名或使用 fabric 清单服务 （如果可用） 的名称。 为方便起见，使用主机的名称将输出文件的名称。

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  每个主机将成为受保护的主机，确保为每个 XML 文件指定唯一名称的重复上述步骤。

4.  向 HGS 管理员提供生成的 XML 文件。

5.  HGS 域中打开 HGS 服务器上提升的 Windows PowerShell 控制台并运行以下命令。 每个 XML 文件重复该命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果添加不受信任的认可密钥证书 (EKCert) 有关的 TPM 标识符时遇到错误，请确保[受信任的 TPM 根证书已添加](guarded-fabric-install-trusted-tpm-root-certificates.md)到 HGS 节点。
    > 此外，某些 TPM 供应商不使用 EKCerts。
    > 您可以检查是否 EKCert 缺少通过如记事本编辑器中打开 XML 文件，检查错误消息指示没有 EKCert 未找到。
    > 如果这种情况，并且在你的计算机 TPM 是否可信，则可以使用信任`-Force`参数添加到 HGS 主机标识符。 需要在 Windows Server 2019 也使用`-PolicyVersion v1`参数时使用`-Force`。 这将创建一个策略与 Windows Server 2016 行为一致，将要求你使用`-PolicyVersion v1`注册 CI 策略和在的 TPM 基线时。

## <a name="create-and-apply-a-code-integrity-policy"></a>创建并应用代码完整性策略

代码完整性策略可帮助确保允许您信任的主机上运行的可执行文件的运行。 阻止运行的恶意软件和其他可执行文件外部受信任的可执行文件。

每个受保护的主机必须具有代码完整性策略应用以便在 TPM 模式下运行受防护的 Vm。 指定通过将它们添加到 HGS 信任的确切代码完整性策略。 可以配置代码完整性策略强制实施该策略，阻止不符合策略，或只是审核的任何软件 （日志事件时执行的策略中未定义的软件）。 

从 Windows Server 1709 版开始，示例代码完整性策略还包括在 C:\Windows\schemas\CodeIntegrity\ExamplePolicies Windows。 两个策略是针对 Windows Server 建议：

- **AllowMicrosoft**:允许由 Microsoft 签名的所有文件。 为服务器应用程序，如 SQL 或 Exchange，建议使用此策略，或者如果由 Microsoft 发布的代理服务器进行监视。
- **DefaultWindows_Enforced**:允许在 Windows 中提供，并且不允许发布的 Microsoft Office 等其他应用程序的文件。 建议运行只有内置服务器角色和功能，例如 HYPER-V 的服务器使用此策略。 

建议您首先创建 CI 策略在审核 （日志记录） 模式下，若要查看是否该文件没有任何内容，然后强制实施该策略用于托管生产工作负荷。 

如果您使用[新建 CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet 以生成您自己的代码完整性策略，你将需要决定要使用的规则级别。 我们建议了主别的**发布服务器**退而求其次**哈希**，这样最进行数字签名的软件更新而无需更改的 CI 策略。
编写相同的发布者的新软件可以也安装在服务器上而无需更改的 CI 策略。
未进行数字签名的可执行文件将进行哈希处理--这些文件的更新将要求您创建新的 CI 策略。
有关可用的 CI 策略规则级别的详细信息，请参阅[部署代码完整性策略： 策略规则和文件规则](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules)和 cmdlet 的帮助。

1.  在引用主机上生成新的代码完整性策略。 下面的命令创建的策略**发布服务器**退而求其次级**哈希**。 然后，它将 XML 文件转换为 Windows 和 HGS 需采用并分别测量 CI 策略的二进制文件格式。

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >上面的命令在审核模式下创建的 CI 策略。 它不会阻止未经授权的二进制文件在主机上运行。 只应在生产环境中使用已实施的策略。

2.  保持在哪里，您可以轻松地找到该代码完整性策略文件 （XML 文件）。 您需要编辑此文件更高版本以强制实施的 CI 策略或在对系统所做的将来更新中更改的合并。

3.  将 CI 策略应用于引用主机：

    1.  运行以下命令以配置要使用你的 CI 策略的计算机。 您还可以部署具有的 CI 策略[组策略](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy)或[System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm)。

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  重新启动主机来应用策略。

3.  通过运行典型工作负荷来测试代码完整性策略。 这可能包括正在运行的 Vm，任何 fabric 管理代理、 备份代理，或故障排除工具在计算机上。 检查是否有任何代码完整性冲突并更新你的 CI 策略如有必要。

4.  通过对更新后的 CI 策略 XML 文件中运行以下命令更改为已强制实施模式的 CI 策略。

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  CI 将策略应用到所有主机 （使用相同的硬件和软件配置） 使用以下命令：

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >将 CI 策略应用到主机时，更新在这些计算机上的任何软件时要小心。 使用 CI 策略不符合任何内核模式驱动程序可能会阻止计算机启动。 

6.  提供的二进制文件 (在此示例中，HW1CodeIntegrity\_enforced.p7b) 向 HGS 管理员。

7.  HGS 域中将代码完整性策略复制到一个 HGS 服务器并运行以下命令。

    有关`<PolicyName>`，指定描述适用于主机的类型的 CI 策略的名称。 最佳做法是在你的计算机和在其上运行的任何特殊软件配置的制造商/型号后将其命名。<br>有关`<Path>`，指定的路径和文件名的代码完整性策略。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>捕获每个唯一的类的硬件的 TPM 基线

需要为每个唯一的类的数据中心结构中的硬件的 TPM 基线。 再次使用"引用主机"。 

1. 在引用主机上，确保安装 HYPER-V 角色和主机保护者 HYPER-V 支持功能。

    >[!WARNING]
    >主机保护者 HYPER-V 支持功能，可能与某些设备不兼容的代码完整性的基于虚拟化的保护。 我们强烈建议启用此功能前在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. 若要捕获的基线策略，请在提升的 Windows PowerShell 控制台中运行以下命令。
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >将需要使用 **-SkipValidation**启用标志，如果引用主机不具有安全引导，存在 IOMMU、 基于已启用安全性的虚拟化和运行或应用代码完整性策略。 这些验证旨在让你知道的最低要求的主机上运行受防护的 VM。 使用-SkipValidation 标志不会更改 cmdlet; 的输出它只是静默处理这些错误。

3.  向 HGS 管理员提供的 TPM 基线 （TCGlog 文件）。

4.  HGS 域中将 TCGlog 文件复制到一个 HGS 服务器并运行以下命令。 通常情况下，将 （例如，"制造商模型修订版本"） 的硬件，它表示类进行仿造命名策略。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

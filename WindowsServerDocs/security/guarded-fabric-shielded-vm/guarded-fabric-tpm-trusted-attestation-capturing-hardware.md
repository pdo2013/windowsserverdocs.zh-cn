---
title: 捕获 HGS 需要的 TPM 模式信息
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: ba67cb80a405cd1c6d368a52798e3dec4d9861cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403517"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>使用基于 TPM 的证明授权受保护的主机

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

TPM 模式使用 TPM 标识符（也称为 "平台标识符" 或 "认可密钥" \[EKpub @ no__t）开始确定某个特定主机是否授权为 "受保护"。 此证明模式使用安全启动和代码完整性度量，以确保给定的 Hyper-v 主机处于正常状态，并且仅运行受信任的代码。 为了证明证明哪些内容不正常，必须捕获以下项目：

1.  TPM 标识符（EKpub）

    -  此信息对于每个 Hyper-v 主机都是唯一的

2.  TPM 基线（启动度量）

    -  这适用于在同一硬件类上运行的所有 Hyper-v 主机

3.  代码完整性策略（允许的二进制文件允许列表）

    -  这适用于共享公用硬件和软件的所有 Hyper-v 主机

建议从 "引用主机" 捕获基线和 CI 策略，该 "引用主机" 代表数据中心内的每个独特的 Hyper-v 硬件配置类。 从 Windows Server 版本1709开始，C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 中包含了示例 CI 策略 

## <a name="versioned-attestation-policies"></a>版本控制证明策略

Windows Server 2019 引入了一种新的证明方法，称为*v2 证明*，其中必须存在 TPM 证书才能将 EKPub 添加到 HGS。 Windows Server 2016 中使用的 v1 证明方法允许您通过在运行 HgsAttestationTpmHost 或其他 TPM 证明 cmdlet 来捕获项目时指定-Force 标志来覆盖此安全检查。 从 Windows Server 2019 开始，默认情况下使用 v2 证明，如果你需要在不使用证书的情况下注册 TPM，则在运行 HgsAttestationTpmHost 时需要指定-PolicyVersion v1 标志。 -Force 标志不适用于 v2 证明。 

仅当所有项目（EKPub + TPM 基线 + CI 策略）使用同一版本的证明时，主机才能证明这一点。 首先尝试 V2 证明，如果失败，则使用 v1 证明。 这意味着，如果需要使用 v1 证明注册 TPM 标识符，则在捕获 TPM 基线并创建 CI 策略时，还需要指定-PolicyVersion v1 标志来使用 v1 证明。 如果 TPM 基线和 CI 策略是使用 v2 证明创建的，稍后需要添加不包含 TPM 证书的受保护主机，则需要使用-PolicyVersion v1 标志重新创建每个项目。 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>捕获每个主机的 TPM 标识符（平台标识符或 EKpub）

1.  在 fabric 域中，确保每个主机上的 TPM 可供使用，即 TPM 已初始化并获得所有权。 你可以通过打开 TPM 管理控制台（tpm）或在提升的 Windows PowerShell 窗口中运行**Get tpm**来检查 tpm 的状态。 如果你的 TPM 未处于 "**就绪**" 状态，你将需要对其进行初始化并设置其所有权。 此操作可在 TPM 管理控制台中或通过运行 "**初始化-TPM**" 完成。

2.  在每个受保护的主机上，在提升的 Windows PowerShell 控制台中运行以下命令以获取其 EKpub。 对于 `<HostName>`，请将唯一主机名替换为适合标识此主机的名称-这可以是其主机名，也可以是构造清单服务（如果可用）使用的名称。 为方便起见，请使用主机的名称命名输出文件。

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  对于将成为受保护主机的每个主机，请重复上述步骤，确保为每个 XML 文件指定一个唯一名称。

4.  向 HGS 管理员提供生成的 XML 文件。

5.  在 HGS 域中，打开已提升权限的 Windows PowerShell 控制台，并运行以下命令。 为每个 XML 文件重复此命令。

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > 如果在添加有关不受信任的认可密钥证书（EKCert）的 TPM 标识符时遇到错误，请确保已将[受信任的 TPM 根证书添加](guarded-fabric-install-trusted-tpm-root-certificates.md)到了 HGS 节点。
    > 此外，某些 TPM 供应商不使用 EKCerts。
    > 可以通过在记事本（如记事本）中打开 XML 文件来检查是否缺少 EKCert，并检查错误消息，指出找不到任何 EKCert。
    > 如果是这种情况，并且你相信计算机中的 TPM 是可信的，则可以使用 `-Force` 参数将主机标识符添加到 HGS。 在 Windows Server 2019 中，使用 `-Force` 时，还需要使用 @no__t 参数。 这会创建与 Windows Server 2016 行为一致的策略，并将要求你在注册 CI 策略和 TPM 基线时使用 `-PolicyVersion v1`。

## <a name="create-and-apply-a-code-integrity-policy"></a>创建并应用代码完整性策略

代码完整性策略可帮助确保只允许运行你信任的可在主机上运行的可执行文件。 受信任的可执行文件外部的恶意软件和其他可执行文件将被阻止运行。

每个受保护的主机必须应用代码完整性策略，才能在 TPM 模式下运行受防护的 Vm。 你可以通过将其添加到 HGS 来指定你信任的确切代码完整性策略。 可以配置代码完整性策略，以强制实施策略、阻止任何不符合策略的软件，或仅审核（在执行策略中未定义的软件时记录事件）。 

从 Windows Server 版本1709开始，C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 中的 Windows 提供了示例代码完整性策略 建议为 Windows Server 使用两个策略：

- **AllowMicrosoft**：允许由 Microsoft 签署的所有文件。 建议将此策略用于 SQL 或 Exchange 等服务器应用程序，或者如果服务器由 Microsoft 发布的代理进行监视，则建议使用此策略。
- **DefaultWindows_Enforced**：仅允许 Windows 中附带的文件，而不允许由 Microsoft 发布的其他应用程序，如 Office。 对于仅运行内置服务器角色和功能（例如 Hyper-v）的服务器，建议使用此策略。 

建议你首先在审核（日志）模式下创建 CI 策略，以查看它是否缺少任何内容，然后为主机生产工作负荷强制实施策略。 

如果使用[CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet 来生成自己的代码完整性策略，则需要确定要使用的规则级别。 建议使用回退到**哈希**的主**发布服务器**级别，这样就可以在不更改 CI 策略的情况下更新已进行数字签名的大多数软件。
同一发行者编写的新软件也可以安装在服务器上，而无需更改 CI 策略。
未进行数字签名的可执行文件将进行哈希处理-更新这些文件将需要你创建新的 CI 策略。
有关可用 CI 策略规则级别的详细信息，请参阅[部署代码完整性策略：策略规则和文件规则](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules)和 cmdlet 帮助。

1.  在引用主机上，生成新的代码完整性策略。 以下命令在**发布服务器**级别上创建策略，并回退到**哈希**。 然后，它会将 XML 文件转换为二进制文件格式，Windows 和 HGS 需要分别应用和度量 CI 策略。

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >上述命令仅在审核模式下创建 CI 策略。 它不会阻止未授权的二进制文件在主机上运行。 只应在生产中使用强制执行的策略。

2.  保留您的代码完整性策略文件（XML 文件），您可以在其中轻松找到该文件。 稍后你将需要编辑此文件，以强制执行 CI 策略或合并以后对系统进行的更新。

3.  将 CI 策略应用到引用主机：

    1.  运行以下命令，将计算机配置为使用 CI 策略。 你还可以将 CI 策略部署[组策略](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy)或[System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm)。

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  重新启动主机以应用该策略。

3.  通过运行典型的工作负荷来测试代码完整性策略。 这可能包括正在运行的 Vm、任何构造管理代理、备份代理或计算机上的故障排除工具。 检查是否存在任何代码完整性冲突，并在必要时更新 CI 策略。

4.  通过对更新的 CI 策略 XML 文件运行以下命令，将 CI 策略更改为强制模式。

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  使用以下命令将 CI 策略应用到所有主机（使用相同的硬件和软件配置）：

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >将 CI 策略应用到主机和更新这些计算机上的任何软件时，请小心。 任何不符合 CI 策略的内核模式驱动程序都可能会阻止计算机启动。 

6.  向 HGS 管理员提供二进制文件（在本示例中为 HW1CodeIntegrity @ no__t-0enforced）。

7.  在 HGS 域中，将代码完整性策略复制到 HGS 服务器，并运行以下命令。

    对于 `<PolicyName>`，请指定 CI 策略的名称，该名称描述应用于的主机类型。 最佳做法是在您的计算机的品牌/型号以及在其上运行的任何特殊软件配置后将其命名为。<br>对于 `<Path>`，指定代码完整性策略的路径和文件名。

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>捕获每个独特硬件类的 TPM 基线

你的数据中心结构中的每个独特硬件类都需要一个 TPM 基线。 再次使用 "引用主机"。 

1. 在引用主机上，请确保安装了 Hyper-v 角色和主机保护者 Hyper-v 支持功能。

    >[!WARNING]
    >主机保护者 Hyper-v 支持功能可为可能与某些设备不兼容的代码完整性启用基于虚拟化的保护。 在启用此功能之前，强烈建议在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. 若要捕获基线策略，请在提升的 Windows PowerShell 控制台中运行以下命令。
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >如果引用主机未启用安全启动、有一个 IOMMU、启用和运行基于虚拟化的安全或已应用代码完整性策略，你将需要使用 **-SkipValidation**标志。 这些验证旨在让你了解在主机上运行受防护的 VM 的最低要求。 使用-SkipValidation 标志不会更改 cmdlet 的输出;它仅静默处理错误。

3.  向 HGS 管理员提供 TPM 基线（TCGlog 文件）。

4.  在 HGS 域中，将 TCGlog 文件复制到 HGS 服务器，并运行以下命令。 通常情况下，将策略命名为它所表示的硬件的类（例如，"制造商型号版本"）。

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [确认证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

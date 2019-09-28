---
title: 主机保护者服务疑难解答
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: be817a2c06b13af254b80090b9a7488209d4df0a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403523"
---
# <a name="troubleshooting-the-host-guardian-service"></a>主机保护者服务疑难解答

> 适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍在受保护的构造中部署或操作主机保护者服务（HGS）服务器时遇到的常见问题的解决方法。
如果你不确定问题的性质，请首先尝试在 HGS 服务器和 Hyper-v 主机上运行[受保护的构造诊断](guarded-fabric-troubleshoot-diagnostics.md)，以缩小可能的原因。

## <a name="certificates"></a>证书

HGS 需要使用多个证书才能运行，包括管理员配置的加密和签名证书，以及由 HGS 本身管理的证明证书。
如果这些证书配置不正确，则 HGS 将无法处理来自 Hyper-v 主机的请求，该主机希望证明或解锁受防护的 Vm 的密钥保护程序。
以下部分介绍了与在 HGS 上配置的证书相关的常见问题。

### <a name="certificate-permissions"></a>证书权限

HGS 必须能够访问证书指纹添加到 HGS 的加密证书和签名证书的公钥和私钥。
具体而言，运行 HGS 服务的组托管服务帐户（gMSA）需要密钥的访问权限。
若要查找 HGS 使用的 gMSA，请在你的 HGS 服务器上的权限提升的 PowerShell 提示符下运行以下命令：

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

如何授予 gMSA 帐户对使用私钥的访问权限取决于存储密钥的位置：在计算机上作为本地证书文件、在硬件安全模块（HSM）上，或使用自定义的第三方密钥存储提供程序。

#### <a name="grant-access-to-software-backed-private-keys"></a>授予对软件支持的私钥的访问权限

如果你使用的是自签名证书或证书颁发机构颁发的证书，而该证书颁发机构**未**存储在硬件安全模块或自定义密钥存储提供程序中，则可以通过执行以下步骤更改私钥权限：

1. 打开本地证书管理器（certlm.msc）
2. 展开 "**个人 > 证书**" 并找到要更新的签名或加密证书。
3. 右键单击该证书，然后选择 "**所有任务" > "管理私钥**"。
4. 单击 "**添加**" 以向新用户授予对证书的私钥的访问权限。
5. 在对象选取器中，输入之前找到的 HGS 的 gMSA 帐户名称，然后单击 **"确定"** 。
6. 确保 gMSA 对证书具有**读取**访问权限。
7. 单击 **"确定"** 以关闭 "权限" 窗口。

如果在服务器核心上运行 HGS 或远程管理服务器，将不能使用本地证书管理器管理私钥。
相反，你将需要下载[受保护的构造工具 PowerShell 模块](https://www.powershellgallery.com/packages/GuardedFabricTools)，该模块将允许你在 PowerShell 中管理权限。

1. 在 Server Core 计算机上打开提升的 PowerShell 控制台，或使用具有对 HGS 的本地管理员权限的帐户的 PowerShell 远程处理。
2. 运行以下命令以安装受保护的构造工具 PowerShell 模块，并向 gMSA 帐户授予对私钥的访问权限。

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>授予对 HSM 或自定义提供程序支持的私钥的访问权限

如果证书的私钥由硬件安全模块（HSM）或自定义密钥存储提供程序（KSP）提供支持，则权限模型将取决于你的特定软件供应商。
为了获得最佳结果，请查阅供应商的文档或支持网站，以获取有关如何为特定设备/软件处理私钥权限的信息。

某些硬件安全模块不支持向特定的用户帐户授予对私钥的访问权限;相反，它们允许计算机帐户访问特定密钥集中的所有密钥。
对于此类设备，通常足以使计算机能够访问密钥，并且 HGS 将能够利用该连接。

**Hsm 的技巧**

下面是建议的配置选项，这些选项可帮助你成功地将支持 HSM 的密钥与基于 Microsoft 及其合作伙伴体验的 HGS 结合使用。
为方便起见，我们提供了这些提示，在阅读本文时并不保证它们是正确的，也不会被 HSM 制造商认可。
如果你有其他问题，请与 HSM 制造商联系，以获取有关特定设备的准确信息。

HSM 品牌/系列      | 建议
----------------------|-------------
Gemalto 身份 SafeNet       | 确保证书请求文件中的 "密钥用法" 属性设置为0xa0，允许使用证书进行签名和加密。 此外，还必须使用本地证书管理器工具授予 gMSA 帐户对私钥的*读取*访问权限（请参阅上述步骤）。
nCipher nShield        | 确保每个 HGS 节点都有权访问包含签名和加密密钥的安全体系。 不需要配置 gMSA 特定的权限。
Utimaco CryptoServers | 确保证书请求文件中的 "密钥用法" 属性设置为 "0x13"，以允许将证书用于加密、解密和签名。

### <a name="certificate-requests"></a>证书请求

如果使用证书颁发机构在公钥基础结构（PKI）环境中颁发证书，则需要确保证书请求中包含对这些密钥的 HGS 使用的最低要求。

**签名证书**

CSR 属性 | 必需的值
-------------|---------------
算法    | RSA
密钥大小     | 至少2048位
密钥用法    | 签名/签名/DigitalSignature

**加密证书**

CSR 属性 | 必需的值
-------------|---------------
算法    | RSA
密钥大小     | 至少2048位
密钥用法    | 加密/加密/DataEncipherment

**Active Directory 证书服务模板**

如果使用 Active Directory 证书服务（ADCS）证书模板来创建证书，则建议使用具有以下设置的模板：

ADCS 模板属性 | 必需的值
-----------------------|---------------
提供程序类别      | 密钥存储提供程序
算法名称         | RSA
最小密钥大小       | 2048
用途                | 签名和加密
密钥用法扩展    | 数字签名，密钥译码，数据加密（"允许对用户数据进行加密"）


### <a name="time-drift"></a>时间偏差

如果服务器的时间与受保护构造中的其他 HGS 节点或 Hyper-v 主机的偏移明显相同，则可能会遇到证明签名者证书有效性的问题。
在 HGS 上的后台创建并续订证明签名者证书，并用于签署由证明服务颁发给受保护主机的健康证书。

若要刷新证明签名者证书，请在提升的 PowerShell 提示符下运行以下命令。

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

或者，你可以通过打开**任务计划程序**（taskschd.msc），导航到**任务计划程序库 > Microsoft > Windows > HGSServer**并运行名为**的任务，手动运行计划任务AttestationSignerCertRenewalTask**。

## <a name="switching-attestation-modes"></a>切换证明模式

如果将 HGS 从 TPM 模式切换到 Active Directory 模式，或使用[HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet 将其切换为相反模式，则你的 hgs 群集中的每个节点可能需要长达10分钟的时间才能开始强制实施新的证明模式。
这是正常行为。
建议你在使用新的证明模式验证所有主机均已成功证明之前，不要删除任何允许主机使用以前的证明模式的策略。

**从 TPM 切换到 AD 模式时的已知问题**

如果在 TPM 模式下初始化了 HGS 群集，并且后来切换到了 Active Directory 模式，则会出现一个已知问题，它会阻止 HGS 群集中的其他节点切换到新的证明模式。
若要确保所有 HGS 服务器都强制实施正确的证明模式，请在 HGS 群集的**每个节点上**运行 `Set-HgsServer -TrustActiveDirectory`。
如果要从 TPM 模式切换到 AD 模式 *，并且*群集最初是在 ad 模式下设置的，则不会应用此问题。

可以通过运行[HgsServer](https://technet.microsoft.com/library/mt652162.aspx)来验证 HGS 服务器的证明模式。

## <a name="memory-dump-encryption-policies"></a>内存转储加密策略

如果你正在尝试配置内存转储加密策略，但未看到默认的 HGS 转储策略（Hgs @ no__t-0NoDumps、Hgs @ no__t-1DumpEncryption 和 Hgs @ no__t-2DumpEncryptionKey）或转储策略 cmdlet （HgsAttestationDumpPolicy），则它是可能没有安装最新的累积更新。
若要解决此问题，请将[你的 HGS 服务器更新](guarded-fabric-manage-hgs.md#patching-hgs)到最新的累积 Windows 更新并[激活新的证明策略](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation)。
激活新的证明策略之前，请确保将 Hyper-v 主机更新为相同的累积更新，因为在激活 HGS 策略后，未安装新的转储加密功能的主机可能会失败证明。

## <a name="endorsement-key-certificate-error-messages"></a>认可密钥证书错误消息

使用[HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet 注册主机时，会从提供的平台标识符文件中提取两个 TPM 标识符：认可密钥证书（EKcert）和公共认可密钥（EKpub）。
EKcert 标识 TPM 的制造商，从而保证 TPM 是真实的，并通过正常供应链制造。
EKpub 可以唯一地标识该特定 TPM，它是 HGS 用于授予主机运行受防护的 Vm 访问权限的一种措施。

如果满足以下两个条件之一，则在注册 TPM 主机时，会收到错误：
1. 平台标识符文件**不**包含认可密钥证书
2. 平台标识符文件包含认可密钥证书，但你的系统**不信任**该证书

某些 TPM 制造商未在其 Tpm 中包含 EKcerts。
如果怀疑您的 TPM 是这种情况，请向 OEM 确认 Tpm 不应具有 EKcert，并使用 `-Force` 标志手动向 HGS 注册该主机。
如果 TPM 应具有 EKcert，但在平台标识符文件中找不到，请确保在主机上运行[PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier)时使用的是管理员（提升） PowerShell 控制台。

如果收到 EKcert 不受信任的错误，请确保在每个 HGS 服务器上[安装了受信任的 tpm 根证书包](guarded-fabric-install-trusted-tpm-root-certificates.md)，并确保 TPM 供应商的根证书位于本地计算机的**TrustedTPM @ no__ 中。2RootCA**存储区。 还需要在本地计算机上的**TrustedTPM @ no__t-1IntermediateCA**存储中安装任何适用的中间证书。
安装根证书和中间证书后，应该能够成功运行 @no__t。

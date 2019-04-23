---
title: 故障排除主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 2dc9a612fa9760a6ca5f05efe1c287fd0872a1d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861248"
---
# <a name="troubleshooting-the-host-guardian-service"></a>故障排除主机保护者服务

> 适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍部署或运行在受保护的构造中的主机保护者服务 (HGS) 服务器时遇到的常见问题的解决方法。
如果您不确定的第一个尝试运行您的问题的性质[受保护的 fabric 诊断](guarded-fabric-troubleshoot-diagnostics.md)HGS 服务器和 HYPER-V 主机来缩小可能会导致。

## <a name="certificates"></a>证书

HGS 需要的多个证书才能正常运行，包括管理员已配置加密和签名证书，以及证明证书由 HGS 本身。
如果未正确配置这些证书，HGS 将无法处理请求所提供的 HYPER-V 主机希望证明或解锁受防护的 Vm 的密钥保护程序。
以下部分介绍与 HGS 上配置的证书相关的常见问题。

### <a name="certificate-permissions"></a>证书权限

HGS 必须能够访问的加密和签名证书由证书的指纹添加到 HGS 公共和私有密钥。
具体而言，组托管服务帐户 (gMSA) 运行 HGS 服务需要访问密钥。
若要查找由 HGS 使用 gMSA，请在 HGS 服务器上在提升的 PowerShell 提示符运行以下命令：

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

如何授予 gMSA 帐户访问权限以使用私有密钥取决于存储密钥的位置： 本地证书文件，在硬件安全模块 (HSM)，或使用自定义第三方密钥存储提供程序上的计算机上。

#### <a name="grant-access-to-software-backed-private-keys"></a>授予对由软件支持的专用密钥访问权限

如果使用自签名的证书或由证书颁发机构颁发的证书**不**存储在硬件安全模块或自定义密钥存储提供程序，您可以通过执行更改的私有密钥权限以下步骤：

1. 打开本地证书管理器 (certlm.msc)
2. 展开**个人 > 证书**并找到你想要更新的签名或加密证书。
3. 右键单击该证书，然后选择**的所有任务 > 管理私钥**。
4. 单击**添加**授予对证书的私钥的新用户访问。
5. 在对象选取器中，输入的 gMSA 帐户名称 HGS 找到更早版本，然后单击**确定**。
6. 请确保具有 gMSA**读取**证书的访问权限。
7. 单击**确定**以关闭权限窗口中。

如果您在 Server Core 上运行 HGS 或远程管理服务器，你将不能用于管理使用本地证书管理器的专用密钥。
相反，你将需要下载[受保护的 Fabric 工具 PowerShell 模块](https://www.powershellgallery.com/packages/GuardedFabricTools)这将允许你管理的权限在 PowerShell 中。

1. 打开提升的 PowerShell 控制台在服务器核心计算机上或使用 PowerShell 远程处理与 HGS 具有本地管理员权限的帐户。
2. 运行以下命令以安装的受保护的构造工具 PowerShell 模块，并授予对的私钥的 gMSA 帐户访问权限。

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

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>授予对 HSM 或自定义由提供程序支持的专用密钥访问权限

如果证书的私钥受硬件安全模块 (HSM) 或自定义密钥存储提供程序 (KSP)，权限模型将依赖于特定的软件供应商联系。
为获得最佳结果，请查阅供应商的文档或站点的支持如何私钥的权限进行处理的每个软件特定的设备上的信息。

某些硬件安全模块不支持授予特定用户帐户访问私钥;相反，它们在特定的密钥集允许计算机帐户访问所有密钥。
对于此类设备，它是通常足以使计算机能够访问你的密钥和 HGS 将能够利用该连接。

**Hsm 的提示**

下面是建议的配置选项来帮助你成功使用由 HSM 支持的密钥与 HGS 基于 Microsoft 和其合作伙伴的经验。
这些提示提供为方便起见，不保证一定要在读取时正确也它们认可的 HSM 制造商。
如果有进一步的问题与特定设备相关的准确信息，请与 HSM 制造商联系。

HSM 品牌/系列      | 建议
----------------------|-------------
Gemalto SafeNet       | 请确保证书请求文件中的密钥使用情况属性设置为将允许使用的签名和加密的证书。 此外，你必须将 gMSA 帐户授予*读取*访问私钥使用本地证书管理器工具 （请参阅上述步骤）。
Thales nShield        | 确保 HGS 的每个节点有权访问包含签名和加密密钥的安全体系。 不需要配置特定于 gMSA 的权限。
Utimaco CryptoServers | 请确保证书请求文件中的密钥使用情况属性设置为 0x13，允许使用的加密、 解密和签名的证书。

### <a name="certificate-requests"></a>证书请求

如果使用的证书颁发机构来颁发证书的公钥基础结构 (PKI) 环境中，您需要确保您的证书申请包含 HGS 的使用情况的这些密钥的最低要求。

**签名证书**

CSR 属性 | 所需的值
-------------|---------------
算法    | RSA
密钥大小     | 至少为 2048 位
密钥用法    | Signature/Sign/DigitalSignature

**加密证书**

CSR 属性 | 所需的值
-------------|---------------
算法    | RSA
密钥大小     | 至少为 2048 位
密钥用法    | Encryption/Encrypt/DataEncipherment

**Active Directory 证书服务模板**

如果正在使用 Active Directory 证书服务 (ADCS) 证书模板来创建的证书，则建议你使用模板具有以下设置：

ADCS 模板属性 | 所需的值
-----------------------|---------------
提供程序类别      | 密钥存储提供程序
算法名称         | RSA
最小密钥大小       | 2048
用途                | 签名和加密
密钥用法扩展    | 数字签名，密钥加密数据加密 （"允许加密的用户数据"）


### <a name="time-drift"></a>时间偏差

如果你的服务器的时间将显著与其他 HGS 节点或受保护的构造中的 HYPER-V 主机的有偏差，您可能会遇到问题证明签名者证书有效性。
创建和续订在后台上 HGS 证明签名者证书，用于运行状况证明服务颁发给受保护的主机的证书进行签名。

若要刷新证明签名者证书，请在提升的 PowerShell 提示符运行以下命令。

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

或者，您可以手动运行计划的任务通过打开**任务计划程序**(taskschd.msc)，导航到**任务计划程序库 > Microsoft > Windows > HGSServer**和运行任务名为**AttestationSignerCertRenewalTask**。

## <a name="switching-attestation-modes"></a>切换证明模式

如果 HGS 从 TPM 模式切换到 Active Directory 模式下或使用，反之亦然[集 HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet，它可能开始实施新的证明模式 HGS 群集中需要最多 10 分钟的每个节点。
这是正常行为。
建议不要删除任何之前验证所有主机都证明成功使用新的证明模式允许从以前的证明模式的主机的策略。

**从 TPM 切换到 AD 模式时的已知的问题**

如果初始化 TPM 模式和更高版本的交换机中为 Active Directory 模式 HGS 的群集，则一个已知的问题，可以防止 HGS 群集中的其他节点切换到新的证明模式。
若要确保所有 HGS 服务器要都强制使用正确的证明模式，请运行`Set-HgsServer -TrustActiveDirectory`**每个节点上**的 HGS 群集。
如果从 TPM 模式切换到 AD 模式下，此问题不适用于*和*群集最初设置 AD 模式中。

您可以通过运行验证 HGS 服务器的证明模式[Get HgsServer](https://technet.microsoft.com/library/mt652162.aspx)。

## <a name="memory-dump-encryption-policies"></a>内存转储加密策略

如果你尝试配置内存转储加密策略，并且看不到默认 HGS 转储策略 (Hgs\_NoDumps、 Hgs\_DumpEncryption 和 Hgs\_DumpEncryptionKey) 或转储策略 cmdlet （Add-HgsAttestationDumpPolicy)，很可能没有安装最新的累积更新。
若要解决此问题，[更新 HGS 服务器](guarded-fabric-manage-hgs.md#patching-hgs)到最新累积的 Windows 更新并[激活新的证明策略](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation)。
请确保为激活 HGS 策略后，没有安装新的转储加密功能的主机可能会失败证明更新到相同的累积更新前激活新的证明策略，你的 HYPER-V 主机。

## <a name="endorsement-key-certificate-error-messages"></a>认可密钥证书错误消息

注册使用的主机时[添加 HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet，将从提供的平台标识符文件中提取两个 TPM 标识符： 认可密钥证书 (EKcert) 和公共认可密钥 (EKpub)。
EKcert 标识的制造商的 TPM，提供 TPM 是可信和制造商通过正常的供应链的保证。
EKpub 唯一标识特定的 TPM，并为其中一个 HGS 用来授予访问权限才能运行受防护的 Vm 主机的度量值。

注册 TPM 主机，如果两个条件之一为真时，您将收到错误：
1. 平台标识符文件**却不**包含认可密钥证书
2. 平台标识符文件包含的认可密钥证书，但该证书**不受信任**在系统上

某些 TPM 制造商不包括在其 Tpm EKcerts。
如果您怀疑这是与你的 TPM 的情况，确认为 oem 在 Tpm 不应具有 EKcert，使用`-Force`标志，用于手动向 HGS 注册主机。
如果你的 TPM 应具有 EKcert，但一个找不到平台标识符文件中，确保使用管理员身份 （提升） PowerShell 控制台的运行时[Get PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier)在主机上。

如果你收到了错误在 EKcert 不受信任，确保您有[安装受信任的 TPM 根证书包](guarded-fabric-install-trusted-tpm-root-certificates.md)上每个 HGS 服务器，并且你的 TPM 供应商的根证书位于本地计算机的**TrustedTPM\_RootCA**存储。 任何适用的中间证书还需要安装在**TrustedTPM\_IntermediateCA**将存储在本地计算机上。
安装根和中间证书后，您应能够运行`Add-HgsAttestationTpmHost`成功。

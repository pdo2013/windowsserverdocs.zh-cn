---
title: 设备运行状况证明
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: 7c2d7113847cc44f18c5234502b58becde1dcb9f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446511"
---
# <a name="device-health-attestation"></a>设备运行状况证明

>适用于：Windows Server 2016

设备运行状况证明 (DHA) 在 Windows 10 版本 1507 中引入，它包括以下方面：

-   根据[开放移动联盟 (OMA) 标准](http://openmobilealliance.org/)与 Windows 10 移动设备管理 (MDM) 框架集成。

-   支持装有设置为固件或离散格式的受信任模块平台 (TPM) 的设备。

-   使企业能够将组织的安全栏提升到受监视和已证明安全的硬件，对操作成本只有极小影响或无影响。

从 Windows Server 2016 开始，现在可以运行 DHA 服务作为组织内的服务器角色。 使用本主题以了解如何安装和配置设备运行状况证明服务器角色。

## <a name="overview"></a>概述

可以使用 DHA 来评估设备运行状况：
  
-   支持 TPM 1.2 或 2.0 的 Windows 10 和 Windows 10 移动设备。  
-   通过使用可访问 Internet 的 Active Directory 来管理的本地设备、通过使用无法访问 Internet 的 Active Directory 来管理的设备、通过 Azure Active Directory 管理的设备，或同时使用 Active Directory 和 Azure Active Directory 的混合部署。


### <a name="dha-service"></a>DHA 服务

DHA 服务为设备验证 TPM 和 PCR 日志，然后发布 DHA 报表。 Microsoft 以下面三种方式提供 DHA 服务：

- **DHA 云服务** Microsoft 管理的 DHA 是免费的地域负载平衡服务，并对来自世界不同地区的访问进行了优化。

- **DHA 本地服务**在 Windows Server 2016 中引入的新的服务器角色。 具有 Windows Server 2016 许可证的客户可免费使用。

- **DHA Azure 云服务** Microsoft Azure 中的虚拟主机。 若要执行此操作，需要为 DHA 本地服务提供虚拟主机和许可证。

DHA 服务与 MDM 解决方案相集成，并提供以下功能： 

-   将其从设备收到的信息（通过现有的设备管理通信渠道）与 DHA 报表相组合
-   基于已证明的硬件和受保护的数据，作出更安全且更受信任的安全决策

以下示例演示了如何使用 DHA 来帮助提升组织资产的安全保护栏。

1. 创建一个策略来检查以下引导配置/特性：
   - 安全启动
   - BitLocker
   - ELAM
2. MDM 解决方案强制实施此策略，并触发基于 DHA 报表数据的纠正操作。  例如，它可以验证以下方面：
   - 安全启动已启用，设备加载了可靠的受信任代码，且 Windows 启动加载器未被篡改。
   - 受信任的引导已成功验证 Windows 内核以及设备启动时加载的组件的数字签名。
   - 标准引导创建了可远程验证的受 TPM 保护的审核线索。
   - BitLocker 已启用，当设备关闭时它可以保护数据。
   - ELAM 在早期启动阶段已启用，并且正在监视运行时。
  
#### <a name="dha-cloud-service"></a>DHA 云服务

DHA 云服务提供以下功能：

-   查看从注册了 MDM 解决方案的设备接收的 TCG 和PCR 设备引导日志。 
-   创建一个能够抵御篡改和防篡改的报表（DHA 报表），用于描述设备基于由设备 TPM 芯片收集和保护的数据的启动方式。 
-   在受保护的通信通道中，将 DHA 报表传递到请求报表的 MDM 服务器。

#### <a name="dha-on-premises-service"></a>DHA 本地服务

DHA 本地服务提供由 DHA 云服务提供的所有功能。  它还使客户能够：

 - 通过运行自己数据中心的 DHA 服务来优化性能
 - 确保 DHA 报表未离开网络

#### <a name="dha-azure-cloud-service"></a>DHA Azure 云服务

此服务提供与 DHA 本地服务相同的功能，除非 DHA Azure 云服务在 Microsoft Azure 中作为虚拟主机运行。

### <a name="dha-validation-modes"></a>DHA 验证模式

可以设置 DHA 本地服务以在 EKCert 或 AIKCert 验证模式下运行。 当 DHA 服务发布一个报表时，它会指示是在 AIKCert 还是 EKCert 验证模式中发布。 AIKCert 和 EKCert 验证模式提供相同的安全保证，前提是 EKCert 信任链保持最新。

#### <a name="ekcert-validation-mode"></a>EKCert 验证模式

在未连接到 Internet 的组织中对设备进行 EKCert 验证模式优化。 连接到在 EKCert 验证模式下运行的 DHA 服务的设备**不**能直接访问 Internet。

当 DHA 在 EKCert 验证模式下运行时，它依赖于企业管理的需要不定期更新（每年大约 5-10 次）的信任链。 

Microsoft 在 .cab 存档可公开访问的存档中为已批准的 TPM 制造商发布受信任的根和中间 CA 聚合包（如果可用）。 需要下载源，验证其完整性，并将其安装在运行设备运行状况证明的服务器上。

示例存档[ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

#### <a name="aikcert-validation-mode"></a>AIKCert 验证模式

AIKCert 验证模式为有权访问 Internet 的运行环境进行了优化。 连接到在 AIKCert 验证模式下运行的 DHA 服务的设备必须具有直接访问 Internet 的权限，并且可以从 Microsoft 获得 AIK 证书。 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>在 Windows Server 2016 上安装和配置 DHA 服务

使用下列部分，在 Windows Server 2016 上安装和配置 DHA。

### <a name="prerequisites"></a>系统必备

为了设置和验证 DHA 本地服务，需要：

- 运行 Windows Server 2016 的服务器。
- 带有在清除/就绪状态运行最新 Windows Insider 内部版本的 TPM（1.2 或 2.0）的一个（或多个） Windows 10 客户端设备。
- 确定是要在 EKCert 还是在 AIKCert 验证模式下运行。
- 以下证书：
  - **DHA SSL 证书**链接到企业信任的根的 x.509 SSL 证书，具有可导出的私钥。 此证书保护传输中的 DHA 数据通信，包括服务器到服务器（DHA 服务和 MDM 服务器）和服务器到客户端（DHA 服务和 Windows 10 设备）的通信。
  - **DHA 签名证书**链接到企业信任的根的 x.509 证书，具有可导出的私钥。 DHA 服务使用此证书进行数字签名。 
  - **DHA 加密证书**链接到企业信任的根的 x.509 证书，具有可导出的私钥。 DHA 服务还使用此证书进行加密。 


### <a name="install-windows-server-2016"></a>安装 Windows Server 2016

使用首选的安装方法来安装 Windows Server 2016，如 Windows 部署服务，或从可启动的媒体、USB 驱动器或本地文件系统中运行安装程序。 如果是首次配置 DHA 本地服务，应使用**桌面体验**安装选项安装 Windows Server 2016。

### <a name="add-the-device-health-attestation-server-role"></a>添加设备运行状况证明服务器角色

可以使用服务器管理器安装设备运行状况证明服务器角色及其依赖项。 

在安装 Windows Server 2016 后，设备会重新启动，并打开服务器管理器。 如果服务器管理器未自动启动，单击“**开始**”，然后单击“**服务器管理器**”。

1.  单击“**添加角色和功能**”。
2.  在“开始之前”  页上，单击“下一步”  。
3.  在 **“选择安装类型”** 页面上，单击 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。
4.  在**选择目标服务器**页上，单击“**从服务器池中选择服务器**”，选择服务器，然后单击“**下一步**”。
5.  在**选择服务器角色**页上，选择“**设备运行状况证明**”复选框。
6.  单击“**添加功能**”来安装其他所需的角色服务和功能。
7.  单击“下一步”  。
8.  在“选择功能”  页上，单击“下一步”  。
9.  在“Web 服务器角色 (IIS)”  页面上，单击“下一步”  。
10. 在**选择角色服务**页上，单击“**下一步**”。
11. 在**设备运行状况证明服务**页上，单击“**下一步**”。
12. 在 **“确认安装选择”** 页上，单击 **“安装”** 。
13. 安装完成后，单击“**关闭**”。

### <a name="install-the-signing-and-encryption-certificates"></a>安装签名和加密证书

使用以下 Windows PowerShell 脚本来安装签名和加密证书。 指纹的详细信息，请参阅[如何：检索证书的指纹](https://msdn.microsoft.com/library/ms734695.aspx)。

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>安装受信任的 TPM 根证书包

若要安装受信任的 TPM 根证书包，必须将其提取，删除任何组织不信任的信任链，然后运行 setup.cmd。

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>下载受信任的 TPM 根证书包

安装证书包之前，你可以下载从受信任的 TPM 根的最新列表[ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

> **重要说明：** 安装包之前, 验证由 Microsoft 进行数字签名。

#### <a name="extract-the-trusted-certificate-package"></a>提取受信任的证书包
通过运行以下命令提取受信任的证书包。
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>为 TPM 供应商删除组织*不*信任的信任链（可选）

为任何 TPM 供应商信任链删除组织不信任的文件夹。

> **注意：** 如果使用 AIK 证书模式，需要 Microsoft 文件夹来验证 Microsoft 发布的 AIK 证书。

#### <a name="install-the-trusted-certificate-package"></a>安装受信任的证书包
通过从 .cab 文件运行安装程序脚本来安装受信任的证书包。

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>配置设备运行状况证明服务

可以使用 Windows PowerShell 来配置 DHA 本地服务。

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>配置证书链策略

通过运行以下 Windows PowerShell 脚本来配置证书链策略。

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>DHA 管理命令

以下是一些可以帮助管理 DHA 服务的 Windows PowerShell 示例。

### <a name="configure-the-dha-service-for-the-first-time"></a>首次配置 DHA 服务

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>删除 DHA 服务配置

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>获取活动签名证书

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>设置活动签名证书

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **注意：** 此证书必须部署在中运行 DHA 服务的服务器上**LocalMachine\My**证书存储区。 当设置活动签名证书时，现有的活动签名证书会移动到非活动的签名证书列表。

### <a name="list-the-inactive-signing-certificates"></a>列出非活动签名证书
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>删除所有非活动签名证书
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **注意：** 仅*一个*随时可能在服务中存在非活动状态的证书 （任何类型）。 一旦不再需要，证书应从非活动证书列表中删除。

### <a name="get-the-active-encryption-certificate"></a>获取活动加密证书

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>设置活动加密证书

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

此证书必须在 **LocalMachine\My** 证书存储中的设备上部署。 

当设置活动加密证书时，现有的活动加密证书会移动到非活动的加密证书列表。

### <a name="list-the-inactive-encryption-certificates"></a>列出非活动加密证书

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>删除任何非活动加密证书

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>获取 X509ChainPolicy 配置 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>更改 X509ChainPolicy 配置

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>DHA 服务报表

以下是由 DHA 服务向 MDM 解决方案报告的消息列表： 

- **200** HTTP 正常。 证书已返回。
- **400** 请求无效。 请求格式无效，运行状况证书无效，证书签名不匹配，运行状况证明 Blob 无效，或运行状况状态 Blob 无效。 响应还包含一条消息，如响应架构所述，有可用于诊断的错误代码和错误消息。
- **500** 内部服务器错误。 如果出现阻止该服务颁发证书的问题，它也可能发生。
- **503** 限制拒绝请求以防止服务器过载。 

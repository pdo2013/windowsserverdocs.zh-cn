---
title: "设备健康审计"
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
ms.openlocfilehash: d304ee3456f8db1e5b202c1d9221d1374a5251be
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="device-health-attestation"></a>设备健康审计

>适用于：Windows Server 2016

引入 Windows 10 版本 1507，包括以下设备健康审计 (DHA):

-   与 Windows 10 移动版设备管理 (MDM) 框架与对齐集成[打开移动版联盟 (OMA) 标准](http://openmobilealliance.org/)。

-   支持设备的具有不同的格式或固件中预配受信任模块平台 (TPM)。

-   启用企业提升到硬件其组织的安全栏监视和 attested 安全，用降至最低或不会影响操作成本。

从 Windows Server 2016 开始，你现在可以运行 DHA 服务服务器角色为在你的组织。 使用本主题以了解如何安装并配置设备健康审计服务器角色。

## <a name="overview"></a>概述

你可以使用 DHA 评估设备的运行状况：
  
-   Windows 10 和 Windows 10 移动版支持 TPM 1.2 或 2.0 的设备。  
-   由通过 Internet 访问权限，由由 Azure Active Directory 或使用 Active Directory 和 Azure Active Directory 混合部署的设备没有 Internet 访问权限，使用 Active Directory 的设备使用 Active Directory 的本地设备。


### <a name="dha-service"></a>DHA 服务

DHA 服务验证设备 TPM 和 PCR 日志，然后问题 DHA 报告。 Microsoft 将提供 DHA 服务采用三种方法：

- **DHA 云服务**Microsoft 托管的 DHA 服务，免费的、地理-负载平衡，且优化来自不同地区的世界上的访问权限。

- **DHA 本地服务**Windows Server 2016 中引入了新服务器作用。 它可以免费拥有 Windows Server 2016 许可证的客户。

- **DHA Azure 云服务**中 Microsoft Azure 虚拟主机。 若要执行此操作，你需要虚拟主机和许可证 DHA 本地服务。

DHA 服务 MDM 解决方案与集成，提供了以下功能： 

-   结合使用 DHA 报告的设备（通过现有设备管理联络渠道）从这里收到的信息
-   使更安全、受信任安全决策，根据硬件 attested 和保护数据

下面介绍如何使用 DHA 来帮助提高你的组织资产安全保护栏显示的示例。

1. 创建检查以下启动配置/特性策略：
  - 安全启动
  - BitLocker
  - ELAM
2. MDM 解决方案强制执行此策略和触发基于 DHA 报告数据纠正操作。  例如，无法验证如下：
  - 启用安全启动设备加载受信任的代码正版，Windows 启动 loader 未被篡改。
  - 受信任的启动成功验证 Windows 内核以及的组件的设备已启动时加载的数字签名。
  - 标准的引导创建无法远程验证 TPM 保护审核古道。
  - 已启用 BitLocker，并使其保护数据，该设备已打开时关闭。
  - ELAM 初期启动在已启用，并且监控运行时。
  
#### <a name="dha-cloud-service"></a>DHA 云服务

DHA 云服务提供以下优势：

-   检查收到来自 MDM 解决方案已注册设备的 TCG 和 PCR 设备启动日志。 
-   创建一个篡改溅和篡改明显报告（DHA 报告）中介绍了如何设备已启动根据的数据收集并受设备的 TPM 芯片。 
-   请求中受保护的信道报告 MDM 服务器提供 DHA 报告。

#### <a name="dha-on-premises-service"></a>DHA 本地服务

DHA 本地服务提供 DHA 云服务提供的所有功能。  它还会启用到客户：

 - 通过运行 DHA 服务的数据中心中优化性能
 - 确保 DHA 报告未将你的网络

#### <a name="dha-azure-cloud-service"></a>DHA Azure 云服务

只是在 Microsoft Azure 虚拟主机作为运行 DHA Azure 云服务，该服务提供 DHA 本地服务，相同的功能。

### <a name="dha-validation-modes"></a>DHA 验证模式

你可以设置 DHA 本地服务以 EKCert 或 AIKCert 验证模式运行。 当 DHA 服务问题的报告时，表明它已颁发型 AIKCert 或 EKCert 验证。 只要信任 EKCert 链保持最新，AIKCert 和 EKCert 验证模式提供相同的安全保障。

#### <a name="ekcert-validation-mode"></a>EKCert 验证模式

EKCert 验证模式非常适合在组织中没有连接到 Internet 的设备。 设备连接到运行 EKCert 验证型 DHA 服务执行**不**直接访问 Internet。

在 EKCert 验证模式下运行的 DHA，它依赖需要有时更新 (大约 5-10 倍每年) 的信任企业管理链。 

Microsoft 发布的受信任的根和中间 CA 的批准 TPM 制造商的聚合的程序包（如它们可用）在公开访问存档.cab 存档中。 你需要下载源、验证其完整性，并将其安装在运行设备健康审计服务器上。

示例存档正在[https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

#### <a name="aikcert-validation-mode"></a>AIKCert 验证模式

AIKCert 验证模式非常适合有权访问 Internet 的操作环境。 设备连接到运行 AIKCert 验证型 DHA 服务必须具有直接访问 Internet，并且能够从 Microsoft 获取 AIK 证书。 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>安装和配置 Windows Server 2016 上 DHA 服务

使用以下各部分以获取 DHA 安装和配置 Windows Server 2016 上。

### <a name="prerequisites"></a>先决条件

设置并验证 DHA 本地服务，以便你需要：

- 运行 Windows Server 2016 的服务器。
- 带有 TPM（1.2 或 2.0），在运行最新的 Windows 预览体验成员清除/ready 状态的 Windows 10 客户端设备一个（或多个）版本。
- 如果你要在 EKCert 或 AIKCert 验证模式下运行的决定。
- 下面的证书：
  - **DHA SSL 证书**具有可以导出的专用键链接到企业版受信任的根 x.509 SSL 证书。 此证书保护 DHA 数据通信中公交包括服务器（DHA 服务和 MDM 服务器）和客户端（DHA 服务和 Windows 10 设备）的通信的服务器。
  - **DHA 签名证书**具有可以导出的专用键链接到企业版受信任的根 x.509 证书。 DHA 服务进行数字签名使用该证书。 
  - **DHA 加密证书**具有可以导出的专用键链接到企业版受信任的根 x.509 证书。 DHA 服务还会使用该证书加密。 


### <a name="install-windows-server-2016"></a>安装 Windows Server 2016

安装 Windows Server 2016 使用您的首选的安装方法，如 Windows 部署服务，或运行安装程序的可启动媒体、U 盘或本地文件系统。 如果你正在配置 DHA 本地服务第一次，你应安装 Windows Server 2016 使用**桌面体验**安装选项。

### <a name="add-the-device-health-attestation-server-role"></a>添加设备健康审计服务器角色

你可以使用服务器管理器中安装设备健康审计服务器角色及其依赖项。 

你已安装了 Windows Server 2016 后，设备会重新启动，并打开服务器管理器。 如果未自动开始服务器管理器，请单击**开始**，然后单击**服务器管理器**。

1.  单击**添加角色和功能**。
2.  在**在开始之前**页上，单击**下一步**。
3.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。
4.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，选择服务器，然后单击**下一步**。
5.  在**选择服务器角色**页上，选择**设备健康审计**复选框。
6.  单击**添加功能**其他安装要求角色服务和功能。
7.  单击**下一步**。
8.  在**选择功能**页上，单击**下一步**。
9.  在**Web 服务器角色 (IIS)**页上，单击**下一步**。
10. 在**选择角色服务**页上，单击**下一步**。
11. 在**设备健康审计服务**页上，单击**下一步**。
12. 在**确认安装选择**页上，单击**安装**。
13. 安装完成后，单击**关闭**。

### <a name="install-the-signing-and-encryption-certificates"></a>安装签名和加密证书

使用下面的 Windows PowerShell 脚本，安装签名和加密证书。 有关指纹的详细信息，请参阅[操作方法：检索证书的指纹](https://msdn.microsoft.com/library/ms734695.aspx)。

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>安装受信任的 TPM 根证书包

若要安装的受信任的 TPM 根证书包，你必须解压缩它、删除任何不受你的组织的受信任的链并运行 setup.cmd。

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>受信任的 TPM 根证书程序包下载

安装证书包之前，你可以下载从受信任 TPM 根的最新列表[https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab)。

> **重要提示：**安装程序包之前, 验证由 Microsoft 进行数字签名。

#### <a name="extract-the-trusted-certificate-package"></a>解压缩受信任的证书包
通过运行以下命令解压缩受信任的证书包。
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>对于 TPM 供应商删除信任链*不*受信任的由你的组织（可选）

删除任何不受你的组织的 TPM 供应商信任链的文件夹。

> **注意：**使用 AIK 证书模式时，如果 Microsoft 文件夹需要验证 Microsoft 发出 AIK 证书。

#### <a name="install-the-trusted-certificate-package"></a>安装受信任的证书包
通过从的.cab 文件运行安装脚本，安装受信任的证书包。

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>配置设备健康审计服务

你可以使用 Windows PowerShell 配置 DHA 本地服务。

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>配置的证书链策略

通过运行以下 Windows PowerShell 脚本配置的证书链的策略。

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>DHA 管理命令

下面是一些可帮助你管理 DHA 服务的 Windows PowerShell 示例。

### <a name="configure-the-dha-service-for-the-first-time"></a>配置 DHA 服务第一次

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>删除 DHA 服务配置

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>获取活动的签名证书

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>设置有效的签名证书

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **注意：**必须运行 DHA 服务的服务器上部署该证书**LocalMachine\My**证书官方商城。 设置有效的签名证书后，现有的活动签名证书移到列表中停用的签名证书。

### <a name="list-the-inactive-signing-certificates"></a>列表中停用的签名证书
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>删除任何处于非活动状态的签名证书
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **注意：**仅*一个*处于非活动状态（的任何类型）的证书可能存在服务中，可随时。 证书后不再需要应从列表中停用的证书。

### <a name="get-the-active-encryption-certificate"></a>获取活动加密证书

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>活动加密证书的设置

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

在该设备上，必须部署证书**LocalMachine\My**证书官方商城。 

当设置活动加密证书时，现有的活动加密证书移到列表中停用加密证书。

### <a name="list-the-inactive-encryption-certificates"></a>列表中停用加密证书

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>删除任何处于非活动状态加密证书

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

## <a name="dha-service-reporting"></a>DHA 服务报告

以下是消息到 MDM 解决方案报告的 DHA 服务的列表： 

- **200** HTTP 确定。 返回证书。
- **400**错误请求。 无效的请求格式、无效健康证书签名证书不匹配，无效健康审计斑点或无效的运行状况状态斑点。 响应还包含一条消息，述响应方案，错误代码和用于诊断一条错误消息。
- **500**内部服务器错误。 防止服务发出证书的问题，如果发生这种情况。
- **503**拒绝请求以防止服务器重载限制。 

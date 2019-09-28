---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: 在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书
description: 在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 230fdaac28f4766c33e62362ca4c7e4d20f22c8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357739"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书



本文介绍如何将新的 SSL 证书部署到 AD FS 和 WAP 服务器。

>[!NOTE]
>替换 AD FS 场的 SSL 证书的建议方法是使用 Azure AD Connect。  有关详细信息，请参阅[更新 Active Directory 联合身份验证服务（AD FS）场的 SSL 证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>获取 SSL 证书
对于生产 AD FS 场，建议使用公开信任的 SSL 证书。 通常通过向第三方公用证书提供程序提交证书签名请求（CSR）来获取。 有多种方法可以生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有此相关文档。

- 请确保证书符合[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少个证书
建议你在所有 AD FS 和 Web 应用程序代理服务器上使用公用 SSL 证书。 有关详细要求，请参阅文档[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 证书要求
对于包括命名、信任根和扩展的要求，请参阅文档[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>替换 AD FS 的 SSL 证书
> [!NOTE]
> AD FS SSL 证书与 AD FS 管理 "管理单元中找到的 AD FS 服务通信证书不同。 若要更改 AD FS SSL 证书，你将需要使用 PowerShell。

首先，确定 AD FS 服务器正在运行的证书绑定模式：默认证书身份验证绑定或备用客户端 TLS 绑定模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>为默认证书身份验证绑定模式下运行 AD FS 替换 SSL 证书
默认情况下，AD FS 在端口443上执行设备证书身份验证，在端口49443上执行用户证书身份验证（或不是443的可配置端口）。
在此模式下，请使用 powershell cmdlet Set-adfssslcertificate 来管理 SSL 证书。

按以下步骤操作：

1. 首先，需要获取新证书。 通常通过将证书签名请求（CSR）提交给第三方公用证书提供程序来完成此操作。 有多种方法可以生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有此相关文档。

    * 请确保证书符合[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 获取证书提供程序的响应后，将其导入到每个 AD FS 和 Web 应用程序代理服务器上的本地计算机存储中。

1. 在**主**AD FS 服务器上，使用以下 cmdlet 安装新的 SSL 证书

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

可以通过执行以下命令来找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他说明

* Set-adfssslcertificate cmdlet 是多节点 cmdlet;这意味着只需要从主节点运行，并且将更新场中的所有节点。 这是服务器2016中的新增项。 在服务器 2012 R2 上，必须在每个服务器上运行 Set-adfssslcertificate。
* Set-adfssslcertificate cmdlet 只能在主服务器上运行。 主服务器必须运行服务器2016，并且场行为级别应提升为2016。
* Set-adfssslcertificate cmdlet 将使用 PowerShell 远程处理来配置其他 AD FS 服务器，请确保在其他节点上打开端口5985（TCP）。
* Set-adfssslcertificate cmdlet 将向 adfssrv 主体授予对 SSL 证书私钥的读取权限。 此主体表示 AD FS 服务。 无需向 AD FS 服务帐户授予对 SSL 证书私钥的读取访问权限。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>为在备用 TLS 绑定模式下运行的 AD FS 替换 SSL 证书
当在备用客户端 TLS 绑定模式下配置时，AD FS 会在不同的主机名上，对端口443和端口443上的用户证书身份验证执行设备证书身份验证。 用户证书主机名 AD FS 是以 "certauth" 预先挂起的主机名，例如 "certauth.fs.contoso.com"。
在此模式下，请使用 powershell cmdlet AdfsAlternateTlsClientBinding 来管理 SSL 证书。 这不仅会管理替代的客户端 TLS 绑定，还会管理 AD FS 设置 SSL 证书的所有其他绑定。

按以下步骤操作：

1. 首先，需要获取新证书。 通常通过将证书签名请求（CSR）提交给第三方公用证书提供程序来完成此操作。 有多种方法可以生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有此相关文档。

    * 请确保证书符合[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 获取证书提供程序的响应后，将其导入到每个 AD FS 和 Web 应用程序代理服务器上的本地计算机存储中。

1. 在**主**AD FS 服务器上，使用以下 cmdlet 安装新的 SSL 证书

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

可以通过执行以下命令来找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他说明

* AdfsAlternateTlsClientBinding cmdlet 是多节点 cmdlet;这意味着只需要从主节点运行，并且将更新场中的所有节点。
* AdfsAlternateTlsClientBinding cmdlet 只能在主服务器上运行。 主服务器必须运行服务器2016，并且场行为级别应提升为2016。
* AdfsAlternateTlsClientBinding cmdlet 将使用 PowerShell 远程处理来配置其他 AD FS 服务器，请确保在其他节点上打开端口5985（TCP）。
* AdfsAlternateTlsClientBinding cmdlet 将向 adfssrv 主体授予对 SSL 证书私钥的读取权限。 此主体表示 AD FS 服务。 无需向 AD FS 服务帐户授予对 SSL 证书私钥的读取访问权限。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>替换 Web 应用程序代理的 SSL 证书
若要在 WAP 上配置默认证书身份验证绑定或备用客户端 TLS 绑定模式，可以使用 WebApplicationProxySslCertificate cmdlet。
若要替换 Web 应用程序代理 SSL 证书，请在**每个**Web 应用程序代理服务器上使用以下 cmdlet 来安装新的 SSL 证书：

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

如果上述 cmdlet 由于旧证书已过期而失败，请使用以下 cmdlet 重新配置代理：

```powershell
$cred = Get-Credential
```

输入 AD FS 服务器上的本地管理员域用户的凭据

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他参考  
* [AD FS 对证书身份验证的备用主机名绑定的支持](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS 和 certificate KeySpec 属性信息](../technical-reference/AD-FS-and-KeySpec-Property.md)

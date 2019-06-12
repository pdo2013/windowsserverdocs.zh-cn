---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: 在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书
description: 在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9acdbe2be56b990876fe365c1f535aaa411009c5
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501625"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>在 Windows Server 2016 中管理 AD FS 和 WAP 中的 SSL 证书



本文介绍如何将新的 SSL 证书部署到你的 AD FS 和 WAP 服务器。

>[!NOTE]
>替换今后为 AD FS 场的 SSL 证书的建议的方法是使用 Azure AD Connect。  有关详细信息请参阅[更新 Active Directory 联合身份验证服务 (AD FS) 场的 SSL 证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>获取 SSL 证书
对于生产 AD FS 场建议公开受信任的 SSL 证书。 这通常是通过提交证书签名请求 (CSR) 的第三方、 公共证书提供程序中获取。 有多种方式来生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有对此文档。

- 请确保证书满足[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少证书
建议在所有 AD FS 和 Web 应用程序代理服务器使用公用 SSL 证书。 有关详细的要求，请参阅文档[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 证书要求
包括命名要求，根的信任和扩展，请参阅文档[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>替换为 AD FS SSL 证书
> [!NOTE]
> AD FS SSL 证书不在 AD FS 管理管理单元中找到的 AD FS 服务通信证书相同。 若要更改 AD FS SSL 证书，你将需要使用 PowerShell。

首先，确定你的 AD FS 服务器运行绑定模式的证书： 默认证书身份验证绑定或备用的客户端 TLS 绑定模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>替换为运行在默认证书身份验证绑定模式的 AD FS SSL 证书
默认情况下 AD FS 执行端口 443 上的设备证书身份验证和用户证书身份验证端口 49443 上的 （或不是 443 的可配置端口）。
在此模式下，使用 powershell cmdlet Set-adfssslcertificate 来管理 SSL 证书。

按以下步骤操作：

1. 首先，需要获取新证书。 这通常是通过提交证书签名请求 (CSR) 的第三方、 公共证书提供程序。 有多种方式来生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有对此文档。

    * 请确保证书满足[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦从证书提供程序获取响应，其导入到每个 AD FS 和 Web 应用程序代理服务器上的本地计算机存储。

1. 上**主**AD FS 服务器上，使用以下 cmdlet 来安装新的 SSL 证书

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

可通过执行此命令找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他说明

* Set-adfssslcertificate cmdlet 是多节点 cmdlet;这意味着它仅有运行从主服务器场中的所有节点将进行都更新。 这是 Server 2016 中的新增功能。 您必须在每个服务器上运行 Set-adfssslcertificate Server 2012 R2 上。
* Set-adfssslcertificate cmdlet 具有仅在主服务器上运行。 主服务器必须运行 Server 2016 和应将场行为级别提升到 2016年。
* Set-adfssslcertificate cmdlet 将使用 PowerShell 远程处理来配置其他 AD FS 服务器，请确保为端口 5985 (TCP) 处于打开状态的其他节点上。
* Set-adfssslcertificate cmdlet 将 adfssrv 向主体授予读取的权限的私钥的 SSL 证书。 此主体表示 AD FS 服务。 不需要 AD FS 服务帐户授予读取访问的 SSL 证书的私钥。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>替换为在备用的 TLS 绑定模式下运行的 AD FS SSL 证书
配置备用的客户端中 TLS 绑定模式，AD FS 对不同的主机名执行端口 443 上的设备证书身份验证和用户证书身份验证，端口 443 上。 用户证书主机名是使用"certauth"，例如"certauth.fs.contoso.com"AD FS 主机名前面附加。
在此模式下，使用 powershell cmdlet 集 AdfsAlternateTlsClientBinding 来管理 SSL 证书。 这将管理不仅能替代客户端 TLS 绑定，但所有其他 AD FS 在其设置的 SSL 证书绑定。

按以下步骤操作：

1. 首先，需要获取新证书。 这通常是通过提交证书签名请求 (CSR) 的第三方、 公共证书提供程序。 有多种方式来生成 CSR，包括从 Windows 7 或更高版本的电脑。 你的供应商应具有对此文档。

    * 请确保证书满足[AD FS 和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 一旦从证书提供程序获取响应，其导入到每个 AD FS 和 Web 应用程序代理服务器上的本地计算机存储。

1. 上**主**AD FS 服务器上，使用以下 cmdlet 来安装新的 SSL 证书

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

可通过执行此命令找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他说明

* 集 AdfsAlternateTlsClientBinding cmdlet 是多节点 cmdlet;这意味着它仅有运行从主服务器场中的所有节点将进行都更新。
* 集 AdfsAlternateTlsClientBinding cmdlet 具有仅在主服务器上运行。 主服务器必须运行 Server 2016 和应将场行为级别提升到 2016年。
* 集 AdfsAlternateTlsClientBinding cmdlet 将使用 PowerShell 远程处理来配置其他 AD FS 服务器，请确保为端口 5985 (TCP) 处于打开状态的其他节点上。
* 集 AdfsAlternateTlsClientBinding cmdlet 将 adfssrv 向主体授予读取的权限的私钥的 SSL 证书。 此主体表示 AD FS 服务。 不需要 AD FS 服务帐户授予读取访问的 SSL 证书的私钥。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>替换为 Web 应用程序代理的 SSL 证书
WAP 上配置的默认证书身份验证绑定或备用的客户端 TLS 绑定模式中，我们可以使用组 WebApplicationProxySslCertificate cmdlet。
若要将 Web 应用程序代理 SSL 证书，为上**每个**Web 应用程序代理服务器使用以下 cmdlet 来安装新的 SSL 证书：

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

如果上面的 cmdlet 会失败，因为旧证书已过期，重新配置的代理帐户使用以下 cmdlet:

```powershell
$cred = Get-Credential
```

输入是 AD FS 服务器上的本地管理员域用户的凭据

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他参考  
* [AD FS 对证书身份验证的备用主机名绑定的支持](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS 和证书 KeySpec 属性信息](../technical-reference/AD-FS-and-KeySpec-Property.md)

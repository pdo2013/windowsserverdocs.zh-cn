---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: "广告金融服务和 Windows Server 2016 中的 WAP 中管理 SSL 证书"
description: "广告金融服务和 Windows Server 2016 中的 WAP 中管理 SSL 证书"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>广告金融服务和 Windows Server 2016 中的 WAP 中管理 SSL 证书

>适用于：Windows Server 2016

本文介绍了如何将新 SSL 证书部署到你的广告金融服务和 WAP 服务器。

>[!NOTE]
>推荐替换 SSL 证书广告 FS 场接下来的方法是使用 Azure AD 连接。  有关详细信息，请参阅[更新 SSL 证书的 Active Directory 联合身份验证服务 (AD FS) 电场的日落](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>获取 SSL 证书
对于生产广告 FS 场公开受信任的 SSL 证书建议。 这通常是通过提交证书签名申请 (CSR) 的第三方、 公用的证书提供商获取。 有多种方式生成 CSR，包括从 Windows 7 或更高版本的电脑。 你供应商应有对此的文档。

- 请确保该证书符合[广告金融服务和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>需要多少证书
建议你跨所有广告金融服务和 Web 应用程序代理服务器使用常见 SSL 证书。 有关详细的要求，请参阅文档[广告金融服务和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>SSL 证书要求
有关包括命名的要求的信任和扩展根看到该文档[广告金融服务和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>对于广告 FS 替换 SSL 证书
> [!NOTE]
> 广告 FS SSL 证书不是广告 FS 管理贴靠中找到广告 FS 服务通信证书相同。 若要更改的广告 FS SSL 证书，你将需要使用 PowerShell。

首先，确定广告 FS 服务器正在运行哪个证书，有约束力的模式： 默认证书身份验证绑定、 或其他客户端 TLS 绑定模式。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>对于广告 FS 默认运行证书身份验证绑定模式替换 SSL 证书
默认情况下广告 FS 执行上端口 443 设备证书身份验证和端口 49443 上的用户证书身份验证 （或不是 443 配置端口）。
在此模式中，使用 powershell cmdlet 组 AdfsSslCertificate 管理 SSL 证书。

请按照以下步骤操作：

1. 首先，你将需要获取新的证书。 这通常是通过提交证书签名申请 (CSR) 的第三方、 公用的证书提供商。 有多种方式生成 CSR，包括从 Windows 7 或更高版本的电脑。 你供应商应有对此的文档。

    * 请确保该证书符合[广告金融服务和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 当你收到证书提供商的响应时，其导入到每个广告金融服务和 Web 应用程序代理服务器上的本地计算机应用商店。

1. 在**主要**广告 FS 服务器上，使用以下 cmdlet 安装新 SSL 证书

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

可以通过执行此命令找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事项

* 设置 AdfsSslCertificate cmdlet 是多节点 cmdlet;这意味着它只有从主运行，并且将更新电场的日落中的所有节点。 这是中 Server 2016 的新增功能。 Server 2012 R2 上，你必须要运行组 AdfsSslCertificate 每个服务器上。
* 设置 AdfsSslCertificate cmdlet 都有运行仅主服务器上。 主服务器 Server 2016 运行，并且应到 2016年引发电场的日落行为级别。
* 设置 AdfsSslCertificate cmdlet 将使用 PowerShell 远程配置其他广告 FS 服务器，请确保端口 5985 (TCP) 已打开其他节点。
* 设置 AdfsSslCertificate cmdlet 将授予 adfssrv 主体读取的权限专用键 SSL 证书。 此主体代表广告 FS 服务。 不需要授予广告 FS 服务帐户阅读访问 SSL 证书专用键。

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>对于广告 FS 里备用 TLS 绑定模式替换 SSL 证书
当配置备用客户端中 TLS 绑定模式，广告 FS 不同的主机上执行上端口 443 设备证书身份验证和用户证书身份验证，也端口 443 上。 用户证书主机已广告 FS 主机预先与"certauth"，例如"certauth.fs.contoso.com"。
在此模式中，使用 powershell cmdlet 组 AdfsAlternateTlsClientBinding 管理 SSL 证书。 这将替换客户端 TLS 绑定不仅广告 FS 该设置的 SSL 证书的其他所有绑定来管理。

请按照以下步骤操作：

1. 首先，你将需要获取新的证书。 这通常是通过提交证书签名申请 (CSR) 的第三方、 公用的证书提供商。 有多种方式生成 CSR，包括从 Windows 7 或更高版本的电脑。 你供应商应有对此的文档。

    * 请确保该证书符合[广告金融服务和 Web 应用程序代理 SSL 证书要求](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. 当你收到证书提供商的响应时，其导入到每个广告金融服务和 Web 应用程序代理服务器上的本地计算机应用商店。

1. 在**主要**广告 FS 服务器上，使用以下 cmdlet 安装新 SSL 证书

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

可以通过执行此命令找到证书指纹：

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>其他注意事项

* 设置 AdfsAlternateTlsClientBinding cmdlet 是多节点 cmdlet;这意味着它只有从主运行，并且将更新电场的日落中的所有节点。
* 设置 AdfsAlternateTlsClientBinding cmdlet 都有运行仅主服务器上。 主服务器 Server 2016 运行，并且应到 2016年引发电场的日落行为级别。
* 设置 AdfsAlternateTlsClientBinding cmdlet 将使用 PowerShell 远程配置其他广告 FS 服务器，请确保端口 5985 (TCP) 已打开其他节点。
* 设置 AdfsAlternateTlsClientBinding cmdlet 将授予 adfssrv 主体读取的权限专用键 SSL 证书。 此主体代表广告 FS 服务。 不需要授予广告 FS 服务帐户阅读访问 SSL 证书专用键。

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>替换为 Web 应用程序代理服务器 SSL 证书
上 WAP 配置默认证书身份验证绑定或其他客户端 TLS 绑定模式中，我们可以使用一组 WebApplicationProxySslCertificate cmdlet。
将 Web 应用程序代理 SSL 证书，在**每个**Web 应用程序代理服务器使用以下 cmdlet 安装新 SSL 证书：

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

如果上述 cmdlet 失败，因为旧证书已过期，重新配置使用以下 cmdlet 代理：

```powershell
$cred = Get-Credential
```

输入凭据的本地管理员广告 FS 服务器上的域用户

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>其他参考  
* [广告 FS 备用主机名为的绑定证书身份验证的支持](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [广告金融服务和证书 KeySpec 属性信息](../technical-reference/AD-FS-and-KeySpec-Property.md)

---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: AD FS 对证书身份验证的备用主机名绑定的支持
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: be522f4dd990a920e910950c1bf2564d795bf9fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358596"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 对证书身份验证的备用主机名绑定的支持

在许多网络上，本地防火墙策略可能不允许流量通过非标准端口，如49443。 尝试使用 Windows Server 2016 AD FS 之前的 AD FS 完成证书身份验证时，这会成为一个问题。 这是因为在同一主机上，不能有不同的设备身份验证绑定和用户证书身份验证。 默认端口443被绑定到接收设备证书，无法对其进行更改以支持同一通道中的多个绑定。 其结果是智能卡身份验证将不起作用，并且用户不知道发生了什么情况，因为没有任何指示实际发生的情况。  
  
对于 Windows Server 2016 中的 AD FS，可以完成此操作。
  
在 Windows Server 2016 上的 AD FS 中，这已发生更改。 现在我们支持两种模式，第一种模式使用不同端口（443，49443）的相同主机（即 adfs.contoso.com）。 第二个使用相同端口（443）的不同主机（adfs.contoso.com 和 certauth.adfs.contoso.com）。 这将需要一个 SSL 证书，以支持 "certauth >" 作为备用使用者名称的 "<"。 这可以在创建场时或在以后通过 PowerShell 完成。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何为证书身份验证配置备用主机名绑定  
可以通过两种方式为证书身份验证添加备用主机名绑定。 第一种情况是，使用 Windows Server 2016 的 AD FS 设置新的 AD FS 场时，如果该证书包含使用者备用名称（SAN），则它将自动设置为使用上面提到的第二种方法。 也就是说，它会自动设置两个不同的主机（sts.contoso.com 和 certauth.sts.contoso.com 具有相同的端口。 如果证书不包含 SAN，你会看到一条警告，告诉你证书使用者备用名称不支持 certauth. *。 请参阅下面的屏幕截图。 第一个示例显示一个安装，其中证书包含一个 SAN，第二个证书显示未使用的证书。  
  
![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同样，在已部署 Windows Server 2016 中的 AD FS 后，你可以使用 PowerShell cmdlet：AdfsAlternateTlsClientBinding。
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出现提示时，单击 "是" 以确认。  这应该是如此。

![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他参考

* [在 Windows Server 2016 中的 AD FS 和 WAP 中管理 SSL 证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

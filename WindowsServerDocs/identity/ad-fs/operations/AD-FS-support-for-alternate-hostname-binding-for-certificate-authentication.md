---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: AD FS 对证书身份验证的备用主机名绑定的支持
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887248"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>AD FS 对证书身份验证的备用主机名绑定的支持

>适用于：Windows Server 2016

许多网络上的本地防火墙策略可能不允许通信通过非标准端口 49443 等。 尝试完成之前 Windows Server 2016 中的 AD FS 的 AD FS 证书身份验证时，这已经成为问题。 这是因为无法在同一主机上具有不同绑定进行设备身份验证和用户证书身份验证。 默认端口 443 绑定到接收设备证书，无法更改以支持在同一个通道中的多个绑定。 结果是，智能卡身份验证将不起作用，并且用户是不知道由于没有任何迹象的事情发生了什么情况。  
  
与 Windows Server 2016 中的 AD FS 配合即可进行。
  
在 Windows Server 2016 上的 AD FS 中这已更改。 现在我们支持两种模式，第一个示例使用不同的端口 （443，49443） 使用相同的主机 (例如 adfs.contoso.com)。 第二个相同的端口 (443) 与使用不同的主机 （adfs.contoso.com 和 certauth.adfs.contoso.com）。 这将需要 SSL 证书来支持"certauth < adfs 服务名称 >"。 作为替代使用者名称。 这可以在创建场或更高版本通过 PowerShell 的时间。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何配置用于证书身份验证的备用主机名绑定  
有两种方法，您可以添加证书的身份验证的备用主机名绑定。 首先，设置包含适用于 Windows Server 2016 中，AD FS 的新 AD FS 场，如果证书中包含使用者可选名称 (SAN)，则它将自动安装程序以使用上面提到的第二个方法时。 也就是说，它将自动设置两个不同的主机 （sts.contoso.com 和 certauth.sts.contoso.com 与同一个端口。 如果证书不包含 SAN，然后将看到一个警告，通知您证书使用者可选名称中不支持 certauth.*。 请参阅下面的屏幕截图。 第一个显示了的安装其中证书有一个 SAN，第二个显示未一个证书。  
  
![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同样，Windows Server 2016 中的 AD FS 部署后可以使用 PowerShell cmdlet:Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

出现提示时，单击是以确认。  而且应是它。

![备用主机名绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他参考

* [在 AD FS 和 Windows Server 2016 中的 WAP 中管理 SSL 证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

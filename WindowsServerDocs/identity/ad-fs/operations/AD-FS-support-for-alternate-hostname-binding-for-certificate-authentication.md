---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: "广告 FS 备用主机名为的绑定证书身份验证的支持"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>广告 FS 备用主机名为的绑定证书身份验证的支持

>适用于：Windows Server 2016

在多个网络上的本地防火墙策略可能不允许交通通过非标准端口 49443 等。 这会变得问题时，尝试完成与 Windows Server 2016 中的广告 FS 之前广告 FS 证书身份验证。 这是因为无法没有设备身份验证和用户证书身份验证的不同绑定同一台主机上。 默认端口 443 绑定接收设备证书，以在相同的频道支持多个绑定无法更改。 结果已，智能卡身份验证将不起作用，因为没有的实际发生迹象发生了什么变化不知道的用户。  
  
在 Windows Server 2016 的广告 FS 与这可以实现。
  
在 Windows Server 2016 上广告 FS 这已更改。 我们支持两种模式，现在第一个与 （49443 443） 的其他端口中使用同一台主机 (即 adfs.contoso.com)。 第二个不同主机 （adfs.contoso.com 和 certauth.adfs.contoso.com） 使用相同端口 (443)。 这将需要 SSL 证书支持"certauth。 < adfs 服务名称 >"作为备用主题名称。 这可以在时间电场的日落创建或更高版本通过 PowerShell 完成。  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>如何配置为证书身份验证的备用主机名称绑定  
有两种方法，你可以添加证书身份验证备用主机名称绑定。 如果证书含有者备用名称 (SAN)，然后它将自动设置为使用如上所述第二种方法新广告 FS 场与广告 FS 适用于 Windows Server 2016，为孩子设置时，第一。 也就是说，它将自动设置两种不同的主机 （sts.contoso.com 和 certauth.sts.contoso.com 使用相同的端口。 如果证书不包含 SAN，你将看到告知你证书主题备用名称不支持 certauth.* 一条警告。 请参阅下面的屏幕截图。 第一个显示位置证书了 SAN 和第二个节目某个证书存在不未安装。  
  
![备用主机绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![备用主机绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
同样，在部署 Windows Server 2016 中的广告 FS 后可以使用 PowerShell cmdlet： 组 AdfsAlternateTlsClientBinding。
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

看到提示后，单击是进行确认。  并且，应使用它。

![备用主机绑定](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>其他参考

* [广告金融服务和 Windows Server 2016 中的 WAP 中管理 SSL 证书](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

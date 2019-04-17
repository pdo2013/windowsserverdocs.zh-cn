---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: "联合身份验证的服务器代理证书要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>联合身份验证的服务器代理证书要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

使用安全套接字层 \(SSL\) 服务器身份验证证书所需联盟服务器代理角色中的 Active Directory 联合身份验证服务 \(AD FS\) 中运行的服务器。 联合身份验证的服务器代理使用 SSL 服务器的身份验证证书安全的 Web 客户端的 Web 服务器交通通信。  
  
向 Internet 上的计算机不包括在企业公钥结构通常公开联合身份验证的服务器代理 \(PKI\)。 因此，使用公用 \(third\-party\) 证书颁发机构颁发服务器身份验证证书 \(CA\)，例如，VeriSign。  
  
当你已联盟服务器代理电场的日落时，所有联合身份验证的服务器代理计算机必须使用相同的服务器身份验证证书。 有关详细信息，请参阅[何时创建联合身份验证的服务器代理场](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
请务必验证中服务器身份验证证书的对象名称匹配广告 FS 管理 snap\ 中指定的联合身份验证服务名称值。 若要查找此值，打开 snap\ 中 right\ 键单击**服务**，单击**编辑联合身份验证服务属性**，然后查找中的值**联合身份验证服务名称**文本框。  
  
有关使用 SSL 证书的常规信息，请参阅配置中 IIS 7.0 安全套接字层 \ ([http:///\/go.microsoft.com\/fwlink\/？LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\)，但在 IIS 7.0 配置服务器证书 \ ([http:///\/go.microsoft.com\/fwlink\/？LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\)。  
  
> [!NOTE]  
> 客户端身份验证证书并不需要的广告 FS 联合 server 代理。  
  
如果你使用的任何证书具有吊销列出 \(CRLs\) 证书，配置证书服务器必须是能够联系分发 Crl 服务器。 CRL 的键入确定使用哪个端口。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

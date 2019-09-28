---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: 联合服务器代理的证书要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408149"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>联合服务器代理的证书要求

需要使用安全套接字层 \(SSL @ no__t server authentication 证书，在联合服务器代理角色中运行的服务器 Active Directory 联合身份验证服务 \(AD FS @ no__t-1。 联合服务器代理使用 SSL 服务器身份验证证书保护与 Web 客户端的 Web 服务器流量通信。  
  
联合服务器代理通常公开给 Internet 上未包括在企业公钥基础结构中的计算机 \(PKI @ no__t-1。 因此，请使用由公共 \(third @ no__t-1party @ no__t 证书颁发机构颁发的服务器身份验证证书 \(CA @ no__t-4，例如 VeriSign。  
  
如果你具有联合服务器代理场，则所有联合服务器代理计算机都必须使用相同的服务器身份验证证书。 有关详细信息，请参阅 [何时创建联合服务器代理场](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
务必确保服务器身份验证证书中的使用者名称与 AD FS 管理 snap @ no__t-0in 中指定的联合身份验证服务名称值匹配。 若要查找此值，请打开 snap @ no__t-0in，right @ no__t-1click **Service**，单击 "**编辑联合身份验证服务属性**"，然后在 "**联合身份验证服务名称**" 文本框中找到该值。  
  
有关使用 SSL 证书的一般信息，请参阅在 IIS 7.0 中配置安全套接字层 \([http： \/\/go.microsoft.com @ no__t-4fwlink @ no__t？ LinkID @ no__t-6108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) 和在 IIS 7.0 中配置服务器证书 \([http： 01go.microsoft.com @ no__t-12fwlink @ no__t-13？ LinkID @ no__t-14108545](https://go.microsoft.com/fwlink/?LinkID=108545)5。  
  
> [!NOTE]  
> AD FS 联合服务器代理不需要客户端身份验证证书。  
  
如果你使用的任何证书具有证书吊销列表 \(CRLs @ no__t-1，则具有配置的证书的服务器必须能够联系分发 Crl 的服务器。 CRL 的类型确定了使用的端口。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

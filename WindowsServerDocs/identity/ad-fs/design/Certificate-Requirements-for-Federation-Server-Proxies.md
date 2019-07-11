---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: 联合服务器代理的证书要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191662"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>联合服务器代理的证书要求

在 Active Directory 联合身份验证服务中的联合身份验证服务器代理角色中运行的服务器\(AD FS\)都需要使用安全套接字层\(SSL\)服务器身份验证证书。 联合服务器代理使用 SSL 服务器身份验证证书保护与 Web 客户端的 Web 服务器流量通信。  
  
联合服务器代理通常在企业公钥基础结构中不包括 Internet 上的计算机上公开\(PKI\)。 因此，使用服务器身份验证证书颁发的公共\(第三个\-方\)证书颁发机构\(CA\)，例如 VeriSign。  
  
联合服务器代理场后，所有联合服务器代理计算机必须都使用相同的服务器身份验证证书。 有关详细信息，请参阅 [何时创建联合服务器代理场](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
务必要验证是否在联合身份验证服务名称值，它是服务器身份验证证书匹配的使用者名称中 AD FS 管理管理单元指定\-中。 若要查找此值，请打开管理单元\-中，右键\-单击**服务**，单击**编辑联合身份验证服务属性**，然后在查找中的值**联合身份验证服务名称**文本框。  
  
有关使用 SSL 证书的常规信息，请参阅 IIS 7.0 中配置安全套接字层\( [http:\/\/go.microsoft.com\/fwlink\/？LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \)和 IIS 7.0 中配置服务器证书\( [http:\/\/go.microsoft.com\/fwlink\/?LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\)。  
  
> [!NOTE]  
> 不需要 AD FS 联合服务器代理的客户端身份验证证书。  
  
如果任何证书，使用具有证书吊销列表\(Crl\)，具有配置的证书的服务器必须能够联系分发 Crl 的服务器。 CRL 的类型确定了使用的端口。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

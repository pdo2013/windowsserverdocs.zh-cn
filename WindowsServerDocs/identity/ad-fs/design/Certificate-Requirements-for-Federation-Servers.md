---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: 联合服务器的证书要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359180"
---
# <a name="certificate-requirements-for-federation-servers"></a>联合服务器的证书要求

在任何 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t 设计中，必须使用各种证书来保护通信，并促进 Internet 客户端与联合服务器之间的用户身份验证。 每个联合服务器必须具有服务通信证书和令牌 @ no__t-0signing 证书，然后才能参与 AD FS 通信。 下表描述了与联合服务器关联的证书类型。  
  
|证书类型|描述|  
|--------------------|---------------|  
|标记 @ no__t-0signing 证书|令牌 @ no__t-0signing 证书是一个 X509 证书。 联合服务器使用关联的 no__t/0private 密钥对，对它们产生的所有安全令牌进行数字签名。 这包括已发布的联合元数据和项目分辨率请求的签名。<br /><br />可以在 AD FS 管理 snap @ no__t-1in 中配置多个令牌 @ no__t-0signing 证书，以允许在一个证书即将过期时使用证书滚动更新。 默认情况下，列表中的所有证书都已发布，但只有主令牌 @ no__t-0signing 证书由 AD FS 用来对令牌进行实际签名。 你选择的所有证书都必须具有相应的私钥。<br /><br />有关详细信息，请参阅 [Token-Signing Certificates](Token-Signing-Certificates.md) 和 [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服务通信证书|联合服务器使用服务器身份验证证书（也称为服务通信） Windows Communication Foundation \(WCF @ no__t 消息安全性。 默认情况下，在 Internet Information Services \(IIS @ no__t 中，联合服务器使用同一证书作为安全套接字层 \(SSL @ no__t 证书。 **注意：** AD FS 管理 snap @ no__t-0in 是指作为服务通信证书的联合服务器的服务器身份验证证书。<br /><br />有关详细信息，请参阅[服务通信证书](Service-Communications-Certificates.md)和[设置服务通信证书](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />由于服务通信证书必须受客户端计算机信任，因此我们建议你使用由受信任的证书颁发机构签名的证书 \(CA @ no__t-1。 你选择的所有证书都必须具有相应的私钥。|  
|安全套接字层 @no__t 0SSL @ no__t-1 证书|联合服务器使用 SSL 证书来保护与 Web 客户端以及与联合服务器代理的 SSL 通信的 Web 服务通信。<br /><br />由于 SSL 证书必须受客户端计算机信任，因此我们建议你使用由受信任的 CA 签名的证书。 你选择的所有证书都必须具有相应的私钥。|  
|标记 @ no__t-0decryption 证书|此证书用于解密此联合服务器收到的令牌。<br /><br />你可以有多个解密证书。 这样，资源联合服务器就可以在将新证书设置为主解密证书后，能够解密使用较旧的证书颁发的令牌。 所有证书都可以用于解密，但是在联合元数据中，只会实际发布主令牌 @ no__t-0decrypting 证书。 你选择的所有证书都必须具有相应的私钥。<br /><br />有关详细信息，请参阅[添加令牌解密证书](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
你可以通过 Microsoft 管理控制台请求服务通信证书来请求和安装 SSL 证书或服务通信证书，\(MMC @ no__t-1 snap @ no__t-2in for IIS。 有关使用 SSL 证书的更多常规信息，请参阅 [IIS 7.0：在 IIS 7.0 @ no__t 和 [IIS 7.0 中配置安全套接字层：在 IIS 7.0 中配置服务器证书 @ no__t-0。  
  
> [!NOTE]  
> 在 AD FS 可以将用于数字签名的安全\(哈\)希算法 SHA 级别更改为 sha\-1 或 sha\-256 \(更安全\)。 AD FSdoes 不支持将证书用于其他哈希方法，@no__t 例如与 Makecert command @ no__t-1line tool @ no__t-2 一起使用的默认哈希算法。 作为安全性最佳做法，我们建议使用默认情况下\-\)为所有\(签名设置的 SHA 256。 仅在必须与不支持使用 SHA @ no__t-1256 通信的产品进行互操作的情况下才使用 SHA @ no__t-01，如非 @ no__t 2Microsoft 产品或 AD FS 1。 *x*。  
  
## <a name="determining-your-ca-strategy"></a>确定 CA 策略  
AD FS 不需要由 CA 颁发证书。 但是，默认情况下也用作服务通信证书 @ no__t @no__t 0the 证书的 SSL 证书必须由 AD FS 客户端信任。 建议你不要为这些证书类型使用 self @ no__t-0signed 证书。  
  
> [!IMPORTANT]  
> 使用 no__t-0signed，在生产环境中的 SSL 证书可以使帐户伙伴组织中的恶意用户能够控制资源伙伴组织中的联合服务器。 存在此安全风险的原因是，no__t-0signed 证书是根证书。 必须将这些用户添加到另一个联合服务器的受信任的根存储中 @no__t 0for 例如，资源联合服务器 @ no__t，这会使该服务器容易受到攻击。  
  
从 CA 收到证书后，请确保将所有的证书导入到本地计算机的个人证书存储中。 可以通过证书 mmc 管理单元\-将证书导入到个人存储中。  
  
作为使用证书 snap @ no__t-0in 的替代方法，你还可以在将 SSL 证书分配给默认网站时，使用 IIS 管理器 snap @ no__t-1in 导入 SSL 证书。 有关详细信息，请参阅将[服务器身份验证证书导入到默认](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)网站。  
  
> [!NOTE]  
> 在将成为联合服务器的计算机上安装 AD FS 软件之前，请确保这两个证书位于本地计算机的 "个人" 证书存储中，并且已将 SSL 证书分配给默认网站。 有关设置联合服务器所需的任务顺序的详细信息，请参阅 [Checklist：设置联合服务器 @ no__t。  
  
根据你的安全和预算需求，请仔细考虑哪些证书将由公共 CA 或企业 CA 来获得。 下图显示了关于给定证书类型的建议的 CA 颁发者。 此建议反映了有关安全和成本的最佳 @ no__t-0practice 方法。  
  
![证书要求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>证书吊销列表  
如果你使用的任何证书具有 CRL，则具有配置证书的服务器必须能够联系分发 CRL 的服务器。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: 联合服务器的证书要求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827098"
---
# <a name="certificate-requirements-for-federation-servers"></a>联合服务器的证书要求

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在任何 Active Directory 联合身份验证服务\(AD FS\)设计中，必须使用各种证书来保护通信和 Internet 客户端与联合身份验证服务器之间的用户身份验证提供便利。 每个联合身份验证服务器必须具有服务通信证书和令牌\-签名证书才可以参与 AD FS 通信。 下表介绍与联合身份验证服务器相关联的证书类型。  
  
|证书类型|描述|  
|--------------------|---------------|  
|令牌\-签名证书|令牌\-签名证书是一个 X509 证书。 联合身份验证服务器使用关联的公共\/专用密钥对，它们生成的所有安全令牌进行数字签名。 这包括已发布的联合元数据和项目分辨率请求的签名。<br /><br />您可以有多个令牌\-签名证书配置 AD FS 管理管理单元\-以一个证书即将过期时允许证书滚动更新。 默认情况下，该列表中的所有证书都均，但只有主令牌\-签名证书由 AD FS 来进行实际签名令牌。 你选择的所有证书都必须具有相应的私钥。<br /><br />有关详细信息，请参阅 [Token-Signing Certificates](Token-Signing-Certificates.md) 和 [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服务通信证书|联合身份验证服务器使用服务器身份验证证书，也称为 Windows Communication Foundation 的服务通信\(WCF\)消息安全。 默认情况下，这是相同的证书的联合身份验证服务器用作安全套接字层\(SSL\)证书在 Internet Information Services \(IIS\)。 **注意：** 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。<br /><br />有关详细信息，请参阅[服务通信证书](Service-Communications-Certificates.md)并[设置服务通信证书](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />由于服务通信证书必须受客户端计算机，我们建议使用受信任的证书颁发机构签名的证书\(CA\)。 你选择的所有证书都必须具有相应的私钥。|  
|安全套接字层\(SSL\)证书|联合服务器使用 SSL 证书来保护与 Web 客户端以及与联合服务器代理的 SSL 通信的 Web 服务通信。<br /><br />由于 SSL 证书必须受客户端计算机信任，因此我们建议你使用由受信任的 CA 签名的证书。 你选择的所有证书都必须具有相应的私钥。|  
|令牌\-解密证书|此证书用于解密此联合身份验证服务器接收的令牌。<br /><br />你可以有多个解密证书。 这使资源联合身份验证服务器以便能够解密后新的证书设置为主解密证书使用较旧的证书颁发的令牌。 所有证书可以都用于解密，但只有主令牌\-联合身份验证元数据中实际发布解密证书。 你选择的所有证书都必须具有相应的私钥。<br /><br />有关详细信息，请参阅[添加令牌解密证书](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
可以请求并通过请求服务通信证书通过 Microsoft 管理控制台来安装 SSL 证书或服务通信证书\(MMC\)对齐\-中的 IIS。 有关使用 SSL 证书的更多常规信息，请参阅 [IIS 7.0：配置安全套接字层在 IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544)和[IIS 7.0:在 IIS 7.0 中配置服务器证书](https://go.microsoft.com/fwlink/?LinkID=108545)。  
  
> [!NOTE]  
> 在 AD FS 中，您可以更改安全哈希算法\(SHA\)用于任一 sha 的数字签名的级别\-1 或 SHA\-256\(更安全\)。 AD FSdoes 支持证书的使用与其他哈希方法，如 MD5\(与 Makecert.exe 命令一起使用的默认哈希算法\-行工具\)。 作为安全性最佳实践，我们建议使用 SHA\-256\(这默认情况下设置\)所有签名。 SHA\-我们建议仅在其中你必须与进行互操作不支持使用 SHA 通信的产品的方案中使用\-256，如非\-Microsoft 产品或 AD FS 1。 *x*。  
  
## <a name="determining-your-ca-strategy"></a>确定 CA 策略  
AD FS 不需要由 CA 颁发证书。 但是，SSL 证书\(还用作默认情况下服务通信证书的证书\)必须受 AD FS 客户端。 我们建议您不要使用自助\-签名证书，对于这些证书类型。  
  
> [!IMPORTANT]  
> 使用自助\-已签名，在生产环境中的 SSL 证书可以使恶意用户帐户伙伴组织控制资源伙伴组织中的联合身份验证服务器中。 由于存在此安全风险自\-签名的证书是根证书。 必须将它们添加到另一个联合身份验证服务器的受信任的根存储区\(例如，资源联合身份验证服务器\)，这样可以使该服务器容易受到攻击。  
  
从 CA 收到证书后，请确保将所有的证书导入到本地计算机的个人证书存储中。 可以证书导入到具有在证书 MMC 管理单元的个人存储区\-中。  
  
除了使用证书管理单元\-中，您还可以导入具有在 IIS 管理器管理单元的 SSL 证书\-中时，将分配 SSL 证书的默认 Web 站点。 有关详细信息，请参阅[服务器身份验证证书导入到默认网站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
> [!NOTE]  
> 在将成为联合身份验证服务器的计算机上安装 AD FS 软件之前，请确保在本地计算机个人证书存储区中的这两个证书和 SSL 证书分配给默认网站。 有关任务所需设置联合身份验证服务器的顺序的详细信息，请参阅[核对清单：设置联合身份验证服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
根据你的安全和预算需求，请仔细考虑哪些证书将由公共 CA 或企业 CA 来获得。 下图显示了关于给定证书类型的建议的 CA 颁发者。 此建议反映了最佳\-练习有关安全和成本的方法。  
  
![证书要求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>证书吊销列表  
如果你使用的任何证书具有 CRL，则具有配置证书的服务器必须能够联系分发 CRL 的服务器。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

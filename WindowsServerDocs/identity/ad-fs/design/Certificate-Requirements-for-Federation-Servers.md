---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: "服务器联合身份验证的证书要求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>服务器联合身份验证的证书要求

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在任何 Active Directory 联合身份验证服务 \(AD FS\) 设计，必须使用不同的证书保护通信，便于实施 Internet 客户端和服务器联盟之间的用户身份验证。 它可以参与广告 FS 通信之前，必须服务通信证书和 token\ 签名证书每个联合身份验证的服务器。 下表介绍了联合身份验证的服务器与相关联的证书类型。  
  
|证书类型|描述|  
|--------------------|---------------|  
|Token\ 签名证书|Token\ 签名证书是提供 X509 证书。 联合身份验证的服务器使用关联的 public\/专用关键对进行数字签名它们产生的所有安全标记。 这包括的已发布的联盟元数据和项目分辨率请求登录。<br /><br />你可以让配置 snap\ 中广告 FS 管理，以使证书翻转一个证书时靠近过期的通知在多个 token\ 签名证书。 默认情况下发布该列表中的所有证书，但只有主要 token\ 签名证书由广告 FS 用于实际上登录标记。 选择你的所有证书都必须对应的专用键。<br /><br />有关详细信息，请参阅[令牌签名证书](Token-Signing-Certificates.md)和[添加令牌签名证书](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服务通信证书|联合身份验证的服务器使用服务器身份验证证书，也称为服务通信为 Windows 通信基础 \(WCF\) 邮件安全。 默认情况下，这是，Internet 信息服务 \(IIS\) 中的安全套接字层 \(SSL\) 证书联合服务器它使用同一个证书。 **注意：**广告 FS 管理 snap\ 中指联盟服务器服务通信证书为服务器身份验证证书。<br /><br />有关详细信息，请参阅[服务通信证书](Service-Communications-Certificates.md)和[设置服务通信证书](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />因为在客户端计算机必须信任服务通信证书，我们建议你使用已签名的受信任的证书颁发机构证书 \(CA\)。 选择你的所有证书都必须对应的专用键。|  
|安全套接字层 \(SSL\) 证书|联合身份验证的服务器使用 SSL 证书安全的 Web 客户端和服务器联合身份验证的代理 SSL 通信的 Web 服务交通。<br /><br />因为在客户端计算机必须信任 SSL 证书，我们建议你使用的受信任的 CA 由签名证书。 选择你的所有证书都必须对应的专用键。|  
|Token\ 解密证书|此证书用于解密收到的此联盟服务器的标记。<br /><br />你可以有多个解密证书。 这样，能够解密标记新的证书设置为主要解密证书后，用较旧的证书颁发的资源联盟服务器。 所有的证书可用于解密，但只有主要 token\ 解密证书实际发布联盟元数据中。 选择你的所有证书都必须对应的专用键。<br /><br />有关详细信息，请参阅[添加令牌解密证书](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
你可以请求，并通过申请通过 Microsoft 管理控制台 \(MMC\) snap\ 服务通信证书安装 SSL 证书或服务通信证书-在 iis。 有关使用 SSL 证书的详细信息，请参阅[IIS 7.0：配置中 IIS 7.0 安全套接字层](https://go.microsoft.com/fwlink/?LinkID=108544)和[IIS 7.0: IIS 7.0 配置服务器证书](https://go.microsoft.com/fwlink/?LinkID=108545)。  
  
> [!NOTE]  
> 广告 FS 中，你可以更改数字签名 SHA\ 1 或 SHA\ 256 \(more secure\) 使用安全哈希算法 \(SHA\) 级别。 广告 FSdoes 不与其他哈希方法，如 MD5 支持证书的使用 \（默认哈希算法，可与 Makecert.exe command\ 行 tool\）。 作为安全的最佳做法，我们建议你使用 SHA\ 256 \（这由设置 default\）所有签名。 仅在你必须交互不支持通信，例如的 non\ Microsoft 产品或广告 F 1 使用 SHA\ 256 在产品的情况下使用建议 SHA\ 1。 *x *.  
  
## <a name="determining-your-ca-strategy"></a>确定 CA 策略  
广告 FS 不需要证书颁发由 CA。 但是，SSL 证书 \ 必须广告 FS 客户端信任（也用作默认情况下服务通信 certificate\ 证书）。 我们建议你不这些证书类型使用 self\ 签名证书。  
  
> [!IMPORTANT]  
> 使用 self\ 登录，SSL 生产环境中的证书可允许来控制联合身份验证的服务器在资源合作伙伴公司帐户合作伙伴公司恶意用户。 此安全风险存在由于 self\ 签名证书根证书。 必须将他们添加到另一台联合身份验证的服务器受信任的根应用商店 \ (例如，资源联盟 server\)，这可以保持该服务器易受攻击。  
  
你收到证书从 CA 后，确保所有证书导都入到本地计算机个人证书应用商店。 你可以将证书导入到个人证书 MMC snap\ 在与应用商店。  
  
或者使用 snap\ 中的证书，你可以导入 SSL 证书与次 IIS 管理器 snap\ 入将 SSL 证书分配到默认的网站。 有关详细信息，请参阅[将服务器身份验证证书导入默认网站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
> [!NOTE]  
> 将成为联盟服务器的计算机上安装广告 FS 软件之前，请确保在本地计算机个人证书应用商店中的两个证书并且 SSL 证书赋予默认网站。 有关的所需设置联盟服务器的任务订单详细信息，请参阅[清单：设置联合服务器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
根据你的安全和 budget 要求，请仔细考虑 CA 或公司 CA 将通过公众获取该证书。 下图显示推荐的 CA 颁发给定的证书类型。 此建议反映关于安全性和成本 best\ 练习方法。  
  
![证书要求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>证书吊销列表  
如果你使用的任何证书具有 Crl，配置证书服务器必须是能够联系分发 Crl 服务器。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

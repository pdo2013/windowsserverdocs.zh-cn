---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: 令牌签名证书
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9db69cfb2eb42af90b392433a6e05eaab9978160
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190815"
---
# <a name="token-signing-certificates"></a>令牌签名证书

联合身份验证服务器需要令牌\-签名证书，以防止攻击者更改或伪造安全令牌以尝试对联合资源未经授权的访问。 私有\/配对的公共密钥用于令牌\-签名证书是最重要的验证任何的机制联合合作关系，因为这些密钥验证安全令牌是否由有效的合作伙伴颁发联合身份验证服务器，并在传输期间未修改该令牌。  
  
## <a name="token-signing-certificate-requirements"></a>令牌\-签名证书要求  
令牌\-签名证书必须满足以下要求才能与 AD FS 配合使用：  
  
-   令牌\-签名证书以成功登录的安全令牌，令牌\-签名证书必须包含私钥。  
  
-   AD FS 服务帐户必须有权访问令牌\-签名证书的私钥在本地计算机的个人存储中。 这是由负责安装程序。 此外可以使用 AD FS 管理管理单元\-以确保此访问权限，如果随后更改令牌\-签名证书。  
  
> [!NOTE]  
> 它是公钥基础结构\(PKI\)最佳做法不共享多个目的的私钥。 因此，不使用该令牌的联合身份验证服务器安装的服务通信证书\-签名证书。  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何标记\-合作伙伴中使用签名证书  
每个令牌\-签名证书包含私钥的加密密钥和用于进行数字签名的公钥\(通过私钥\)安全令牌。 更高版本，它们接收伙伴联合身份验证服务器后，这些密钥验证真实性\(通过公钥\)的加密的安全令牌。  
  
因为每个安全令牌进行数字签名的帐户伙伴，资源伙伴可以验证确实由帐户伙伴颁发的安全令牌和未进行修改。 通过合作伙伴的令牌的公钥部分来验证数字签名\-签名证书。 验证签名，资源联合身份验证服务器生成其自身为其组织的安全令牌和它对其自己的令牌的安全令牌进行签名后\-签名证书。  
  
联合身份验证伙伴环境中，当令牌\-已由 CA 颁发签名证书时，请确保：  
  
1.  证书吊销列表\(Crl\)的证书都可以访问信赖方和信任的联合身份验证服务器的 Web 服务器。  
  
2.  信赖方和信任的联合身份验证服务器的 Web 服务器信任的根 CA 证书。  
  
资源伙伴中的 Web 服务器使用的公钥令牌\-签名证书来验证安全令牌是否由资源联合身份验证服务器签名。 然后，Web 服务器允许对客户端的相应访问权限。  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>有关令牌的部署注意事项\-签名证书  
在部署中安装新的 AD FS 的第一个联合服务器时，必须获取令牌\-签名证书并将其安装在该联合身份验证服务器上的本地计算机个人证书存储中。 可以获取令牌\-签名证书由请求一个从企业 CA 或公共 CA 或通过创建自\-签名的证书。  
  
在部署 AD FS 场时，令牌\-方式不同，具体取决于如何创建服务器场安装签名证书。  
  
两个服务器场的选项，你可以考虑当获取令牌\-签名证书为你的部署：  
  
-   从一个令牌的私钥\-签名证书在一个场中的所有联合身份验证服务器之间共享。  
  
    在联合服务器场环境中，我们建议所有联合服务器都共享\(或重用\)相同的令牌\-签名证书。 你可以安装单个标记\-签名证书从联合身份验证服务器上的 CA，然后导出私钥，前提是已颁发的证书标记为可导出。  
  
    如中所示下图中，通过单个私钥令牌\-于场中的所有联合服务器可以共享签名证书。 此选项 — 相比于以下"唯一令牌\-签名证书"选项，降低了成本，如果计划获取的令牌\-签名公用 ca 颁发的证书。  
  
![令牌签名](media/adfs2_fedserver_certstory_3.gif)  
  
-   唯一令牌\-签名证书，对于每台联合服务器场中。  
  
    当使用多个时，在您的服务器场，在该场中每个服务器的唯一证书将令牌签名使用其自己唯一的私钥。  
  
    下图中所示，可以获取单独的标记\-签名证书，对于场中的每个单个联合身份验证服务器。 此选项是开销更大，如果你打算获取你的令牌\-签名公用 ca 颁发的证书。  
  
![令牌签名](media/adfs2_fedserver_certstory_4.gif)  
  
有关使用 Microsoft 证书服务作为您的企业 CA 时安装证书的信息，请参阅[IIS 7.0:在 IIS 7.0 中创建域服务器证书](https://go.microsoft.com/fwlink/?LinkId=108548)。  
  
有关从公共 CA 安装证书的信息，请参阅 [IIS 7.0：请求 Internet 服务器证书](https://go.microsoft.com/fwlink/?LinkId=108549)。  
  
有关安装自助\-签名证书，请参阅[IIS 7.0:创建自\-签名服务器证书在 IIS 7.0 中的](https://go.microsoft.com/fwlink/?LinkID=108271)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

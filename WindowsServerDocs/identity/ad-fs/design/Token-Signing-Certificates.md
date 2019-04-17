---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: "令牌签名证书"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>令牌签名证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合身份验证的服务器需要 token\ 签名证书，以防止攻击者更改或伪造安全标记，以尝试获取到联盟资源的未授权的访问。 配对，它 private\/公钥 token\ 签名证书用于是任何联盟合作的最重要的验证机制，因为这些键验证，由无效的合作伙伴联盟服务器颁发安全标记，没有在公交期间修改标记。  
  
## <a name="token-signing-certificate-requirements"></a>Token\ 签名证书要求  
Token\ 签名证书，必须满足以下要求处理广告 FS:  
  
-   成功登录安全令牌 token\ 签名证书，token\ 签名证书必须包含专用键。  
  
-   广告 FS 服务帐户必须在本地计算机个人应用商店中访问 token\ 签名证书专用密钥。 这是关注安装程序。 你还可以使用 snap\ 中广告 FS 管理以确保这访问，如果你以后改变 token\ 签名证书。  
  
> [!NOTE]  
> 它是公钥 \(PKI\) 最佳做法不共享专用供多个键。 因此，请使用你安装的 token\ 签名证书联合身份验证的服务器的服务通信证书。  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何 token\ 签名证书用于合作伙伴跨  
每个 token\ 签名证书包含专用密钥，并且用于进行数字签名的公用键 \（通过专用 key\) 安全标记。 之后，他们由合作伙伴联合服务器收到后，这些键验证的真实性 \（通过公共 key\) 的加密的安全标记。  
  
由帐户合作伙伴情况下进行数字签名的每个安全标记，因为资源合作伙伴可验证帐户合作伙伴事实上颁发安全标记，并且未进行修改。 数字签名是由合作伙伴 token\ singing 证书公钥部分验证。 在验证签名后，资源联盟服务器生成其组织为其自己安全标记，它进行签名安全令牌自己 token\ 签名证书。  
  
对于联盟合作伙伴环境，当已发布的 ca token\ 签名证书，确保：  
  
1.  证书证书吊销列表 \(CRLs\) 是信赖方和信任联盟服务器的 Web 服务器读取。  
  
2.  根证书被受信任的信赖方和 Web 服务器信任联合身份验证的服务器。  
  
在资源合作伙伴的 Web 服务器使用 token\ 签名证书公钥来验证安全令牌已被资源联合身份验证的服务器。 Web 服务器，然后允许相应客户端。  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>部署 token\ 签名证书注意事项  
部署广告 FS 全新安装中的第一个联盟服务器时，你必须获取 token\ 签名证书，并在该联合身份验证的服务器上的本机个人证书的应用商店安装它。 你可以获取 token\ 签名证书通过从企业请求一个 CA 或公用 CA 或通过创建 self\ 签名证书。  
  
部署广告 FS 电场的日落，会不同，具体取决于你如何创建服务器电场的日落安装 token\ 签名证书。  
  
有两个服务器电场的日落选项，可以考虑 token\ 签名证书获取部署时：  
  
-   从一个 token\ 签名证书中的专用密钥共享场中所有联合身份验证的服务器。  
  
    在联盟服务器电场的日落环境中，我们建议所有联合身份验证的服务器共享 \(or reuse\) 相同 token\ 签名证书。 可以从 CA 联合身份验证的服务器上安装的单个 token\ 签名证书，然后导出专用键，只要颁发的证书已标记为可导出。  
  
    下图所示从单个 token\ 签名证书专用的键可以共享到场中所有联合身份验证的服务器。 此选项，相比于下列"唯一 token\ 签名证书"选项 — 降低成本，如果你打算从公共 CA 获得 token\ 签名证书。  
  
![令牌签名](media/adfs2_fedserver_certstory_3.gif)  
  
-   没有场中每个联合身份验证的服务器唯一 token\ 签名证书。  
  
    当你使用多个时，您电场的日落，该电场的日落中的每个服务器整个唯一证书具有其自己的独特专用键登录标记。  
  
    在下图所示，你可以获取电场的日落中的每一个联盟服务器的单独 token\ 签名证书。 如果你打算从公开 CA 获得你 token\ 签名证书成本较高，则此选项。  
  
![令牌签名](media/adfs2_fedserver_certstory_4.gif)  
  
有关安装证书，当您使用 Microsoft 证书服务作为你的企业 CA 的信息，请参阅[IIS 7.0：创建 IIS 7.0 域服务器证书](https://go.microsoft.com/fwlink/?LinkId=108548)。  
  
安装来自公共认证的证书有关的信息，请参阅[IIS 7.0：请求 Internet 服务器证书](https://go.microsoft.com/fwlink/?LinkId=108549)。  
  
有关安装 self\ 签名证书的信息，请参阅[IIS 7.0：创建 IIS 7.0 Self\-Signed 服务器证书](https://go.microsoft.com/fwlink/?LinkID=108271)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

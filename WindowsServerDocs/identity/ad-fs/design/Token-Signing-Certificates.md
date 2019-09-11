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
ms.openlocfilehash: d31b7d60021e72f80c762df5b4e1a4199ea3909c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867688"
---
# <a name="token-signing-certificates"></a>令牌签名证书

联合服务器需要令牌\-签名证书，以防止攻击者在尝试获取对联合资源的未授权访问权限时更改或伪造安全令牌。 与令牌\/\-签名证书一起使用的私钥配对是任何联合合作关系的最重要验证机制，因为这些密钥验证安全令牌是否由有效的合作伙伴颁发联合服务器，并且在传输过程中未修改该令牌。  
  
## <a name="token-signing-certificate-requirements"></a>令牌\-签名证书要求  
令牌\-签名证书必须满足以下要求才能处理 AD FS：  
  
-   令牌签名证书\-必须包含私钥，\-才能让令牌签名证书成功地对安全令牌进行签名。  
  
-   AD FS 服务帐户必须在本地计算机的个人存储\-中有权访问令牌签名证书的私钥。 这由安装程序执行。 如果随后更改令牌\-\-签名证书，还可以使用 AD FS 管理 "管理单元来确保此访问权限。  
  
> [!NOTE]  
> 这是一种公钥基础\(结构\) PKI 最佳做法，不会出于多种目的共享私钥。 因此，请不要使用在联合服务器上安装的服务通信证书作为令牌\-签名证书。  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何跨\-伙伴使用令牌签名证书  
每个\-令牌签名证书都包含加密私钥和公钥，这些密钥用于通过私钥\(\)以安全令牌进行数字签名。 之后，伙伴联合服务器收到这些密钥后，这些密钥会通过加密安全令牌\(的\)公钥来验证真实性。  
  
由于每个安全令牌由帐户伙伴进行数字签名，因此资源伙伴可以验证安全令牌是否确实由帐户伙伴颁发并且未被修改。 数字签名由合作伙伴令牌\-签名证书的公钥部分进行验证。 验证签名后，资源联合服务器将为其组织生成其自己的安全令牌，并通过其自己的令牌\-签名证书对安全令牌进行签名。  
  
对于联合身份验证伙伴环境，当\-令牌签名证书已由 CA 颁发时，请确保：  
  
1.  证书吊销列表\(证书的\) crl 可用于信任联合服务器的信赖方和 Web 服务器。  
  
2.  根 CA 证书受信任的信赖方和 Web 服务器信任。  
  
资源伙伴中的 Web 服务器使用令牌\-签名证书的公钥来验证安全令牌是否由资源联合服务器签名。 然后，Web 服务器允许对客户端进行适当的访问。  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>令牌\-签名证书的部署注意事项  
在新的 AD FS 安装中部署第一台联合服务器时，必须获取令牌\-签名证书，并将其安装在该联合服务器上的本地计算机个人证书存储中。 可以通过从企业 ca\-或公共 CA 请求，或者通过创建自\-签名证书来获取令牌签名证书。  
  
部署 AD FS 场时，令牌\-签名证书的安装方式将有所不同，具体取决于你创建服务器场的方式。  
  
获取部署的令牌\-签名证书时，可以考虑以下两个服务器场选项：  
  
-   一个令牌\-签名证书中的私钥在场中的所有联合服务器之间共享。  
  
    在联合服务器场环境中，建议所有联合服务器共享\(或重复使用\)同一令牌\-签名证书。 只要颁发的证书标记为\-可导出，你就可以在联合服务器上的 CA 中安装单个令牌签名证书，然后导出私钥。  
  
    如下图所示，单个令牌\-签名证书中的私钥可以共享到场中的所有联合服务器。 如果你计划从公共 CA 获取令牌\-\-签名证书，则此选项（与以下 "唯一令牌签名证书" 选项相比）可降低成本。  
  
![令牌签名](media/adfs2_fedserver_certstory_3.gif)  
  
-   在场中的每个\-联合服务器都有一个唯一的令牌签名证书。  
  
    在整个场中使用多个唯一的证书时，该服务器场中的每个服务器都会使用其自己的唯一私钥对令牌进行签名。  
  
    如下图所示，你可以为场中的每台\-联合服务器获取单独的令牌签名证书。 如果你计划从公共 CA 获取令牌\-签名证书，则此选项会更昂贵。  
  
![令牌签名](media/adfs2_fedserver_certstory_4.gif)  
  
有关将 Microsoft 证书服务用作企业 CA 时安装证书的信息，请参阅[IIS 7.0：在 IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548)中创建域服务器证书。  
  
有关从公共 CA 安装证书的信息，请参阅 [IIS 7.0：请求 Internet 服务器证书](https://go.microsoft.com/fwlink/?LinkId=108549)。  
  
有关安装自\-签名证书的信息，请参阅[IIS 7.0：在 IIS 7.0\-](https://go.microsoft.com/fwlink/?LinkID=108271)中创建自签名服务器证书。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

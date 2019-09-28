---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: 添加令牌签名证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8b2246842dd70c06442faed995f6b883dbaf70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360095"
---
# <a name="add-a-token-signing-certificate"></a>添加令牌签名证书


Active Directory 联合身份验证服务中的联合服务器 \(AD FS @ no__t-1 需要令牌 @ no__t-2signing 证书，以防止攻击者更改或伪造安全令牌以尝试获取对联合身份验证的未经授权的访问中心. 每个\-令牌签名证书都包含加密私钥和公钥，这些密钥用于通过私钥\(\)以安全令牌进行数字签名。 稍后，伙伴联合服务器收到这些密钥后，它们将验证加密安全令牌的公钥 @ no__t （0by）的 @no__t 可靠性。  
  
> [!CAUTION]  
> 用于标记 @ no__t-0signing 的证书对于联合身份验证服务的稳定性至关重要。 由于为此目的配置的任何证书丢失或未计划删除都可能中断服务，因此应备份任何为此目的配置的证书。  
  
令牌 @ no__t-0signing 证书应链接到联合身份验证服务中的受信任的根。 你可以使用以下过程将令牌 @ no__t-0signing 证书添加到已导出文件中的 AD FS 管理 "snap @ no__t-1in。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
### <a name="to-add-a-token-signing-certificate"></a>添加令牌 @ no__t-0signing 证书  
  
1.  在 "**开始**" 屏幕上，键入 "**AD FS 管理**"，然后按 enter。  
  
2.  在控制台树中，双击 "no__t-0click **Service**"，然后单击 "**证书**"。  
  
3.  在 "**操作**" 窗格中，单击 "**添加令牌 @ No__t-2Signing 证书**" 链接。  
  
4.  在 "**浏览证书文件**" 对话框中，导航到要添加的证书文件，选择证书文件，然后单击 "**打开**"。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


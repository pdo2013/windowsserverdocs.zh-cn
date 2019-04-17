---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: "添加令牌签名证书"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>添加令牌签名证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Active Directory 联合身份验证服务 \(AD FS\) 联盟服务器需要 token\ 签名证书，以防止攻击者更改或伪造安全标记，以尝试获取到联盟资源的未授权的访问。 每个 token\ 签名证书包含专用密钥，并且用于进行数字签名的公用键 \（通过专用 key\) 安全标记。 更高版本，由合作伙伴联盟服务器接收这些键后，他们验证的真实性 \（通过公共 key\) 的加密的安全标记。  
  
> [!CAUTION]  
> 用于 token\ 签名证书至关重要联合身份验证服务的稳定性。 丢失或未计划的任何的删除配置实现此目的的证书可能会服务中断，因为你应备份任何证书配置实现此目的。  
  
Token\ 签名证书应到联合身份验证服务中受信任根链接。 下面的过程中可用于你已将导出文件从广告 FS 管理 snap\ 中添加 token\ 签名证书。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-add-a-token-signing-certificate"></a>若要添加的 token\ 签名证书  
  
1.  在**开始**屏幕上，键入**广告 FS 管理**，然后按 ENTER。  
  
2.  控制台树中 double\ 单击**服务**，然后单击**证书**。  
  
3.  在**操作**窗格中，单击**添加 Token\ 签名证书**链接。  
  
4.  在**浏览证书文件**对话框中，导航到你想要添加、选择该证书文件，然后单击证书文件**打开**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


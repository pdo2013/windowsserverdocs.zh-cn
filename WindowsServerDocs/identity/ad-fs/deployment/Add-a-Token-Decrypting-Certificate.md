---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: "添加令牌解密证书"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-decrypting-certificate"></a>添加令牌解密证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合身份验证的服务器时信赖的方联合服务器必须解密标记新的证书设置为主要解密证书后，用较旧的证书颁发，请使用 token\ 解密证书。 Active Directory 联合身份验证服务 \(AD FS\) 使用作为默认解密证书 Internet 信息服务 \(IIS\) 的安全套接字层 \(SSL\) 证书。  
  
> [!CAUTION]  
> 证书用于 token\ 解密至关重要联合身份验证服务的稳定性。 丢失或未计划的任何的删除配置实现此目的的证书可能会服务中断，因为你应备份任何证书配置实现此目的。  
  
下面的过程中可用于广告 FS 管理 snap\ 中添加 token\ 解密证书，从已将导出你的文件。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-add-a-token-decrypting-certificate"></a>若要添加的 token\ 解密证书  
  
1.  在**开始**屏幕上，键入**广告 FS 管理**，然后按 ENTER。  
  
2.  控制台树中 double\ 单击**服务**，然后单击**证书**。  
  
3.  在**操作**窗格中，单击**添加 Token\ 解密证书**链接。  
  
4.  在**浏览证书文件**对话框中，导航到你想要添加、选择该证书文件，然后单击证书文件**打开**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: "设置服务通信证书"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>设置服务通信证书

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

联合服务器中 \(AD FS\) Active Directory 联合身份验证服务用于服务通信证书安全的 Web 客户端与或联合身份验证的服务器代理安全套接字层 \(SSL\) 通信的 Web 服务交通。 这是同一个证书联合服务器用作中 Internet 信息服务 \(IIS\) SSL 证书。  
  
可以使用下面的过程中更改与广告 FS 管理 snap\ 在服务通信证书。  
  
> [!NOTE]  
> 广告 FS 管理 snap\ 中将称为服务通信证书服务器身份验证证书的联合身份验证的服务器。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
### <a name="to-set-a-service-communications-certificate"></a>若要设置服务通信证书  
  
1.  在**开始**屏幕上，键入**广告 FS 管理**，然后按 ENTER。  
  
2.  控制台树中 double\ 单击**服务**，然后单击**证书**。  
  
3.  在**操作**窗格中，单击**设置服务通信证书**链接。  
  
4.  在**选择服务通信证书**对话框中，导航到你想要设置为服务通信证书、 选择该证书文件，然后单击证书文件**打开**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


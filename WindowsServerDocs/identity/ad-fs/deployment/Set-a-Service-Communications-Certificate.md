---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: 设置服务通信证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888468"
---
# <a name="set-a-service-communications-certificate"></a>设置服务通信证书

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active Directory 联合身份验证服务中的联合身份验证服务器\(AD FS\)使用服务通信证书来保护 Web 服务通信的安全套接字层\(SSL\)与 Web 通信客户端或与联合服务器代理。 这是相同的联合身份验证服务器使用作为 Internet 信息服务中的 SSL 证书的证书\(IIS\)。  
  
可以使用以下过程来更改与 AD FS 管理管理单元的服务通信证书\-中。  
  
> [!NOTE]  
> 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-set-a-service-communications-certificate"></a>若要设置服务通信证书  
  
1.  上**启动**屏幕上，键入**AD FS 管理**，然后按 ENTER。  
  
2.  在控制台树中，双击\-单击**服务**，然后单击**证书**。  
  
3.  在中**操作**窗格中，单击**设置服务通信证书**链接。  
  
4.  在中**选择一个服务通信证书**对话框框中，导航到你想要设置服务通信证书，选择证书文件，然后单击该证书文件**打开**.  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合身份验证服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


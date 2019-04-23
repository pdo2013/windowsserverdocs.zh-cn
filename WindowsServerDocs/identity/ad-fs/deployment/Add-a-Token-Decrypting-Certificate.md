---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: 添加令牌解密证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eba51521e7ef88542bccf93d92d2e783d800b5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842208"
---
# <a name="add-a-token-decrypting-certificate"></a>添加令牌解密证书

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

联合身份验证服务器使用令牌\-解密证书时的信赖方联合身份验证服务器必须对新的证书设置为主解密证书后，使用较旧的证书颁发的令牌进行解密。 Active Directory 联合身份验证服务\(AD FS\)使用安全套接字层\(SSL\) Internet 信息服务的证书\(IIS\)作为默认解密证书。  
  
> [!CAUTION]  
> 用于令牌的证书\-解密对联合身份验证服务的稳定性至关重要。 因为丢失或非计划的删除为此配置的任何证书可能会中断服务，应备份任何为此配置的证书。  
  
可以使用以下过程添加令牌\-解密证书到 AD FS 管理管理单元\-中的文件中的已导出。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
### <a name="to-add-a-token-decrypting-certificate"></a>若要添加令牌\-解密证书  
  
1.  上**启动**屏幕上，键入**AD FS 管理**，然后按 ENTER。  
  
2.  在控制台树中，双击\-单击**服务**，然后单击**证书**。  
  
3.  在中**操作**窗格中，单击**添加令牌\-解密证书**链接。  
  
4.  在中**浏览证书文件**对话框框中，导航到你想要添加、 选择证书文件，然后单击该证书文件**打开**。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合身份验证服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


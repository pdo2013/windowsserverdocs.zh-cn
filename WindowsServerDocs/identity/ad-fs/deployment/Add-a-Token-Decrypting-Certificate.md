---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: 添加令牌解密证书
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 388414fff97705901bf52ee844b90508d62f8c83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408450"
---
# <a name="add-a-token-decrypting-certificate"></a>添加令牌解密证书

在将新的证书设置为主解密证书后，如果信赖方联合服务器必须对使用较旧证书颁发的令牌进行解密，则联合服务器将使用令牌 @ no__t-0decryption 证书。 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t-1 使用安全套接字层 \(SSL @ no__t 证书来 Internet Information Services \(IIS @ no__t-5 作为默认的解密证书。  
  
> [!CAUTION]  
> 用于标记 @ no__t-0decrypting 的证书对于联合身份验证服务的稳定性至关重要。 由于为此目的配置的任何证书丢失或未计划删除都可能中断服务，因此应备份任何为此目的配置的证书。  
  
你可以使用以下过程将令牌 @ no__t-0decrypting 证书添加到已导出文件中的 AD FS 管理 "snap @ no__t-1in。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
### <a name="to-add-a-token-decrypting-certificate"></a>添加令牌 @ no__t-0decrypting 证书  
  
1.  在 "**开始**" 屏幕上，键入 "**AD FS 管理**"，然后按 enter。  
  
2.  在控制台树中，双击 "no__t-0click **Service**"，然后单击 "**证书**"。  
  
3.  在 "**操作**" 窗格中，单击 "**添加令牌 @ No__t-2Decrypting 证书**" 链接。  
  
4.  在 "**浏览证书文件**" 对话框中，导航到要添加的证书文件，选择证书文件，然后单击 "**打开**"。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[联合服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  


---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: 将计算机加入到域
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 02df9659ee3a1121c0cee3f7c5fa21b91c36b87c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192050"
---
# <a name="join-a-computer-to-a-domain"></a>将计算机加入到域

Active Directory 联合身份验证服务\(AD FS\)正常工作，每台计算机，其功能的联合身份验证服务器必须加入到域。 联合服务器代理可能会加入到域，但这不是必需。  
  
无需将 Web 服务器加入到域中，如果 Web 服务器承载声明\-识别应用程序。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-join-a-computer-to-a-domain"></a>若要将计算机加入到域  
  
1.  上**启动**屏幕上，键入**控制面板**，然后按 ENTER。  
  
2.  导航到**系统和安全**，然后单击**系统**。  
  
3.  在“计算机名、域和工作组设置”  下，单击“更改设置”  。  
  
4.  在“计算机名”  选项卡上，单击“更改”  。  
  
5.  下**的成员**，单击**域**，键入你想要加入，然后单击此计算机的域的名称**确定**。  
  
6.  单击“确定”  ，然后重新启动计算机。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


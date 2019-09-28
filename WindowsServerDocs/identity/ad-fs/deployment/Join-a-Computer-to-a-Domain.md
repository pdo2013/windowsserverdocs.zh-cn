---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: 将计算机加入域
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9f6d657397cb07d081a229135e3e6c97c7191164
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408341"
---
# <a name="join-a-computer-to-a-domain"></a>将计算机加入域

要使 Active Directory 联合身份验证服务 \(AD FS @ no__t 为正常工作，必须将充当联合服务器的每台计算机加入域。 联合服务器代理可以加入到域中，但这不是必需的。  
  
如果 Web 服务器仅托管声明 @ no__t-0aware 应用程序，则无需将 Web 服务器加入域。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-join-a-computer-to-a-domain"></a>将计算机加入域  
  
1.  在 "**开始**" 屏幕上，键入 **"控制面板**"，然后按 enter。  
  
2.  导航到 "**系统和安全**"，然后单击 "**系统**"。  
  
3.  在“计算机名、域和工作组设置”下，单击“更改设置”。  
  
4.  在“计算机名” 选项卡上，单击“更改”。  
  
5.  在 "**隶属于**" 下，单击 "**域**"，键入希望此计算机加入的域的名称，然后单击 **"确定"** 。  
  
6.  单击“确定”，然后重新启动计算机。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


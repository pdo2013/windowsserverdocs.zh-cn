---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: "计算机到某个域加入"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>计算机到某个域加入

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

为到函数 Active Directory 联合身份验证服务 \(AD FS\)，必须到某个域加入充当联盟服务器每台计算机。 联合身份验证的服务器代理服务器可能加入域，但这不是要求。  
  
你无需到某个域加入 Web 服务器，如果的 Web 服务器托管 claims\ 意识的应用。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-join-a-computer-to-a-domain"></a>若要将计算机连接到域  
  
1.  在**开始**屏幕上，键入**“控制面板”**，然后按 ENTER。  
  
2.  导航到**系统和安全**，然后单击**系统**。  
  
3.  下**计算机名称、域和工作组设置**，单击**更改设置**。  
  
4.  在**计算机名称**选项卡上，单击**更改**。  
  
5.  下**的成员**，单击**域**，键入，这台计算机将加入，然后单击域名**确定**。  
  
6.  单击**确定**，然后重新启动计算机。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: "验证联合身份验证的服务器代理在运行"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>验证联合身份验证的服务器代理在运行

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用下面的过程验证联合身份验证的服务器代理会与中 Active Directory 联合身份验证服务 \(AD FS\) 联合身份验证服务。 运行后，运行此过程**广告 FS 联盟服务器的代理配置向导**配置计算机运行在联合身份验证的服务器代理角色。 有关如何运行该向导中的详细信息，请参阅[将计算机配置为联合身份验证的服务器代理角色](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
> [!IMPORTANT]  
> 此测试的结果是事件的成功代事件查看器中联合身份验证的服务器代理计算机上特定。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>若要验证联合身份验证的服务器代理在运行  
  
1.  登录到联合 server 代理以管理员身份登录。  
  
2.  在**开始**屏幕上，键入**事件查看器**，然后按 ENTER。  
  
3.  在详细信息窗格中，double\ 单击**应用程序和服务日志**，double\ 单击**广告 FS 事件**，然后单击**管理员**。  
  
4.  在**事件 ID**列中，查找内容事件 ID 198。  
  
    如果联合身份验证的服务器代理配置无误，你将看到新的应用程序日志带有事件 ID 198 事件查看器中的事件。 此事件验证联盟服务器代理服务器服务已成功启动和现在处于联机状态。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


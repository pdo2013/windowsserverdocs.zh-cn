---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: 验证联合服务器代理是否正常运行
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 24f4fe2a152244dc904be82c4c10abe71abffcc4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359971"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>验证联合服务器代理是否正常运行


你可以使用以下过程来验证联合服务器代理是否可以与 Active Directory 联合身份验证服务 \(AD FS @ no__t 中的联合身份验证服务进行通信。 运行**AD FS 联合服务器代理配置向导**以将计算机配置为在联合服务器代理角色中运行后，请运行此过程。 有关如何运行此向导的详细信息，请参阅[为联合服务器代理角色配置计算机](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
> [!IMPORTANT]  
> 此测试的结果是在联合服务器代理计算机上的事件查看器中成功地生成了特定事件。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>验证联合服务器代理是否正常工作  
  
1.  以管理员身份登录到联合服务器代理。  
  
2.  在 "**开始**" 屏幕上，键入**事件查看器**，然后按 enter。  
  
3.  在详细信息窗格中，\-双击 "**应用程序和服务日志**"，双击\-" **AD FS 事件**"，然后单击 "**管理员**"。  
  
4.  在“事件 ID”列中，查找事件 ID 198。  
  
    如果正确配置了联合服务器代理，则会在事件查看器的应用程序日志中看到一个新事件，事件 ID 为198。 此事件验证联合服务器代理服务已成功启动且现在处于联机状态。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


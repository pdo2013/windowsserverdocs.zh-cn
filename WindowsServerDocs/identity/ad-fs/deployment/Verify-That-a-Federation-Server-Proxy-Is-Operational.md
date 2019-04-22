---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: 验证联合服务器代理是否正常运行
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814618"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>验证联合服务器代理是否正常运行

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以使用以下过程来验证联合服务器代理可以与 Active Directory 联合身份验证服务中的联合身份验证服务通信\(AD FS\)。 在运行后运行此过程**AD FS 联合服务器代理配置向导**若要将计算机配置为在联合服务器代理角色中运行。 有关如何运行此向导的详细信息，请参阅[将计算机配置为联合服务器代理角色](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
> [!IMPORTANT]  
> 此测试的结果是在联合服务器代理计算机上的事件查看器中成功地生成了特定事件。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>若要验证联合服务器代理正常运行  
  
1.  登录到联合服务器代理，以管理员身份。  
  
2.  上**启动**屏幕上，键入**事件查看器**，然后按 ENTER。  
  
3.  在详细信息窗格中，双击\-单击**应用程序和服务日志**，双精度型\-单击**AD FS 事件**，然后单击**管理员**。  
  
4.  在“事件 ID”列中，查找事件 ID 198。  
  
    如果联合服务器代理配置正确，您会看到事件查看器中，与事件 ID 198 的应用程序日志中的新事件。 此事件证实联合服务器代理服务已成功启动且现在处于联机状态。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  


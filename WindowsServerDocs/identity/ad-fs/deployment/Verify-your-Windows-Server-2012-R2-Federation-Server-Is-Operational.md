---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: 验证 Windows Server 2012 R2 联合身份验证服务器操作
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2df8a00a953196d7ca19ea0d164abbbf6eefd829
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840738"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>验证 Windows Server 2012 R2 联合身份验证服务器操作

>适用于：Windows Server 2016, Windows Server 2012 R2

你可以使用以下过程验证联合服务器正常工作；也即，同一网络中的任何客户端都可以到达新的联合服务器。  
  
若要完成此过程，必须至少拥有本地计算机上的**用户**、**备份操作员**、**高级用户**、**管理员**成员身份或等效身份。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>过程 1：验证联合服务器是否正常运行的步骤  
  
1.  若要验证 Internet Information Services \(IIS\)联合身份验证服务器，登录到位于同一个林的联合身份验证服务器的客户端计算机上正确配置。  
  
2.  打开浏览器窗口，在地址栏中键入联合身份验证服务器的 DNS 主机名，然后追加\/adfs\/fs\/federationserverservice.asmx 到其新的联合身份验证服务器，例如：  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  按 Enter，然后在联合服务器计算机上完成下一过程。 如果你看到消息“此网站的安全证书有问题”，请单击“继续浏览此网站”。  
  
    预期输出为显示 XML 以及服务说明文档。 如果显示此页，则联合服务器上的 IIS 正常工作且成功提供页面。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>步骤 2:验证联合服务器是否正常运行的步骤  
  
1.  以管理员身份登录到新的联合身份验证服务器。  
  
2.  上**启动**屏幕上，键入**事件查看器**，然后按 ENTER。  
  
3.  在详细信息窗格中，双击\-单击**应用程序和服务日志**，双精度型\-单击**AD FS 事件**，然后单击**管理员**。  
  
4.  在中**事件 ID**列，查找事件 ID 为 100。 如果联合身份验证服务器配置正确，请参阅新的事件-事件查看器在应用程序日志 — 与事件 ID 为 100。 此事件证实联合服务器已能够成功地与联合身份验证服务进行通信。  
  
## <a name="see-also"></a>请参阅 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联合服务器场](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  


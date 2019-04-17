---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: "验证你的 Windows Server 2012 R2 联盟服务器处于运行"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2df8a00a953196d7ca19ea0d164abbbf6eefd829
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>验证你的 Windows Server 2012 R2 联盟服务器处于运行

>适用于：Windows Server 2016，Windows Server 2012 R2

可以使用下面的过程，以验证联合服务器操作;也就是说，相同网络的任何客户可以联系新联合身份验证的服务器。  
  
在会员**用户**，**备份运营商**，**电源用户**，**管理员**或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>步骤 1： 若要验证联盟服务器是否在运行  
  
1.  若要验证 Internet 信息服务 \(IIS\) 正确配置联合身份验证的服务器上，请登录到位于联合身份验证的服务器林中客户端计算机。  
  
2.  打开浏览器窗口，则在地址栏类型联盟 DNS 服务器的主机名，然后附加 \/adfs\/fs\/federationserverservice.asmx 为新联合身份验证的服务器，例如：  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  按 enter 键，然后完成联合身份验证的服务器上的下一个过程。 如果你看到此消息**此网站的安全证书有问题**，单击**继续浏览此网站**。  
  
    预期的输出为 XML 与服务描述文档的显示。 如果出现此页面，IIS 联合身份验证的服务器上成功是运营和提供的页面。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>步骤 2： 若要验证联盟服务器是否在运行  
  
1.  以管理员身份登录到新联合身份验证的服务器。  
  
2.  在**开始**屏幕上，键入**事件查看器**，然后按 ENTER。  
  
3.  在详细信息窗格中，double\ 单击**应用程序和服务日志**，double\ 单击**广告 FS 事件**，然后单击**管理员**。  
  
4.  在**事件 ID**列中，查找内容事件 ID 100。 联合身份验证的服务器配置无误，如果你看到一个新的事件 — 事件查看器中的应用程序日志中，与事件 ID 100。 此事件验证联合身份验证的服务器时无法成功与联合身份验证服务通信。  
  
## <a name="see-also"></a>请参阅 

[广告 FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012R2 广告 FS 部署指导](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署联盟服务器电场的日落](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  


---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: 何时使用身份委派
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872548"
---
# <a name="when-to-use-identity-delegation"></a>何时使用身份委派

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>什么是身份委派？  
身份委派是 Active Directory 联合身份验证服务的一项功能\(AD FS\)允许管理员\-指定的帐户可以模拟用户。 模拟用户的帐户称为“代理人”。 对于要为其进行一系列访问控制检查并且必须为处于原始请求的授权链中的每个应用程序、数据库或服务按顺序进行这些检查的许多分布式应用程序，此委派功能至关重要。 许多实际\-世界存在一个 Web 应用程序"前端"必须在其中检索数据更安全"后端"，如连接到 Microsoft SQL Server 数据库的 Web 服务。  
  
例如，现有部件\-订购网站可以以编程方式增强，因此，它允许合作伙伴组织查看其自己的购买历史记录和帐户状态。 出于安全原因，所有合作伙伴财务数据都存储在安全的数据库上专用的结构化查询语言\(SQL\)服务器。 在此情况下，在前面的代码\-最终应用程序知道有关合作伙伴组织的财务数据执行任何操作。 因此，它必须从另一台计算机上承载的网络的其他位置检索该数据\(这种情况下\)零件数据库的 Web 服务\(后端\)。  
  
此数据\-检索过程成功，一些连续的授权"手\-握手"，如下图中所示，必须执行的 Web 应用程序与零件数据库的 Web 服务之间。  
  
![标识委托](media/adfs2_identitydelegationconcept.gif)  
  
因为原始请求是对 Web 服务器本身进行的，而 Web 服务器可能位于与尝试访问 Web 服务器的用户的组织完全不同的组织中，所以随请求一起发送的安全令牌不符合访问除 Web 服务器之外的任何其他计算机所需的授权条件。 因此，可以实现原始用户请求的唯一方法是在资源伙伴组织中放置中间联合服务器，以帮助重新颁发没有适当访问权限的安全令牌。  
  
## <a name="how-does-identity-delegation-work"></a>身份委派是如何工作的？  
多层应用程序体系结构中的 Web 应用程序通常调用 Web 服务来访问公共数据或功能。 这些 Web 服务应了解原始用户的身份，以便服务可以进行授权决策并便于审核，这十分重要。 在此情况下，前面\-结束 Web 应用程序到 Web 服务作为委托表示的用户。 AD FS 通过允许 Active Directory 帐户充当向其他信赖方用户来帮助实现此方案。 身份委派方案如下图所示。  
  
![标识委托](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank 尝试访问部分\-排序另一组织中的 Web 应用程序中的历史记录。 他的客户端计算机请求并收到一个令牌从前面的 AD FS\-结束部分\-订购 Web 应用程序。  
  
2.  客户端计算机将请求发送到 Web 应用程序（包括在步骤 1 中获取的令牌）以证明客户端身份。  
  
3.  Web 应用程序需要与 Web 服务进行通信来为客户端完成事务。 Web 应用程序联系以获取与 Web 服务进行交互的委派令牌的 AD FS。 委派令牌是颁发给代理人以充当用户的安全令牌。 AD FS 将返回包含有关客户端，针对 Web 服务的声明的委派令牌。  
  
4.  Web 应用程序使用从步骤 3 中的 AD FS 来访问充当客户端 Web 服务中获取了令牌。 通过检查委派令牌，Web 服务可以确定 Web 应用程序在充当客户端。 Web 服务执行其授权策略，记录请求，并将最初由 Frank 请求的所需零件历史记录数据提供给 Web 应用程序，因而提供给 Frank。  
  
对于特定委派，AD FS 可以限制 Web 应用程序可能会为其请求委派令牌的 Web 服务。 客户端计算机不必具有 Active Directory 帐户即可成功执行此操作。 最后，如前所述，Web 服务可以轻松确定充当用户的代理人的身份。 这允许 Web 服务基于它们是直接与客户端计算机进行通信还是通过代理人来展示不同的行为。  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>为身份委派配置 AD FS  
可以使用 AD FS 管理管理单元\-以配置为身份委派的 AD FS，无论何时需要促进数据检索过程。 AD FS 配置它之后，可以生成新的安全令牌将包括授权上下文的背面\-端服务可能需要之前它可以提供对受保护数据的访问权限。  
  
AD FS 不限制可以模拟的用户。 为身份委派的 AD FS 配置后，执行以下操作：  
  
-   它确定可以向颁发机构委派哪些服务器以请求令牌来模拟用户。  
  
-   它为委派的客户端帐户和充当代理人的服务器建立并保留单独的身份上下文。  
  
可以通过添加委派授权规则对信赖方信任 AD FS 管理管理单元中配置身份委派\-中。 有关如何执行此操作的详细信息，请参阅[核对清单：为信赖方信任创建声明规则](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)。  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>配置前端\-结束 Web 应用程序为身份委派  
开发人员有几个选项，它们可用于适当编程 Web 前端\-结束应用程序或服务委派请求重定向到 AD FS 计算机。 有关如何自定义 Web 应用程序以使用身份委派的详细信息，请参阅 [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

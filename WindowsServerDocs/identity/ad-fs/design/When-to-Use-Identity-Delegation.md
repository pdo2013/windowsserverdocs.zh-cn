---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "何时使用身份委派"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>何时使用身份委派

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>什么是身份委派？  
标识委派是允许 administrator\ 指定模拟用户帐户的 Active Directory 联合身份验证服务 \(AD FS\) 的一项功能。 模拟用户的帐户被称为*委派*。 此委派功能非常重要的为其有是一串访问控制检查必须为每个应用、数据库中或服务，在授权链原始请求按顺序进行的许多分布式应用程序。 许多 real\ 世界情形中的 Web 应用程序"前端"必须检索来自更加安全"后端"，如已连接到 Microsoft SQL Server 数据库 Web 服务的数据。  
  
例如，现有的 parts\ 排序的网站可以增强编程方式，以便它使合作伙伴公司能够查看自己购买历史记录和帐户状态。 出于安全原因，所有合作伙伴财经数据都存储在专用的结构化查询语言 \(SQL\) 服务器的安全数据库中。 在此情况下，front\ 端应用程序中的代码知道的关于合作伙伴公司财经数据执行任何操作。 因此，它必须从另一台计算机承载网络上的其他位置中检索该数据 \（在此 case\) 的 Web 服务部分数据库 \(the back end\)。  
  
对于此 data\ 检索流程成功的授权"hand\ 晃动"必须采取一些连续将之间的 Web 应用程序和 Web 服务的部分数据库中，在下图所示。  
  
![标识委派](media/adfs2_identitydelegationconcept.gif)  
  
因为其原始申请已到 Web 服务器本身，这很可能位于完全不同的组织中的用户尝试访问 Web 服务器的组织中，将与该请求一起发送该安全标记不符合访问 Web 服务器之外的任何其他计算机所需的授权条件。 因此，可以将履行初始用户请求的唯一方法是通过将置于资源合作伙伴组织来帮助重新发起安全令牌没有相应的访问权限的中间联合身份验证的服务器。  
  
## <a name="how-does-identity-delegation-work"></a>标识委派如何工作？  
多层应用程序体系结构中的 web 应用程序通常呼叫 Web 服务访问常用功能或数据。 重要的是这些 Web 服务，以便可以在进行授权决定和促进审核服务了解原始用户的身份。 在此情况下，front\ 结束 Web 应用程序表示用户 Web 服务的代理。 广告 FS，从而使其作为用户访问信赖的另一方的 Active Directory 帐户便于这种情况。 标识委派方案下图所示。  
  
![标识委派](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank 尝试从另一组织中的 Web 应用程序访问 part\ 订购历史记录。 他的客户端计算机从请求和接收令牌广告 FS front\ 结束 part\ 订购 Web 应用程序。  
  
2.  客户端计算机发送到 Web 应用程序，包括第 1 步以证明客户端的身份获取该标记的请求。  
  
3.  Web 应用程序需要与 Web 服务，以完成其笔交易的客户端进行通信。 Web 应用程序联系人广告 FS 获取委派标记，以通过 Web 服务交互。 委派标记是向委托用作用户发出的安全标记。 广告 FS 返回委派标记的关于面向 Web 服务的客户端的索赔。  
  
4.  Web 应用程序使用的从第 3 步中的广告 FS 访问作为客户端的 Web 服务获取的标记。 检查委派令牌，Web 服务可以确定的 Web 应用程序，作为客户端。 Web 服务执行其授权策略、日志请求，并提供所需的部分最初请求 Frank 通过 Web 应用程序的历史记录数据，因此到 Frank。  
  
特定代理，广告 FS 可以限制为其 Web 应用程序可能会请求委派令牌 Web 服务。 客户端计算机没有有 Active Directory 帐户成功执行此操作。 最后，如之前所述，Web 服务可以轻松地确定充当用户的代理身份。 这样 Web 服务出现根据他们讲话直接与客户端计算机或通过委托不同的行为。  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>配置为身份委派广告 FS  
可以使用 snap\ 中广告 FS 管理配置身份委派广告 FS 每当你需要促进数据检索过程。 广告 FS 对其进行配置后，可生成新的安全标记，包括可能需要 back\ 结束服务授权上下文它可以提供访问权限的受保护的数据之前。  
  
广告 FS 不限制可模拟的用户。 配置为身份委派广告 FS 后，它执行以下操作：  
  
-   确定哪些服务器可以将委派模拟用户请求令牌颁发机构。  
  
-   建立，并使单独这两个委派的客户端帐户和充当代理服务器的身份上下文。  
  
你可以通过向 snap\ 中广告 FS 管理信赖的方信任添加委派授权规则配置身份委派。 有关如何执行此操作的详细信息，请参阅[清单：创建索赔规则依赖方信任](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)。  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>配置身份委派 front\ 结束 Web 应用程序  
开发人员有多个选项，它们可使用相应地程序委派请求重定向到广告 FS 计算机的 Web front\ 端应用程序或服务。 有关如何自定义与身份委派 Web 应用程序的详细信息，请参阅[Windows 身份基础 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)

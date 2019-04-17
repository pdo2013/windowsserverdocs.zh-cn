---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: "部署联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>部署联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

部署中 Active Directory 联合身份验证服务 \(AD FS\) 联盟服务器，完成每个在任务[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)。  
  
> [!NOTE]  
> 当你使用此清单时，我们建议你先阅读联合身份验证的服务器计划中的参考[广告 FS 设计指南 Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx)之前开始配置服务器的过程。 按照以这种方式清单提供联合身份验证的服务器的设计和部署过程更好地了解。  
  
## <a name="about-federation-servers"></a>有关联盟服务器  
联合身份验证的服务器是安装了广告 FS 软件与运行 Windows Server 2008 已配置采取行动联合身份验证的服务器角色中的计算机。 联合身份验证的服务器身份验证或路线从其他公司中的用户帐户和可以任意位置 Internet 的客户端计算机的请求。  
  
在计算机上安装广告 FS 软件和使用广告 FS 联盟服务器配置向导对其进行配置为服务器联盟角色使该计算机的联合身份验证的服务器。 它还会广告 FS 管理使 snap\ 中适用于在该计算机**Start\\Administrative Tools\\**菜单上，以便你可以指定以下：  
  
-   合作伙伴公司和应用程序将在发送令牌要求和响应广告 FS 主机名称  
  
-   组织和应用程序合作的广告 FS 标识符将用于识别唯一的名称或你的组织的位置  
  
-   Token\ 签名证书服务器电场的日落中的所有联盟服务器会使用标记的问题并登录到  
  
-   自定义 ASP.NET 网页的客户端登录、注销和将增强客户端体验的帐户合作伙伴发现的位置  
  
    > [!NOTE]  
    > 以下 core 用户界面 \(UI\) 设置大多数包含在每个联合身份验证的服务器上。 Web.config 文件中并非指定广告 FS 主机名称和广告 FS 标识符值。  
  
联合服务器承载颁发令牌基于凭据索赔颁发引擎 \（适用于示例、用户名和 password\），将提供给它。 安全令牌是签名加密数据单元这表示一个或多个索赔。 声明的是服务器生成声明 \（例如，名称、的身份、密钥、组、特权或 capability\）客户端有关。 联合身份验证的服务器上验证都凭据后 \（通过用户登录 process\) 用户提起的索赔收集通过检查存储在应用商店中指定的特性用户属性。  
  
在联合 Web Single\-Sign\-On \(SSO\) 设计 \（在这两个或多个的组织有 involved\ 广告 FS 设计）的特定信赖方索赔规则可以修改索赔。 索赔内置了到中资源合作伙伴公司联合身份验证的服务器发送一个标记。 在资源合作伙伴联盟服务器接收为传入的索赔索赔后，它执行索赔颁发引擎运行一套索赔规则筛选、通过，或将这些索赔。 索赔然后内置了对在资源合作伙伴的 Web 服务器发送一个新标记。  
  
在 Web SSO 设计 \（只有一个组织处于 involved\ 广告 FS 设计），以便可以在上一次登录和多个应用仍然可以访问员工可以使用单个联盟服务器。  
  

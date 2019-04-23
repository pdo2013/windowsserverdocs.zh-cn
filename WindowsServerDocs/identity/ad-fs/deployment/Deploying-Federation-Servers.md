---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: 部署联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847168"
---
# <a name="deploying-federation-servers"></a>部署联合服务器

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

若要部署 Active Directory 联合身份验证服务中的联合身份验证服务器\(AD FS\)，完成每个中的任务[核对清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)。  
  
> [!NOTE]  
> 当使用此清单时，我们建议您先阅读对联合身份验证服务器规划中的引用[Windows Server 2012 中 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)开始配置服务器的过程之前。 以下清单中这种方式提供更好地了解联合身份验证服务器的设计和部署过程。  
  
## <a name="about-federation-servers"></a>有关联合身份验证服务器  
联合身份验证服务器是安装的 AD FS 软件与运行 Windows Server 2008 的已配置为充当联合身份验证服务器角色的计算机。 联合身份验证服务器进行身份验证，或从其他组织中的用户帐户和可以位于任意位置在 Internet 的客户端计算机的请求路由。  
  
在计算机上安装 AD FS 软件，并且使用 AD FS 联合身份验证服务器配置向导以将其配置为联合身份验证服务器角色的行为使该计算机的联合身份验证服务器。 它还使 AD FS 管理管理单元\-中在该计算机上可用**启动\\管理工具\\**菜单，以便你可以指定以下：  
  
-   AD FS 托管合作伙伴组织和应用程序将在其中发送令牌请求和响应的名称  
  
-   合作伙伴组织和应用程序的 AD FS 标识符将用于标识唯一的名称或你的组织的位置  
  
-   令牌\-签名证书的服务器场中的所有联合身份验证服务器将使用到问题并登录令牌  
  
-   有关客户端登录、 注销和帐户伙伴发现，将增强客户端体验的自定义 ASP.NET Web 页面的位置  
  
    > [!NOTE]  
    > 这些大部分核心用户界面\(UI\)设置包含在每台联合服务器上的 web.config 文件。 未在 web.config 文件中指定的 AD FS 主机名和 AD FS 标识符值。  
  
联合服务器承载颁发令牌基于凭据的声明颁发引擎\(例如，用户名和密码\)向其显示。 安全令牌是一个加密签名的数据单元，表示一个或多个声明。 声明是服务器生成的语句\(，名称、 标识、 密钥、 组、 权限或功能\)客户端有关的。 联合身份验证服务器上验证凭据后\(通过在用户登录过程\)，该用户的声明收集通过检查指定的属性存储中存储的用户属性。  
  
在联合 Web 单一\-符号\-上\(SSO\)设计\(AD FS 设计中所涉及两个或多个组织\)，可以通过特定信赖的声明规则修改声明参与方。 声明已内置到发送给资源伙伴组织中的联合身份验证服务器的令牌。 资源伙伴中的联合服务器收到的声明与传入声明后，它将执行声明发出引擎来运行一组声明规则，以便筛选、 传递，或转换这些声明。 然后，声明已内置到发送给资源伙伴中的 Web 服务器的新令牌。  
  
在 Web SSO 设计\(都只有一个组织的 AD FS 设计\)，可以使用单个联合身份验证服务器，以便员工可以登录一次并仍访问多个应用程序。  
  

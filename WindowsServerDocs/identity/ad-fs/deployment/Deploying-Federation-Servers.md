---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: 部署联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 785dcad4ac8e03cc59730fb533e30a001569dd63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359629"
---
# <a name="deploying-federation-servers"></a>部署联合服务器

若要在 Active Directory 联合身份验证服务 \(AD FS @ no__t-1 中部署联合服务器，请完成 [Checklist：设置联合服务器 @ no__t。  
  
> [!NOTE]  
> 使用此清单时，建议您先阅读[Windows server 2012 的 "AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)" 中的联合服务器规划的引用，然后再开始配置服务器的过程。 按照此方法检查清单，可以更好地了解联合服务器的设计和部署过程。  
  
## <a name="about-federation-servers"></a>关于联合服务器  
联合服务器是运行 Windows Server 2008 的计算机，其中安装的 AD FS 软件已配置为充当联合服务器角色。 联合服务器对来自其他组织中用户帐户的请求进行身份验证或路由，以及从 Internet 上的任何位置访问的客户端计算机。  
  
在计算机上安装 AD FS 软件并使用 AD FS 联合服务器配置向导为联合服务器角色配置该软件的操作使该计算机成为联合服务器。 它还会在 "**启动 @ no__t-2Administrative Tools @ no__t** " 菜单中使该计算机上的 AD FS 管理 snap @ no__t 0in 可用，以便您可以指定以下内容：  
  
-   合作伙伴组织和应用程序将在其中发送令牌请求和响应的 AD FS 主机名  
  
-   合作伙伴组织和应用程序将用于标识组织的唯一名称或位置的 AD FS 标识符  
  
-   服务器场中的所有联合服务器将用于颁发和签名令牌的 no__t-0signing 证书  
  
-   自定义的 ASP.NET 网页的位置，用于客户端登录、注销和将提高客户端体验的帐户伙伴发现  
  
    > [!NOTE]  
    > 其中的大多数核心用户界面 \(UI @ no__t 设置都包含在每个联合服务器上的 web.config 文件中。 Web.config 文件中未指定 AD FS 的主机名和 AD FS 标识符值。  
  
联合服务器托管声明颁发引擎，该引擎根据凭据 \(for 示例、用户名和密码 @ no__t （提供给它）颁发令牌。 安全令牌是一个表示一个或多个声明的经过加密签名的数据单元。 声明是服务器对客户端进行 @no__t 0for 示例、名称、标识、密钥、组、权限或功能 @ no__t 的一种声明。 在联合服务器上验证凭据之后 @no__t 0through 用户登录过程 @ no__t-1，将通过检查存储在指定属性存储中的用户属性来收集用户的声明。  
  
在联合 Web 单比 @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 设计 \(AD FS 设计中，其中有两个或多个组织参与了 @ no__t-5，声明可由特定信赖方的声明规则修改。 声明内置于发送到资源伙伴组织中的联合服务器的令牌中。 资源伙伴中的联合服务器接收声明作为传入声明后，它将执行声明发出引擎，以运行一组声明规则来筛选、传递或转换这些声明。 然后将声明内置于发送到资源伙伴中的 Web 服务器的新令牌中。  
  
在 Web SSO 设计中 \(an AD FS 设计（其中只有一个组织涉及到 @ no__t），可以使用单个联合服务器，以便员工可以登录一次并仍可访问多个应用程序。  
  

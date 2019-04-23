---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: 配置伙伴组织
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875178"
---
# <a name="configuring-partner-organizations"></a>配置伙伴组织

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

若要部署 Active Directory 联合身份验证服务中新的合作伙伴组织\(AD FS\)，完成中的任务[核对清单：配置资源伙伴组织](Checklist--Configuring-the-Resource-Partner-Organization.md)或[核对清单：配置帐户伙伴组织](Checklist--Configuring-the-Account-Partner-Organization.md)，取决于你的 AD FS 设计。  
  
> [!NOTE]  
> 当你使用其中某个清单时，我们强烈建议您先阅读对帐户伙伴或资源伙伴规划指南中的引用[Windows Server 2012 中 AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)然后继续执行用于设置新的合作伙伴组织的过程。 按照这种方式中的检查表将帮助提供更好地了解在帐户伙伴或资源伙伴组织完整 AD FS 设计和部署案例。  
  
## <a name="about-account-partner-organizations"></a>有关帐户伙伴组织  
帐户伙伴是联合身份验证信任关系的 AD FS – 支持属性存储中以物理方式存储用户帐户中的组织。 帐户伙伴负责收集和进行身份验证用户的凭据、 累积该用户的声明和将声明封装到安全令牌。 然后可通过联合身份验证信任，以便启用对 Web 提供这些令牌\-基于位于资源伙伴组织中的资源。  
  
换而言之，帐户伙伴代表其用户的组织帐户\-端联合身份验证服务器会颁发安全令牌。 帐户伙伴组织中的联合身份验证服务器进行身份验证本地用户，并在做出授权决定创建资源合作伙伴所使用的安全令牌。  
  
有关属性存储在 AD FS 帐户伙伴是概念上等同于单个 Active Directory 林的帐户需要在物理上位于另一个林中的资源的访问权限。 仅当外部信任或林信任两个林之间存在关系，并向其用户尝试访问的资源设置了适当的授权时，在此林中的帐户可以访问资源林中的资源权限。  
  
## <a name="about-resource-partner-organizations"></a>有关资源伙伴组织  
资源伙伴是组织中的 AD FS 部署 Web 服务器的位置。 资源伙伴信任帐户伙伴用户进行身份验证。 因此，要做出授权决策，资源伙伴将使用来自帐户伙伴中用户的安全令牌中打包的声明。  
  
换而言之，资源伙伴代表其 Web 服务器保护的资源的组织\-端联合身份验证服务器。 在资源伙伴联合身份验证服务器使用由帐户伙伴进行的 Web 服务器的授权决策资源伙伴中的安全令牌。  
  
若要充当 AD FS 资源，资源伙伴组织中的 Web 服务器必须具有 Windows Identity Foundation \(WIF\)安装或具有 Active Directory 联合身份验证服务\(AD FS\) 1.x声明\-感知 Web 代理角色服务安装。 Web 服务器的 AD FS 资源可以承载任一 Web 发挥\-浏览器\-基于或 Web\-服务\-基于应用程序。  

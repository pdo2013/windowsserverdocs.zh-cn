---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: 配置合作伙伴组织
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 575d7e3fc97496c3f7c147220fe342add66517c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408394"
---
# <a name="configuring-partner-organizations"></a>配置合作伙伴组织

若要在 Active Directory 联合身份验证服务 \(AD FS @ no__t-1 中部署新的合作伙伴组织，请完成以下任一项 [Checklist：配置资源伙伴组织 @ no__t-0 或 [Checklist：配置帐户伙伴组织 @ no__t-0，具体取决于 AD FS 的设计。  
  
> [!NOTE]  
> 使用上述任一清单时，我们强烈建议您先阅读[Windows Server 2012 的 "AD FS 设计指南](https://technet.microsoft.com/library/dd807036.aspx)" 中的帐户伙伴或资源伙伴计划指南，然后再继续执行设置新的合作伙伴组织。 以这种方式遵循清单将有助于更好地了解帐户伙伴或资源伙伴组织的完整 AD FS 设计和部署案例。  
  
## <a name="about-account-partner-organizations"></a>关于帐户伙伴组织  
帐户伙伴是联合身份验证信任关系中的组织，以物理方式将用户帐户存储在 AD FS 支持的属性存储中。 帐户伙伴负责收集和验证用户的凭据、生成该用户的声明，以及将声明打包为安全令牌。 然后，可以通过联合身份验证信任提供这些令牌，以允许访问位于资源伙伴组织中的 Web @ no__t-0based 资源。  
  
换句话说，帐户伙伴代表其用户帐户 @ no__t-0side 联合服务器颁发安全令牌的组织。 帐户伙伴组织中的联合服务器对本地用户进行身份验证，并创建资源伙伴在做出授权决定时使用的安全令牌。  
  
对于属性存储，AD FS 中的帐户伙伴在概念上等效于单个 Active Directory 林，其帐户需要访问物理上位于另一个林中的资源。 只有当两个林之间存在外部信任或林信任关系并且用户尝试访问的资源已设置适当的授权时，此林中的帐户才能访问资源林中的资源访问.  
  
## <a name="about-resource-partner-organizations"></a>关于资源伙伴组织  
资源伙伴是 Web 服务器所在 AD FS 部署中的组织。 资源伙伴信任帐户伙伴来对用户进行身份验证。 因此，若要做出授权决策，资源伙伴需使用由帐户伙伴中的用户提供的安全令牌中的声明。  
  
换句话说，资源伙伴代表其 Web 服务器受资源 @ no__t-0side 联合服务器保护的组织。 资源伙伴中的联合服务器使用由帐户伙伴生成的安全令牌为资源伙伴中的 Web 服务器做出授权决策。  
  
若要作为 AD FS 资源运行，资源伙伴组织中的 Web 服务器必须安装 Windows Identity Foundation \(WIF @ no__t，或具有 Active Directory 联合身份验证服务 @no__t 2AD FS @ no__t-4Aware Web已安装代理角色服务。 作为 AD FS 资源的 web 服务器可以托管 Web @ no__t-0browser @ no__t-1based 或 Web @ no__t-2service @ no__t 应用程序。  

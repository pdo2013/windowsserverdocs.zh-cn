---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: "特性官方商城的作用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
 >适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>特性官方商城的作用
Active Directory 联合身份验证服务使用术语"应用商店属性"指目录或组织用于保存它的用户帐户和及其关联的特性值的数据库。 广告金融服务身份提供商组织中配置后，从应用商店中检索这些属性值，和创建索赔，根据该信息，以便的 Web 应用程序或在信赖方组织托管的服务可以使适当的授权决策，只要联盟用户 \（其帐户中的身份提供商 organization\ 存储的用户）尝试访问的应用程序或服务。  
  
有关索赔如何生成的详细信息，请参阅[索赔角色](The-Role-of-Claims.md)。  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>如何特性存储适应您的广告 FS 部署目标  
用户特性官方商城的位置和位置用户身份验证确定设计广告 FS 支持的用户身份的方式。 根据特性官方商城的位置，以及用户访问的应用程序 \ (intranet 或 Internet 上 \)，你可以使用下面的部署目标之一：  
  
-   [你的 Active Directory 用户提供访问您的索赔识别应用程序和服务](https://technet.microsoft.com/library/dd807071.aspx)— 这一目标，在你的组织中的用户访问广告保护 FS-应用程序或服务 \（自己应用程序或服务或合作伙伴的应用程序或 service\）时的用户登录到 Active Directory 在公司的 intranet。  
  
-   [提供您 Active Directory 用户的访问权限的应用程序和服务的其他机构](https://technet.microsoft.com/library/dd807123.aspx)— 这一目标，在你的组织中的用户访问广告保护 FS-应用程序或服务 \（自己应用程序或服务或合作伙伴的应用程序或 service\）时的用户登录到公司的 intranet 和在登录时远程 Internet 从属性应用商店。  
  
-   [提供给你的索赔识别应用程序和服务中的用户另一个组织访问](https://technet.microsoft.com/library/dd807099.aspx)，在这一目标，在另一个组织都位于上组织的公司的 intranet 属性应用商店中的用户帐户必须访问广告 FS – 保护你的组织中的应用程序。 位于你的组织外围网络属性官方商城的基于 consumer\ 的用户帐户必须具有访问提供给广告 FS – 保护你的组织中的应用程序时，这一目标也适用。  
  
根据我们特性官方商城位置和您的组织的其他要求，可以结合这些部署目标，以完成广告 FS 部署设计的几个。  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>特性受广告 FS 的官方商城  
广告 FS 支持了种类丰富的目录，并数据库存储可用于解压缩 administrator\ 定义特性值和填充与这些值的索赔。 广告 FS 支持特性官方商城的以下目录或数据库任何：  
  
-   Windows Server 2003，在 Windows Server 2008、广告 DS 在 Windows Server 2012 和 2012 R2 和 Windows Server 2016 的 Active Directory 域服务 \(AD DS\) Active Directory。 
  
-   2005 的 Microsoft SQL Server、SQL Server 2008、SQL Server 2012、SQL Server 2014 年和 SQL Server 2016 的所有版本  
  
-   自定义特性官方商城  
  


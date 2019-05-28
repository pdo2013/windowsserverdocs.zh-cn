---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: 属性存储的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bec3ebf1bd12b260dbbb245a6a905277ff0d749f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188538"
---
# <a name="the-role-of-attribute-stores"></a>属性存储的角色
Active Directory 联合身份验证服务使用术语"特性存储"来指代的目录或组织用于存储其用户帐户及其关联的属性值的数据库。 AD FS 配置的标识提供程序组织中后，从存储中检索这些属性值和创建基于该信息，以便 Web 应用程序或信赖方组织中托管的服务可以进行适当的声明只要联合用户，授权决策\(其帐户存储在标识提供程序组织中的用户\)尝试访问应用程序或服务。  
  
有关如何生成声明的详细信息，请参阅 [The Role of Claims](The-Role-of-Claims.md)。  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>属性存储如何符合 AD FS 部署目标  
用户属性存储的位置和用户身份验证的位置决定了如何设计 AD FS 以支持用户标识。 具体取决于属性存储所在的位置和用户访问应用程序\(在 intranet 或 Internet 上\)，可以使用以下部署目标之一：  
  
-   [为您声明感知应用程序和服务提供您 Active Directory 用户访问](https://technet.microsoft.com/library/dd807071.aspx)— 在这一目标，你的组织中的用户访问的 AD FS 保护的应用程序或服务\(您自己的应用程序或服务或合作伙伴的应用程序或服务\)当用户登录到 Active Directory 企业 intranet 中。  
  
-   [向应用程序和服务的其他组织提供您 Active Directory 用户访问](https://technet.microsoft.com/library/dd807123.aspx)— 在这一目标，你的组织中的用户访问的 AD FS 保护的应用程序或服务\(您自己的应用程序或服务或合作伙伴的应用程序或服务\)当用户登录到企业 intranet 中和从 Internet 远程登录的属性存储。  
  
-   [为您声明感知应用程序和服务提供另一个组织 Access 中的用户](https://technet.microsoft.com/library/dd807099.aspx)— 在这一目标，必须访问另一组织中的用户帐户，该组织企业 intranet 上属性存储中的 AD FS –在你的组织中的受保护应用程序。 此目标也适用时使用者\-位于组织的外围网络中的属性存储的基于的用户帐户必须具有访问权限提供给 AD FS 保护你的组织中的应用程序。  
  
根据属性存储放置和组织的其他要求，你可以组合多个这些部署目标，以完成 AD FS 部署的设计。  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>AD FS 支持的属性存储  
AD FS 支持范围广泛的目录和数据库存储，你可将用于提取管理员\-定义属性值并使用这些值填充声明。 AD FS 支持任何以下目录或数据库作为属性存储：  
  
-   Windows Server 2003 Active Directory 域服务中的 active Directory \(AD DS\) Windows Server 2012 和 2012 R2 中的 Windows Server 2008 中，AD DS 和 Windows Server 2016 中。 
  
-   所有版本的 Microsoft SQL Server 2005、 SQL Server 2008、 SQL Server 2012、 SQL Server 2014 和 SQL Server 2016  
  
-   自定义属性存储  
  


---
title: Azure AD 域服务和远程桌面服务
description: 了解如何将 Azure AD 域服务集成到 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: 511aee3ea5be7e5c70c75cbc4d1febe1ec58dd97
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871066"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>将 Azure AD 域服务与 RDS 部署集成

可以在远程桌面服务部署中使用 [Azure AD 域服务](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) 代替 Windows Server Active Directory。 Azure AD DS 允许你将现有 Azure AD 标识与经典 Windows 工作负载一起使用。

与 Azure AD DS 一起使用时可以： 
- 为云中生成的组织创建一个具有本地域的 Azure 环境。 
- 使用用于本地和在线环境的同一标识，创建一个隔离的 Azure 环境，无需创建站点到站点 VPN 或 ExpressRoute。 

将 Azure AD DS 集成到你的远程桌面部署后，你的体系结构将如下所示：

![显示使用 Azure AD DS 的 RDS 体系结构图示](media/aadds-rds.png)

要了解此体系结构与其他 RDS 部署方案的比较，请查看[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)。

若要更好地了解 Azure AD DS，请查看 [Azure AD DS 概述](/azure/active-directory-domain-services/active-directory-ds-overview)和[如何确定 Azure AD DS 是否适合你的用例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用以下信息通过 RDS 部署 Azure AD DS。

## <a name="prerequisites"></a>必备条件

在将 Azure AD 中的标识用于 RDS 部署之前，请[配置 Azure AD 以保存用户标识的哈希密码](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)。 云中生成的组织不需要在其目录中进行任何其他更改，但是，本地组织需要允许密码哈希在 Azure AD 中同步并存储，对于某些组织而言，这可能是不允许的。 进行此配置更改后，用户必须重置密码。

## <a name="deploy-azure-ad-ds-and-rds"></a>部署 Azure AD DS 和 RDS 
使用以下步骤来部署 Azure AD DS 和 RDS。

1. 启用 [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 请注意，链接的文章将：
   - 引导用户创建合适的 Azure AD 组以管理域。
   - 在可能需要强制用户更改其密码，以便其帐户能够使用 Azure AD DS 时突出显示。
   
2. 设置 RDS。 你可以使用 Azure 模板，也可以手动部署 RDS。
   - 使用[现有 AD 模板](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 确保自定义以下内容：
   
     - **设置**
       - **资源组**：使用要在其中创建 RDS 资源的资源组。
         > [!NOTE] 
         > 现在，这必须是 Azure 资源管理器虚拟网络所在的同一资源组。

       - **Dns 标签前缀**：输入你希望用户用于访问 RD Web 的 URL。
       - **Ad 域名**：输入 Azure AD 实例的全名，例如“contoso.onmicrosoft.com”或“contoso.com”。
       - **Ad Vnet 名称**和 **Ad 子网名称**：输入在创建 Azure 资源管理器虚拟网络时使用的相同值。 这是 RDS 资源将连接到的子网。
       - **管理员用户名**和**管理员密码**：输入管理员用户的凭据，该用户是 Azure AD 中 AAD DC 管理员  组的成员。
   
     - **模板**
        - 删除 dnsServers  的所有属性：从 Azure 快速入门模板页面中选择“编辑模板”  后，搜索“dnsServers”并删除该属性。 

           例如，在删除 dnsServers  属性之前：
      
           ![具有 dnsSettings 属性的 Azure 快速入门模板](media/rds-remove-dnssettings-before.png)

           下面是删除属性之后的同一文件：

           ![删除了 dnsSettings 属性的 Azure 快速入门模板](media/rds-remove-dnssettings-after.png)
   
   - [手动部署 RDS](rds-deploy-infrastructure.md)。 


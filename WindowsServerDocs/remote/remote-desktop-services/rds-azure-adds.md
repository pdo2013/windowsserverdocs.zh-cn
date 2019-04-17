---
title: Azure AD 域服务和远程桌面服务
description: 了解如何集成 RDS 部署 Azure AD 域服务。
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
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614511"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>RDS 部署集成 Azure AD 域服务

Windows Server Active Directory 代替远程桌面服务部署中，可以使用[Azure AD 域服务](/azure/active-directory-domain-services/active-directory-ds-overview)(Azure AD DS)。 Azure AD DS 允许您使用您现有的 Azure AD 标识使用经典 Windows 工作负荷。

与 Azure AD DS 中，您可以： 
- 使用本地域出生--云中组织创建 Azure 环境。 
- 使用相同的标识用于您的本地和联机环境，而无需创建站点到站点 VPN 或 ExpressRoute 创建独立的 Azure 环境。 

完成将 Azure AD DS 集成到远程桌面部署后，您的体系结构将如下所示：

![显示与 Azure AD DS 的 RDS 体系结构图示](media/aadds-rds.png)

若要查看此体系结构与其他 RDS 部署方案之间的比较，请签出[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)。

若要获得更好地理解 Azure AD ds，签出[Azure AD DS 概述](/azure/active-directory-domain-services/active-directory-ds-overview)以及[如何决定 Azure AD DS 是否适合您的使用案例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用以下信息将与 rds.Azure AD DS 部署

## <a name="prerequisites"></a>先决条件

之前可以从 RDS 部署，[配置 Azure AD 以保存您的用户标识的哈希的密码](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)中使用的 Azure AD 将您的标识。 出生--云中组织不需要进行任何其他更改，在其目录;但是，内部部署组织需要允许密码哈希同步并存储在 Azure AD，这可能不是允许对某些组织中。 用户将必须进行此配置更改后重置其密码。

## <a name="deploy-azure-ad-ds-and-rds"></a>部署 Azure AD DS 和 RDS 
使用以下步骤来部署 Azure AD DS 和 rds.

1. 启用[Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 注意链接的文章，下列操作：
   - 逐步创建相应的域管理的 Azure AD 组。
   - 您可能需要强制用户更改其密码，因此 Azure AD DS 可以处理其帐户时，请突出显示。
   
2. 设置 rds. 您可以使用 Azure 模板，或手动部署 RDS。
   - 使用[现有 AD 模板](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 请确保以下自定义：
   
      - **设置**
         - **资源组**： 使用资源组想要创建的 RDS 资源。
         > [!NOTE] 
         > 现在，这必须是相同的资源组所在的 Azure 资源管理器的虚拟网络。

         - **Dns 标签前缀**： 输入 URL 您希望用户用于访问 RD Web。
         - **Ad 域名称**： 输入 Azure AD 实例，例如，"contoso.onmicrosoft.com"或"contoso.com"的完整名称。
         - **Ad Vnet 名称**和**Ad 子网名称**： 输入创建 Azure 资源管理器的虚拟网络时使用的相同值。 这是 RDS 资源将连接到的子网。
         - **管理员用户名**和**密码管理**： Azure AD 中是**AAD DC Administrators**组的成员的管理员用户输入的凭据。
   
      - **模板**
         - 删除所有属性的**那么**： 后从 Azure 快速入门模板页中，选择**编辑模板**，"那么"的搜索和都删除属性。 

            例如，之前删除**那么**属性：
      
            ![Azure 快速入门模板使用 dnsSettings 属性](media/rds-remove-dnssettings-before.png)

            而以下后删除属性是相同的文件：

            ![使用 dnsSettings 属性删除 azure 快速入门模板](media/rds-remove-dnssettings-after.png)
   
   - [部署 RDS 手动](rds-deploy-infrastructure.md)。 


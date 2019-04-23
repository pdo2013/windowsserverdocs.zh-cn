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
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860508"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>将 Azure AD 域服务与 RDS 部署集成

可以使用[Azure AD 域服务](/azure/active-directory-domain-services/active-directory-ds-overview)(Azure AD DS) 在远程桌面服务部署到 Windows Server Active Directory 位置中。 Azure AD DS，可以使用现有 Azure AD 标识使用经典 Windows 工作负荷。

与 Azure AD DS，你可以： 
- 使用生成---云中的组织的本地域中创建的 Azure 环境。 
- 使用用于本地和联机环境中，而无需创建站点到站点 VPN 或 ExpressRoute 的同一标识创建一个独立的 Azure 环境。 

完成将 Azure AD DS 集成到你的远程桌面部署后，您的体系结构将如下所示：

![显示与 Azure AD DS 的 RDS 体系结构图示](media/aadds-rds.png)

若要查看其他 RDS 部署方案时，此体系结构的进行比较，请查看[远程桌面服务体系结构](desktop-hosting-logical-architecture.md)。

若要获取更好地了解 Azure AD DS，请查看[Azure AD DS 概述](/azure/active-directory-domain-services/active-directory-ds-overview)并[如何确定 Azure AD DS 是否适合你的用例](/azure/active-directory-domain-services/active-directory-ds-comparison)。

使用以下信息来部署 Azure AD DS 与 rds。

## <a name="prerequisites"></a>先决条件

你可以从 Azure AD 即可使用在 RDS 部署中，将您标识之前[配置 Azure AD，以保存用户的标识的哈希的密码](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync)。 生成---云中的组织不需要在其目录; 中进行任何其他更改但是，在本地组织需要允许密码哈希同步并存储在 Azure AD 中，这可能不是允许对某些组织。 用户将必须进行此配置更改后重置其密码。

## <a name="deploy-azure-ad-ds-and-rds"></a>将 Azure AD DS 和 RDS 部署 
使用以下步骤来部署 Azure AD DS 和 rds。

1. 启用[Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started)。 请注意链接的文章，以下任务：
   - 逐步创建相应域管理的 Azure AD 组。
   - 突出显示时可能需要强制用户更改其密码，以便其帐户能够使用 Azure AD DS。
   
2. 设置 rds。 可以使用 Azure 模板，也可以手动部署 RDS。
   - 使用[现有 AD 模板](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/)。 请确保自定义以下内容：
   
      - **设置**
         - **资源组**:使用你想要创建的 RDS 资源的资源组。
         > [!NOTE] 
         > 现在，这必须是 Azure 资源管理器虚拟网络所在的同一资源组。

         - **Dns 标签前缀**:输入的 URL，你希望用户使用 RD Web 访问。
         - **Ad 域名**:输入 Azure AD 实例，例如，"contoso.onmicrosoft.com"或"contoso.com"的完整名称。
         - **Ad Vnet-name**并**Ad 子网名称**:输入在创建 Azure 资源管理器虚拟网络时使用的相同值。 这是 RDS 资源将连接到的子网。
         - **管理员用户名**并**管理员密码**:输入管理员用户的成员的凭据**AAD DC 管理员**Azure AD 中的组。
   
      - **模板**
         - 删除的所有属性**dnsServers**： 选择后**编辑模板**从 Azure 快速入门模板页面中，搜索"dnsServers"并都删除的属性。 

            例如，然后再删除**dnsServers**属性：
      
            ![Azure 快速入门模板与 dnsSettings 属性](media/rds-remove-dnssettings-before.png)

            和此处删除属性后是相同的文件：

            ![使用 dnsSettings 属性已被删除的 azure 快速入门模板](media/rds-remove-dnssettings-after.png)
   
   - [手动部署 RDS](rds-deploy-infrastructure.md)。 


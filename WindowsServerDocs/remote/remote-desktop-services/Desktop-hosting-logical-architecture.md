---
title: 远程桌面服务体系结构
description: RDS 体系结构关系图
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: 7cd46cadf5ed5424e50556ee0c91a80804108113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404214"
---
# <a name="remote-desktop-services-architecture"></a>远程桌面服务体系结构

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

下面是用于部署远程桌面服务来为最终用户托管 Windows 应用和桌面系统的各种配置。

>[!NOTE]
> 下面的体系结构关系图显示在 Azure 中使用 RDS。 但是，你可以在本地和其他云上部署远程桌面服务。 这些关系图主要用于演示如何协调 RDS 角色并使用其他服务。

## <a name="standard-rds-deployment-architectures"></a>标准 RDS 部署体系结构

远程桌面服务有两个标准体系结构：
-   基本部署 – 这包含创建完全有效的 RDS 环境所需的最小服务器数量
-   高度可用部署 – 这包含所有必要组件，以保证 RDS 环境具有最高的正常运行时间

### <a name="basic-deployment"></a>基本部署

![基本 RDS 部署](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>高度可用部署

![高度可用的 RDS 部署](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>具有唯一 Azure PaaS 角色的 RDS 体系结构

虽然标准 RDS 部署体系结构适合大多数情况，但 Azure 继续致力于推动实现客户价值的第一方 PaaS 解决方案。 下面是一些体系结构，展示了它们如何与 RDS 集成。

### <a name="rds-deployment-with-azure-ad-domain-services"></a>使用 Azure AD 域服务的 RDS 部署

上面的两个标准体系结构关系图基于部署在 Windows Server VM 上的传统 Active Directory (AD)。 但是，如果你没有传统 AD 并且只有一个 Azure AD 租户（通过 Office 365 等服务），但仍想要利用 RDS，则可以使用 [Azure AD 域服务](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)在使用 Azure AD 租户中存在的相同用户的 Azure IaaS 环境中创建完全托管的域。 这消除了手动同步用户和管理更多虚拟机的复杂性。 Azure AD 域服务可以在以下两个部署中工作：基本或高可用性。

![Azure AD 和 RDS 部署](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>使用 Azure AD 应用程序代理的 RDS 部署

上面的两个标准体系结构关系图使用 RD Web/网关服务器作为面向 Internet 的 RDS 系统的入口点。 对于某些环境，管理员更希望从外围网络中删除自己的服务器，而使用通过反向代理技术提供额外安全性的技术。 [Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) PaaS 角色可以很好地适合这种情况。

有关支持的配置以及如何创建此设置，请参阅如何[使用 Azure AD 应用程序代理发布远程桌面](/azure/active-directory/application-proxy-publish-remote-desktop)。

![使用 Azure AD 应用程序代理的 RDS](./media/aadappproxy-rds.png)

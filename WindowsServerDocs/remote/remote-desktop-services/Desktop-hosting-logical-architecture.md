---
title: 远程桌面服务体系结构
description: 有关 RDS 体系结构关系图
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887868"
---
# <a name="remote-desktop-services-architecture"></a>远程桌面服务体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

下面是用于部署远程桌面服务来托管 Windows 应用和桌面系统对最终用户的各种配置。

>[!NOTE]
> 下面的体系结构关系图显示在 Azure 中使用 RDS。 但是，您可以在其中部署远程桌面服务的本地和其他云。 这些关系图主要用于演示 RDS 角色位于相同位置并使用其他服务。

## <a name="standard-rds-deployment-architectures"></a>标准 RDS 部署体系结构

远程桌面服务具有两个标准体系结构：
-   基本部署 – 这包含的最小服务器以创建完全有效的 RDS 环境数
-   高度可用部署 – 这包含所有必要的组件具有的最高保证正常运行时间 RDS 环境

### <a name="basic-deployment"></a>基本部署

![基本 RDS 部署](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>高度可用的部署

![高度可用的 RDS 部署](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>使用唯一的 Azure PaaS 角色的 RDS 体系结构

尽管标准 RDS 部署体系结构适合大多数情况下，Azure 将继续在第一方 PaaS 解决方案方面投入的推动实现客户价值。 下面是显示如何使用 rds.将合并某些体系结构

### <a name="rds-deployment-with-azure-ad-domain-services"></a>使用 Azure AD 域服务的 RDS 部署

上面的两个标准体系结构关系图基于上传统 Active Directory (AD) 部署在 Windows Server VM 上。 但是，如果您没有传统 AD 并只能有一个 Azure AD 租户 — 通过 office 365 等服务，但仍想要利用 RDS，可以使用[Azure AD 域服务](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)若要在 Azure IaaS 中创建完全托管的域使用 Azure AD 租户中存在的相同用户的环境。 这会删除手动同步用户和管理更多虚拟机的复杂性。 Azure AD 域服务可以在两个部署中工作： 基本或高可用性。

![Azure AD 和 RDS 部署](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>使用 Azure AD 应用程序代理的 RDS 部署

上面的两个标准体系结构关系图使用 RD Web/网关服务器作为 RDS 系统的面向 Internet 的入口点。 对于某些环境，管理员希望从外围网络中删除其自己的服务器并改为使用还提供通过反向代理技术的附加安全性的技术。 [Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)PaaS 角色可以很好地适合这种情况。

有关支持的配置以及如何创建此安装程序，请参阅如何[使用 Azure AD 应用程序代理发布远程桌面](/azure/active-directory/application-proxy-publish-remote-desktop)。

![使用 Azure AD 应用程序代理的 RDS](./media/aadappproxy-rds.png)

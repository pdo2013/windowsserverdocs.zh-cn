---
title: 了解桌面托管环境
description: 使用 Azure IaaS RDS deployhment 的概述。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877918"
---
# <a name="understanding-the-desktop-hosting-environment"></a>了解桌面托管环境

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下信息介绍桌面托管服务的组件。  
  
## <a name="tenant-environment"></a>租户环境  
提供程序的桌面托管服务作为一组隔离的租户环境。 每个租户的环境包含存储容器、 一组虚拟机和所有通过隔离的虚拟网络进行通信的 Azure 服务组合。 每个虚拟机包含一个或多个组件构成租户的托管桌面环境。 以下小节介绍了构成每个租户的托管桌面环境的组件。

## <a name="remote-desktop-services"></a>远程桌面服务
在桌面托管环境，以下远程桌面服务角色安装在各种虚拟机之间：

  - 远程桌面连接代理
  - 远程桌面网关
  - 远程桌面授权
  - 远程桌面会话主机
  - 远程桌面 Web 访问

有关每个这些角色以及与彼此交互的方式的完整说明，请查看[了解 RDS 角色](Understanding-RDS-roles.md)文档。
  
##  <a name="azure-active-directory-domain-services"></a>(Azure)Active Directory 域服务  
有多种方法来连接到和为 Azure 中的桌面托管环境中管理 Active Directory 域服务 (AD DS):

1. 在运行 AD DS 角色的租户的环境中创建虚拟机
2. 使用租户的本地环境以使用现有的 AD DS 中创建站点到站点 VPN 连接
3. 使用 Azure AD 域服务 PaaS 角色，在基于租户的 Azure Active Directory 租户的虚拟网络创建域

使用远程桌面服务，租户必须具有 Active Directory 到环境中，用户配置文件存储管理的访问权限和监视部署中。 在使用标准 (非 Azure) AD DS 时，租户的林不需要与提供程序的管理林的任何信任关系。 域管理员帐户可能被设置租户的域中允许的提供程序的技术人员 （如监视系统状态和应用软件更新） 的租户的环境中执行管理任务并帮助解决与故障排除和配置。  
    
其他信息：  
[Azure Active Directory 域服务文档](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[在 Azure 虚拟网络中安装新的 Active Directory 林](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[创建具有站点到站点 VPN 连接使用 Azure 门户的资源管理器 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Azure SQL 数据库  
Azure SQL 数据库允许宿主来扩展其远程桌面服务部署，而无需部署和维护一个完整的 SQL Server Always-On 群集。 Azure SQL 数据库是远程桌面连接代理用于存储部署信息，例如当前用户的连接到最终主机服务器的映射。 类似于其他 Azure 服务，Azure SQL DB 遵循消耗模型，适用于任何规模的部署。   
  
其他信息：  
[什么是 SQL 数据库？](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 应用程序代理  
Azure Active Directory 应用程序代理是付费 Sku 的 Azure Active Directory，使用户能够连接到通过 Azure 的反向代理服务的内部应用程序中提供的服务。 这样，要隐藏在虚拟网络，无需向公共 IP 地址通过 internet 公开的 RD Web 和 RD 网关终结点。 这进一步使托管商仍保持完整部署时压缩在租户的环境中的虚拟机数。
  
其他信息：  
[启用 Azure AD 应用程序代理](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>文件服务器  
文件服务器通过使用服务器消息块 (SMB) 3.0 协议提供共享的文件夹。 若要创建和存储用户配置文件磁盘文件 (.vhdx) 到备份数据，并允许用户与租户的虚拟网络中的其他用户共享数据的位置使用的共享的文件夹。
  
使用部署文件服务器的 VM 必须具有一个 Azure 数据磁盘附加和配置共享文件夹。 Azure 数据磁盘使用直写式缓存的保证，将写入到磁盘保留在重启后的 VM。  
  
对于小租户，可以通过将与租户的环境中的单个虚拟机上运行的 RD 连接代理和 RD 许可角色的虚拟机结合使用文件服务器降低成本。  
  
其他信息  
[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)  
[如何将数据磁盘附加到虚拟机](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>用户配置文件磁盘  
用户配置文件磁盘允许用户将个人设置和文件保存时签名在集合中，RD 会话主机服务器上的会话，然后在登录到集合中的其他 RD 会话主机服务器时有权访问相同的设置和文件。 当用户首次登录时，在租户的文件服务器上创建用户配置文件磁盘和该磁盘装载到该用户连接到 RD 会话主机服务器。 为每个后续登录中，用户配置文件磁盘装载到相应的 RD 会话主机服务器，并与每个注销，它是未装入。 只能由该用户访问配置文件磁盘的内容。  
  



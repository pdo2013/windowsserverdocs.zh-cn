---
title: 了解桌面托管环境
description: 使用 Azure IaaS 进行 RDS 部署的概述。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 1bd672c52c892430339bb6c17c6324bf4d6d79a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387808"
---
# <a name="understanding-the-desktop-hosting-environment"></a>了解桌面托管环境

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

以下信息介绍了桌面托管服务组件。  
  
## <a name="tenant-environment"></a>租户环境  
提供商的桌面托管服务实现为一组隔离的租户环境。 每个租户的环境包括一个存储容器、一组虚拟机以及 Azure 服务的组合，所有这些组件通过隔离的虚拟网络进行通信。 每个虚拟机包含一个或多个构成了租户托管桌面环境的组件。 以下小节将介绍构成每个租户的托管桌面环境的组件。

## <a name="remote-desktop-services"></a>远程桌面服务
在桌面托管环境中，在各种虚拟机中安装了以下远程桌面服务角色：

  - 远程桌面连接代理
  - 远程桌面网关
  - 远程桌面授权
  - 远程桌面会话主机
  - 远程桌面 Web 访问

有关每个角色以及与彼此交互的方式的完整说明，请查看[了解 RDS 角色](Understanding-RDS-roles.md)文档。
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory 域服务  
有多种方法可以连接到 Azure 桌面托管环境的 Active Directory 域服务 (AD DS) 并对其进行管理：

1. 在运行 AD DS 角色的租户环境中创建虚拟机
2. 使用租户的本地环境创建站点到站点 VPN 连接以使用现有 AD DS
3. 使用 Azure AD 域服务 PaaS 角色，它基于租户的 Azure Active Directory 在租户的虚拟网络上创建域

使用远程桌面服务，租户必须具有 Active Directory 以管理对环境、用户配置文件存储和部署中监视的访问。 在使用标准（非 Azure）AD DS 时，租户的林不需要与提供商的管理林之间存在任何信任关系。 可以在租户的域中设置域管理员帐户，使提供商的技术人员能够在租户的环境中执行管理任务（例如监视系统状态和应用软件更新），并帮助进行故障排除和配置。  
    
其他信息：  
[Azure Active Directory 域服务文档](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[在 Azure 虚拟网络中安装新的 Active Directory 林](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[使用 Azure 门户创建具有站点到站点 VPN 连接的资源管理器 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Azure SQL 数据库  
Azure SQL 数据库允许主机托管服务提供商扩展其远程桌面服务部署，而无需部署和维护一个完整的 SQL Server Always-On 群集。 远程桌面连接代理使用 Azure SQL 数据库来存储部署信息，例如当前用户连接到最终主机服务器的映射。 类似于其他 Azure 服务，Azure SQL DB 遵循消耗模型，该模型适用于任何规模的部署。   
  
其他信息：  
[什么是 SQL 数据库？](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 应用程序代理  
Azure Active Directory 应用程序代理是 Azure Active Directory 的付费 SKU 中提供的一项服务，这些 SKU 使用户能够通过 Azure 自身的反向代理服务连接到内部应用程序。 这样，RD Web 和 RD 网关终结点可以隐藏在虚拟网络内，而无需通过公共 IP 地址向 Internet 公开。 这进一步允许主机托管服务提供商减少租户环境中的虚拟机数，同时仍保持完整部署。
  
其他信息：  
[启用 Azure AD 应用程序代理](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>文件服务器  
文件服务器使用服务器消息块 (SMB) 3.0 协议提供共享文件夹。 使用这些共享文件夹可以创建和存储用户配置文件磁盘文件 (.vhdx) 来备份数据，并让用户在租户的虚拟网络中与其他用户共享数据。
  
部署文件服务器的 VM 必须附有一个 Azure 数据磁盘并配置有共享文件夹。 Azure 数据磁盘使用直写式缓存，这保证了对磁盘的写入操作能够在 VM 重启期间继续。  
  
对于小型租户，可以通过将文件服务器与运行 RD 连接代理的虚拟机以及租户环境中单个虚拟机上的 RD 授权角色相结合来降低成本。  
  
其他信息  
[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)  
[如何将数据磁盘附加到虚拟机](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>用户配置文件磁盘  
使用用户配置文件磁盘，用户在登录到一个集合中的 RD 会话主机服务器上的会话后可以保存个人设置和文件，然后在登录到该集合中另一个 RD 会话主机服务器时可访问相同的设置和文件。 当用户首次登录时，租户的文件服务器上会创建一个用户配置文件磁盘，该磁盘装载到用户连接到的 RD 会话主机服务器。 以后每次登录时，该用户配置文件磁盘将装载到相应的 RD 会话主机服务器，而每次注销时都会卸载。 只能由该用户访问配置文件磁盘的内容。  
  



---
title: 桌面托管服务
description: 介绍桌面托管服务的组件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 597016ecf10da0febb7ba1bb099da0c1dddf96ce
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749222"
---
# <a name="desktop-hosting-service"></a>桌面托管服务

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

本文详细介绍桌面托管服务的组件。

## <a name="tenant-environment"></a>租户环境

如[远程桌面服务角色](rds-roles.md)中所述，每个角色在租户环境中发挥着不同的作用。

提供商的桌面托管服务实现为一组隔离的租户环境。 每个租户的环境包括一个存储容器、一组虚拟机以及 Azure 服务的组合，所有这些组件通过隔离的虚拟网络进行通信。 每个虚拟机包含一个或多个构成了租户托管桌面环境的组件。 以下小节将介绍构成每个租户的托管桌面环境的组件。

## <a name="active-directory-domain-services"></a>Active Directory 域服务

Active Directory 域服务 (AD DS) 提供域和林信息，使租户的用户可以登录到桌面和应用程序来执行其工作负荷。 这样，你还可以设置或连接到 Windows 应用程序可能需要的文件共享和数据库。

租户的林不需要与提供商的管理林之间存在任何信任关系。 可以在租户的域中设置域管理员帐户，使提供商的技术人员能够在租户的环境中执行管理任务（例如监视系统状态和应用软件更新），并帮助进行故障排除和配置。

可通过多种方式部署 AD DS：

1. 在租户的虚拟网络环境中启用 Azure Active Directory 域服务。 这会根据 Azure AD 中存在的用户和组创建租户的托管 AD DS 实例。
2. 在租户的虚拟网络环境中设置独立的 AD DS 服务器。 这样就可以全面控制虚拟机上运行的 AD DS 实例。
3. 与租户场地的 AD DS 服务器建立站点到站点 VPN 连接。 这样，租户就可以连接到其现有的 AD DS 实例，并减少重复的用户、组、组织单位等。

有关详细信息，请参阅以下文章：

* [Azure Active Directory 域服务文档](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Windows Server 2012 R2 的桌面托管参考体系结构指南](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [在 Azure 门户中创建站点到站点连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>SQL 数据库

远程桌面连接代理使用高可用性 SQL 数据库来存储部署信息，例如当前用户连接到主机服务器的映射。

可通过多种方式部署 SQL 数据库：

1. 在租户的环境中创建 Azure SQL 数据库。 这可以获取冗余 SQL 数据库的功能，而无需管理服务器本身。 此外，你只需为实际使用的资源付费，而无需对基础结构投资。
2. 创建 SQL Server AlwaysOn 群集。 这样就可以利用现有的 SQL Server 基础结构，并全面控制 SQL Server 实例。

有关如何设置高可用性 SQL 数据库基础结构的详细信息，请参阅以下文章：

* [什么是 Azure SQL 数据库服务？](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [创建和配置可用性组 (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017)。
* [添加 RD 连接代理服务器以部署和配置高可用性](rds-connection-broker-cluster.md)。

## <a name="file-server"></a>文件服务器

文件服务器使用服务器消息块 (SMB) 3.0 协议提供共享文件夹。 使用这些共享文件夹可以创建和存储用户配置文件磁盘文件 (.vhdx) 来备份数据，并让用户在租户的云服务中相互共享数据。

部署文件服务器的虚拟机必须附有一个 Azure 数据磁盘并配置有共享文件夹。 Azure 数据磁盘使用直写式缓存，保证每次重启虚拟机后不会擦除写入到磁盘的内容。

小型租户可以通过将文件服务器和 [RD 授权角色](rds-roles.md#remote-desktop-licensing)合并到租户环境中的单个虚拟机来降低成本。

有关详细信息，请参阅以下文章：

* [Windows Server 中的存储](../../storage/storage.md)
* [如何在 Azure 门户中将托管的数据磁盘附加到 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>用户配置文件磁盘

使用用户配置文件磁盘，用户在登录到一个集合中的 RD 会话主机服务器上的会话后可以保存个人设置和文件，然后在登录到该集合中另一个 [RD 会话主机](rds-roles.md#remote-desktop-session-host)服务器时访问相同的设置和文件。 当用户首次登录时，租户的文件服务器会创建一个用户配置文件磁盘，该磁盘已装载到用户当前所连接到的 RD 会话主机服务器。 以后每次登录时，该用户配置文件磁盘将装载到相应的 RD 会话主机服务器，而每次注销时都会卸载。只有该用户能够访问该配置文件磁盘的内容。
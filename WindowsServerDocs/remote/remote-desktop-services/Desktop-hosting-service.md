---
title: 桌面托管服务
description: 介绍的桌面托管服务的组件。
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
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840468"
---
# <a name="desktop-hosting-service"></a>桌面托管服务

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本文将告诉您有关桌面托管服务的组件的详细信息。

## <a name="tenant-environment"></a>租户环境

如中所述[远程桌面服务角色](rds-roles.md)，每个角色所起的作用不同租户环境中。

提供程序的桌面托管服务作为一组隔离的租户环境。 每个租户的环境包含存储容器、 一组虚拟机和所有通过隔离的虚拟网络进行通信的 Azure 服务组合。 每个虚拟机包含一个或多个组件构成租户的托管桌面环境。 以下小节介绍了构成每个租户的托管桌面环境的组件。

## <a name="active-directory-domain-services"></a>Active Directory 域服务

Active Directory 域服务 (AD DS) 提供的域和林的信息，以便租户的用户可以登录到桌面和应用程序以执行其工作负荷。 这还可以设置或连接到所需的文件共享和所需的 Windows 应用程序的数据库。

租户的林不需要与提供程序的管理林的任何信任关系。 域管理员帐户可能被设置租户的域中允许的提供程序的技术人员 （如监视系统状态和应用软件更新） 的租户的环境中执行管理任务并帮助解决与故障排除和配置。

有多种方法来部署 AD DS:

1. 在租户的虚拟网络环境中启用 Azure Active Directory 域服务。 这将为基于用户和组存在于 Azure AD 中的租户创建托管的 AD DS 实例。
2. 设置租户的虚拟网络环境中的独立 AD DS 服务器。 这使你的虚拟机上运行的 AD DS 实例的完全控制所有。
3. 创建站点到站点 VPN 连接到位于在租户的本地 AD DS 服务器。 这样，要连接到其现有的 AD DS 实例，并减少重复的用户、 组、 组织单位和等等的租户。

有关详细信息，请参阅以下文章：

* [Azure Active Directory 域服务文档](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [桌面托管参考体系结构指南适用于 Windows Server 2012 R2](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [在 Azure 门户中创建站点到站点连接](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>SQL 数据库

高度可用的 SQL 数据库是远程桌面连接代理用于存储部署信息，例如当前用户的连接到主机服务器的映射。

有多种方式部署 SQL 数据库：

1. 租户的环境中创建 Azure SQL 数据库。 这可冗余的 SQL 数据库的功能而无需管理服务器本身。 这还允许你支付，而不是基础结构中投入使用。
2. 创建 SQL Server AlwaysOn 群集。 这让你利用现有的 SQL Server 基础结构，并提供对 SQL Server 实例的完全控制。

有关如何设置高度可用的 SQL 数据库基础结构的详细信息，请参阅以下文章：

* [什么是 Azure SQL 数据库服务？](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [创建和配置可用性组 (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017)。
* [将 RD 连接代理服务器添加到部署和配置高可用性](rds-connection-broker-cluster.md)。

## <a name="file-server"></a>文件服务器

文件服务器使用服务器消息块 (SMB) 3.0 协议提供共享的文件夹。 这些共享的文件夹用于创建和存储用户配置文件磁盘文件 (.vhdx) 来备份数据，并让用户在租户的云服务中与每个其他共享数据。

部署文件服务器虚拟机必须具有一个 Azure 数据磁盘附加和配置共享文件夹。 Azure 数据磁盘使用直写式缓存，保证只要重新启动虚拟机，将不清除写入到磁盘。

小型租户可以通过组合文件服务器来降低成本并[RD 许可角色](rds-roles.md#remote-desktop-licensing)租户的环境中的单个虚拟机上。

有关详细信息，请参阅以下文章：

* [Windows Server 中存储](../../storage/storage.md)
* [如何将托管的数据磁盘附加到在 Azure 门户中的 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>用户配置文件磁盘

用户配置文件磁盘允许用户将个人设置和文件保存时签名在一个集合中的 RD 会话主机服务器上的会话，然后访问相同的设置和文件时登录到不同[RD 会话主机](rds-roles.md#remote-desktop-session-host)集合中的服务器。 用户首次登录时，租户的文件服务器创建获取装载到用户当前连接到 RD 会话主机服务器的用户配置文件磁盘。 对于每个后续登录中，用户配置文件磁盘装载到相应的 RD 会话主机服务器，并会卸载与每个注销。只有用户可以访问配置文件磁盘的内容。
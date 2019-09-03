---
title: 迁移角色和功能
description: 介绍如何将角色和功能迁移到更新版的 Windows Server。
ms.prod: windows-server
ms.date: 08/28/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 33c1aa654e4c660b4fe2f3305bfaf78b5191220a
ms.sourcegitcommit: e58e1646ffd75d4a89576d967b2dbbbb84764303
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119203"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>在 Windows Server 中迁移角色和功能

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本页包含一些信息和工具的链接，这些信息和工具会引导你完成将角色和功能迁移到新版 Windows Server 的过程。 可以使用[存储迁移服务](../storage/storage-migration-service/overview.md)迁移文件服务器和存储，并且可以使用 Windows Server 迁移工具迁移许多其他的角色和功能；Windows Server 迁移工具是 Windows Server 2008 R2 中引入的一组 PowerShell cmdlet，可用于迁移角色和功能。

这些迁移指南支持将指定的角色和功能从一台服务器迁移到另一台服务器（非就地升级）。 除非这些指南中另行说明，否则支持在物理与虚拟计算机之间以及在 Windows Server 的完全安装选项与正在运行服务器核心安装选项的服务器之间进行迁移。

## <a name="before-you-begin"></a>开始之前

在开始迁移角色和功能之前，验证源和目标服务器是否都在运行适用于其操作系统的最新 Service Pack。 

> [!NOTE]
> 每当迁移到或升级到任何版本的 Windows Server 时，都应查看并了解[支持生命周期策略](https://support.microsoft.com/lifecycle)以及该版本的时间范围，并且作出相应的计划。 你可以[搜索生命周期信息](https://support.microsoft.com/lifecycle)，以便了解你感兴趣的特定 Windows server 版本。

## <a name="windows-server-2019"></a>Windows Server Standard 2012 R2

若要将文件服务器和存储迁移到 Windows Server 2019 或 Windows Server 2016，建议使用[存储迁移服务](../storage/storage-migration-service/overview.md)。 若要迁移其他角色，请参阅 Windows Server 2016 和 Windows Server 2012 R2 的指南。

## <a name="windows-server-2016"></a>Windows Server 2016

下面是 Windows Server 2016 的迁移指南。 请注意，在许多情况下，也可使用 Windows Server 2012 R2 迁移指南。

- [远程桌面服务](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Web 服务器 (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)

若要将文件服务器迁移到 Windows Server 2019 或 Windows Server 2016，建议使用[存储迁移服务](../storage/storage-migration-service/overview.md)。

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

按照这些指南中的步骤将角色和功能从运行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 或 Windows Server 2012 R2 的服务器迁移到 Windows Server 2012 R2。 Windows Server 2012 R2 中的 Windows Server 迁移工具支持跨子网迁移。

- [安装、使用和删除 Windows Server 迁移工具](https://technet.microsoft.com/library/jj134202.aspx)
- [适用于 Windows Server 2012 R2 的 Active Directory 证书服务迁移指南](https://technet.microsoft.com/library/dn486797.aspx)
- [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Active Directory Rights Management Services 迁移和升级指南](https://technet.microsoft.com/library/cc754277.aspx)
- [将文件和存储服务迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [将 Hyper-V 从 Windows Server 2012 迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn486799.aspx)
- [将网络策略服务器迁移到 Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [将远程桌面服务迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [将 Windows Server Update Services 迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [将群集角色迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [将 DHCP 服务器迁移到 Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)

现已提供 Windows Server 2012 R2 和 Windows Server 2012 迁移指南的电子书。 有关详细信息，以及如何下载本电子书，请参阅 [Microsoft 技术电子图书库](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles)。

## <a name="windows-server-2012"></a>Windows Server 2012

按照这些指南中的步骤将角色和功能从运行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 的服务器迁移到 Windows Server 2012。 Windows Server 2012 中的 Windows Server 迁移工具支持跨子网迁移。

- [安装、使用和删除 Windows Server 迁移工具](https://technet.microsoft.com/library/jj134202)
- [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [将健康注册机构迁移到 Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [将 Hyper-V 从 Windows Server 2008 R2 迁移到 Windows Server 2012](https://technet.microsoft.com/library/jj574113)
- [将 IP 配置迁移到 Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [将网络策略服务器迁移到 Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [将打印和文档服务迁移到 Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [将远程访问迁移到 Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [将 Windows Server Update Services 迁移到 Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [将 Active Directory 域控制器升级到 Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [将群集服务和应用程序迁移到 Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

有关其他迁移资源，请访问[将角色和功能迁移到 Windows Server 2012](https://technet.microsoft.com/library/jj134039)。

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

按照这些指南中的步骤将角色和功能从运行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的服务器迁移到 Windows Server 2008 R2。 Windows Server 2008 R2 中的 Windows Server 迁移工具不支持跨子网迁移。

- [Windows Server 迁移工具的安装、访问和删除](https://technet.microsoft.com/library/dd379545)
- [Active Directory 证书服务迁移指南](https://technet.microsoft.com/library/ee126170)
- [Active Directory 域服务和域名系统 (DNS) 服务器迁移指南](https://technet.microsoft.com/library/dd379558)
- [BranchCache 迁移指南](https://technet.microsoft.com/library/dd548365)
- [动态主机配置协议 (DHCP) 服务器迁移指南](https://technet.microsoft.com/library/dd379535)
- [文件服务迁移指南](https://technet.microsoft.com/library/dd379487)
- [HRA 迁移指南](https://technet.microsoft.com/library/ee791829)
- [Hyper-V 迁移指南](https://technet.microsoft.com/library/ee849855)
- [IP 配置迁移指南](https://technet.microsoft.com/library/dd379537)
- [本地用户和组迁移指南](https://technet.microsoft.com/library/dd379531)
- [NPS 迁移指南](https://technet.microsoft.com/library/ee791849)
- [打印服务迁移指南](https://technet.microsoft.com/library/dd379488)
- [远程桌面服务迁移指南](https://technet.microsoft.com/library/ff849223)
- [RRAS 迁移指南](https://technet.microsoft.com/library/ee822825)
- [Windows Server 迁移常见任务和信息](https://technet.microsoft.com/library/ff400258)
- [Windows Server Update Services 3.0 SP2 迁移指南](https://technet.microsoft.com/library/ee822826)
 
有关其他迁移资源，请访问[将角色和功能迁移到 Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353)。

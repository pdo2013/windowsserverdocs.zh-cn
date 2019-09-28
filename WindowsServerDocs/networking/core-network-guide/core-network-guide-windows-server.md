---
title: 适用于 Windows Server 的核心网络指南
description: 本主题提供了核心网络指南的概述，使你可以使用 Windows Server 2016 在新林中规划和部署完全正常运行的网络和新的 Active Directory 域所需的核心组件。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 52f8b9e1446b5b3f3b1e7060cc737204771d1eae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356073"
---
# <a name="core-network-guidance-for-windows-server"></a>适用于 Windows Server 的核心网络指南

>适用于：Windows Server、Windows Server 2016

本主题概述了 Windows Server @ no__t 2016 的核心网络指南，其中包含以下各节。  
  
-   [Windows Server 核心网络简介](#bkmk_intro)  
  
-   [Windows Server 核心网络指南](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Windows Server 核心网络简介

核心网络是一个为满足组织的信息技术 (IT) 需要提供基础服务的网络硬件、设备和软件的集合。

Windows Server 核心网络提供了包括以下各项在内的多个优点。

- 用于计算机和其他与传输控制协议/Internet 协议 (TCP/IP) 兼容设备之间进行网络连接的核心协议。 TCP/IP 是一套用于连接计算机和构建网络的标准协议。 TCP/IP 是随 Microsoft @ no__t 提供的网络协议软件，该软件可实现和支持 TCP/IP 协议套件。

- 动态主机配置协议 (DHCP) 服务器自动 IP 寻址。 与从 DHCP 服务器为计算机和其他设备动态提供 IP 地址租约相比，在网络上的所有计算机上手动配置 IP 地址非常耗时且不太灵活。

- 域名系统 (DNS) 名称解析服务。 DNS 允许用户、计算机、应用程序和服务使用计算机或设备的完全限定的域名在网络上查找计算机和设备的 IP 地址。

- 林，是共享相同类和属性定义（架构）、站点和复制信息（配置）以及全林性搜索功能（全局编录）的一个或多个 Active Directory 域。

- 目录林根级域，它是在新林中创建的第一个域。 Enterprise Admins 组和 Schema Admins 组是位于目录林根级域中的全林性管理组。 此外，目录林根级域也与其他域一样，是在 Active Directory 域服务 (AD DS) 中由管理员定义的计算机、用户和组对象的集合。 这些对象共享公用目录数据库和安全策略。 如果你随着组织的发展添加了域，它们还可以共享与其他域之间的安全关系。 目录服务还存储目录数据，并允许已授权的计算机、应用程序和用户访问数据。

- 用户和计算机帐户数据库。 目录服务提供了集中的用户帐户数据库，它允许你为被授权连接到网络并访问网络资源（例如应用程序、数据库、共享的文件和文件夹以及打印机）的人员和计算机创建用户和计算机帐户。

核心网络还允许随着组织的发展和 IT 需要的变化缩放网络。 例如，通过核心网络，可以添加域、IP 子网、远程访问服务、无线服务以及 Windows Server 2016 提供的其他功能和服务器角色。

## <a name="bkmk_core"></a>Windows Server 核心网络指南

Windows Server 2016 Core 网络指南提供了有关如何在新林中规划和部署完全正常运行的网络和新的 Active Directory @ no__t-0 域所需的核心组件的说明。 使用此指南，可以部署使用以下 Windows 服务器组件配置的计算机：

- Active Directory 域服务 (AD DS) 服务器角色

- 域名系统 (DNS) 服务器角色

- 动态主机配置协议 (DHCP) 服务器角色

- 网络策略和访问服务服务器角色的网络策略服务器 (NPS) 角色服务

- Web 服务器 (IIS) 服务器角色

- 单个服务器上的传输控制协议/Internet 协议版本 4 (TCP/IP) 连接

本指南可在以下位置找到。

- Windows Server 2016 技术库中的[核心网络指南](../core-network-guide/Core-Network-Guide.md)。
  



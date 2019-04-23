---
title: 适用于 Windows Server 的核心网络指南
description: 本主题提供了核心网络指南，可用于规划和部署完全正常运行的网络和新的 Active Directory 域中具有 Windows Server 2016 的新林所需的核心组件的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a905fd0c11237edd3a408998f8f71aa25a054328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847898"
---
# <a name="core-network-guidance-for-windows-server"></a>适用于 Windows Server 的核心网络指南

>适用于：Windows Server、 Windows Server 2016

本主题概述了适用于 Windows Server 核心网络指南&reg;2016，并包含以下各节。  
  
-   [Windows Server 核心网络简介](#bkmk_intro)  
  
-   [适用于 Windows Server 核心网络指南](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Windows Server 核心网络简介

核心网络是一个为满足组织的信息技术 (IT) 需要提供基础服务的网络硬件、设备和软件的集合。

Windows Server 核心网络提供了包括以下各项在内的多个优点。

- 用于计算机和其他与传输控制协议/Internet 协议 (TCP/IP) 兼容设备之间进行网络连接的核心协议。 TCP/IP 是一套用于连接计算机和构建网络的标准协议。 TCP/IP 是随 Microsoft 提供的网络协议软件&reg;Windows&reg;操作系统的实现，并支持 TCP/IP 协议套件。

- 动态主机配置协议 (DHCP) 服务器自动 IP 寻址。 与从 DHCP 服务器为计算机和其他设备动态提供 IP 地址租约相比，在网络上的所有计算机上手动配置 IP 地址非常耗时且不太灵活。

- 域名系统 (DNS) 名称解析服务。 DNS 允许用户、计算机、应用程序和服务使用计算机或设备的完全限定的域名在网络上查找计算机和设备的 IP 地址。

- 林，是共享相同类和属性定义（架构）、站点和复制信息（配置）以及全林性搜索功能（全局编录）的一个或多个 Active Directory 域。

- 目录林根级域，它是在新林中创建的第一个域。 Enterprise Admins 组和 Schema Admins 组是位于目录林根级域中的全林性管理组。 此外，目录林根级域也与其他域一样，是在 Active Directory 域服务 (AD DS) 中由管理员定义的计算机、用户和组对象的集合。 这些对象共享公用目录数据库和安全策略。 如果你随着组织的发展添加了域，它们还可以共享与其他域之间的安全关系。 目录服务还存储目录数据，并允许已授权的计算机、应用程序和用户访问数据。

- 用户和计算机帐户数据库。 目录服务提供了集中的用户帐户数据库，它允许你为被授权连接到网络并访问网络资源（例如应用程序、数据库、共享的文件和文件夹以及打印机）的人员和计算机创建用户和计算机帐户。

核心网络还允许随着组织的发展和 IT 需要的变化缩放网络。 例如，通过核心网络可以添加域、 IP 子网、 远程访问服务、 无线服务和其他功能和提供的 Windows Server 2016 的服务器角色。

## <a name="bkmk_core"></a>适用于 Windows Server 核心网络指南

Windows Server 2016 核心网络指南说明了如何规划和部署完全正常运行的网络和新的 Active Directory 所需的核心组件&reg;在新林中的域。 使用此指南，可以部署使用以下 Windows 服务器组件配置的计算机：

- Active Directory 域服务 (AD DS) 服务器角色

- 域名系统 (DNS) 服务器角色

- 动态主机配置协议 (DHCP) 服务器角色

- 网络策略和访问服务服务器角色的网络策略服务器 (NPS) 角色服务

- Web 服务器 (IIS) 服务器角色

- 单个服务器上的传输控制协议/Internet 协议版本 4 (TCP/IP) 连接

本指南则可以从以下位置。

- [核心网络指南](../core-network-guide/Core-Network-Guide.md)Windows Server 2016 技术库中。
  



---
title: Windows Server 的核心网络指南
description: 本主题提供核心网络指南，这允许你计划，并将其部署所需的完全正常网络域和新 Active Directory 新林中与 Windows Server 2016 的核心组件的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Windows Server 的核心网络指南

>适用于：Windows Server、Windows Server 2016

本主题提供的核心网络指南为 Windows Server 概述&reg;2016，并包含以下部分。  
  
-   [Windows Server 核心网络简介](#bkmk_intro)  
  
-   [Windows Server 的核心网络指南](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Windows Server 核心网络简介

核心网络集锦网络硬件、 设备，并且需要提供一些基本服务，为你的组织的信息的技术 (IT) 的软件。

Windows Server 核心网络为您提供了很多权益，其中包括以下。

- 在计算机和其他传输控件协议/Internet 协议 (TCP/IP) 的兼容设备之间的网络连接的核心协议。 TCP/IP 是一套连接计算机和生成网络标准协议。 TCP/IP 是使用 Microsoft 提供的网络协议软件&reg;Windows&reg;实现，并支持 TCP/IP 操作系统协议套件。

- 动态主机配置协议 (DHCP) 服务器自动 IP 地址。 手动配置你的网络上的所有计算机上的 IP 地址是耗时较低比动态提供计算机和其他设备 IP 地址租赁从 DHCP 服务器灵活。

- 域名系统 (DNS) 分辨率服务。 DNS 允许用户、 的计算机、 应用和服务使用完全限定域名计算机或设备在网络上查找计算机和设备的 IP 地址。

- 森林，这是一个或多个共享相同类和属性定义 （架构）、 网站和复制信息 （配置），并树林搜索功能 （全球目录） 的 Active Directory 域。

- 森林根域，这是第一个域创建新树林中。 企业管理员和方案管理员组，了树林管理组，都位于森林根域。 此外，森林根域中，使用其他域，是集合的计算机、 用户和组对象程序定义的 Active Directory 域服务 (广告 DS) 中的管理员。 这些对象共享常见的目录数据库和安全策略。 如果你的组织随着添加域，它们也可以与其他域共享安全关系。 目录服务还会存储目录数据，并允许授权的计算机、 应用程序和用户访问数据。

- 用户和计算机的帐户数据库。 目录服务提供允许您创建个人和获得授权，可连接到你的网络和访问网络资源，如应用、 数据库、 共享的文件和文件夹以及打印机的计算机用户和计算机帐户集中的用户帐户数据库。

核心网络还允许您您的组织成长和 IT 要求更改随着你的网络。 例如，使用 core 网络可以添加域、 IP 子网、 远程访问服务、 无线服务和其他功能和服务器角色由 Windows Server 2016。

## <a name="bkmk_core"></a>Windows Server 的核心网络指南

Windows Server 2016 Core 网络指南提供有关如何计划，并将其部署核心组件完全正常网络和新的 Active Directory 所需的说明进行操作&reg;新建森林中的域。 使用本指南，你可以将部署 Windows 以下服务器组件的配置计算机：

- Active Directory 域服务 (广告 DS) 服务器角色

- 域名系统 (DNS) 服务器角色

- 动态主机配置协议 (DHCP) 服务器角色

- 网络策略和服务的访问权限的服务器角色网络策略 Server (NPS) 角色服务

- Web 服务器 (IIS) 服务器角色

- 传输控件协议/Internet 协议版本 4 (TCP/IP) 各个服务器上的连接

本指南可以在以下位置。

- [核心网络指南](../core-network-guide/Core-Network-Guide.md)Windows Server 2016 Technical 库中。
  



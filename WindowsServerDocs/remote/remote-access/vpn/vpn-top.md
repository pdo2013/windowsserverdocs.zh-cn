---
title: 虚拟专用网 (VPN)
description: 你可以使用本主题以了解有关 Windows Server 2016 和 Windows 10 VPN 的特性和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067029"
---
# 虚拟专用网络 \(VPN\)

>适用于: Windows Server (半年频道)、Windows Server 2016、Windows 10

## RAS 网关为一个租户 VPN 服务器

在 Windows Server 2016 远程访问服务器角色是以下相关的网络访问技术的逻辑分组。

- 远程访问服务 (RA)
- 路由
- Web 应用程序代理

这些技术都远程访问服务器角色的角色服务。

当你使用添加角色和功能向导或 Windows PowerShell 安装远程访问服务器角色时，你可以安装一个或多个以下三个角色服务。

当你安装了**DirectAccess 和 VPN (RA)** 的角色服务时，你要部署远程访问服务网关 \ (**RAS 网关**\)。 你可以为一个租户 RAS 网关虚拟专用网络 \(VPN\) 服务器，可提供许多高级功能和增强功能部署 RAS 网关。

>[!NOTE]
>你还可以作为软件定义的网络 \(SDN\)，使用的多租户 VPN 服务器或 DirectAccess 服务器部署 RAS 网关。 有关详细信息，请参阅[RAS 网关](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)、[软件定义网络 (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)和[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## 相关主题
- [始终启用 VPN 特性和功能](vpn-map-da.md)： 在本主题中，你了解有关的特性和功能始终启用 VPN。 

- [Windows 10 中配置 VPN 的设备隧道](vpn-device-tunnel-config.md)： 始终启用 VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 始终启用 VPN 连接包括两种类型的隧道：_设备隧道_和_用户隧道_。 设备隧道用于登录前连接方案和设备管理目的。 用户隧道允许用户通过 VPN 服务器访问组织的资源。

- [始终在 VPN 部署的 Windows Server 2016 和 Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)： 提供有关部署为单个远程访问的说明允许远程连接到你的组织员工的点数格式 \ to\ 站点 VPN 连接的租户 VPN RAS 网关始终启用 VPN 连接的网络。 建议你查看每种技术，该部署中使用的设计和部署指南。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)： 将指导你完成将使 Windows 10 客户端在企业 VPN 解决方案以及如何配置你的部署中的决策。 你可以找到引用到 VPNv2 配置服务提供程序 (CSP)，并提供移动设备管理 (MDM) 配置说明适用于 Windows 10 中使用 Microsoft Intune 和 VPN 配置文件模板。

- [如何创建 VPN 配置文件在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)： 在本主题中，你将了解如何创建 System Center Configuration Manager (SCCM) 中的 VPN 配置文件。

- [配置 Windows 10 客户端始终在 VPN 连接](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)： 本主题介绍了 ProfileXML 选项和架构，如何创建 ProfileXML VPN。 设置后服务器基础结构，你必须配置 windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。 

- [VPN 配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)： 本主题介绍 Windows 10 中的 VPN 配置文件设置，并了解如何配置使用 Intune 或 SCCM 的 VPN 配置文件。 你可以在 Windows 10 中使用 VPNv2 CSP 中的 ProfileXML 节点中配置的所有 VPN 设置。

---
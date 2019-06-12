---
title: 虚拟专用网 (VPN)
description: 可以使用本主题以了解有关 Windows Server 2016 和 Windows 10 VPN 功能和功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bfd00b7a13e9fad113da1191e7ccd33965223070
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749557"
---
# <a name="virtual-private-networking-vpn"></a>虚拟专用网 (VPN)

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>RAS 网关作为单租户 VPN 服务器

在 Windows Server 2016 中，远程访问服务器角色是以下相关的网络访问技术的逻辑分组。

- 远程访问服务 (RAS)
- 路由
- Web 应用程序代理

这些技术是远程访问服务器角色的角色服务。

当使用添加角色和功能向导或 Windows PowerShell 安装远程访问服务器角色时，可以安装一个或多个这些三个角色服务。

当你安装**DirectAccess 和 VPN (RAS)** 角色服务，您要部署远程访问服务网关 (**RAS 网关**)。 你可以部署 RAS 网关提供许多高级功能和增强功能的单租户 RAS 网关虚拟专用网络 (VPN) 服务器。

>[!NOTE]
>为多租户 VPN 服务器用于与软件定义网络 (SDN)，或为 DirectAccess 服务器，还可以部署 RAS 网关。 有关详细信息，请参阅[RAS 网关](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)，[软件定义网络 (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)，并[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## <a name="related-topics"></a>相关主题
- [Always On VPN 特性和功能](vpn-map-da.md):在本主题中，您将学习如何的特性和功能的 Always On VPN。 

- [在 Windows 10 中配置 VPN 设备隧道](vpn-device-tunnel-config.md):Always On VPN 使你能够创建设备或计算机的专用的 VPN 配置文件。 Always On VPN 连接包括两种类型的隧道：_设备隧道_并_用户隧道_。 设备隧道用于登录前连接方案和进行设备管理。 用户隧道允许用户通过 VPN 服务器访问组织资源。

- [Always On VPN 部署 Windows Server 2016 和 Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md):说明了将远程访问部署为单个租户 VPN RAS 网关的允许你连接到 Always On VPN 连接你的组织网络的远程员工的点到站点 VPN 连接。 建议你查看针对每种技术在此部署中使用的设计和部署指南。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide):指导你完成将在企业 VPN 解决方案中的 Windows 10 客户端和如何配置你的部署中做出的决定。 您可以查找引用到 VPNv2 配置服务提供商 (CSP)，以及提供移动设备管理 (MDM) 适用于 Windows 10 中使用 Microsoft Intune 和 VPN 配置文件模板的配置说明。

- [如何创建 VPN 配置文件在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles):在本主题中，您将学习如何创建 System Center Configuration Manager (SCCM) 中的 VPN 配置文件。

- [配置 Windows 10 客户端 Always On VPN 连接](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections):本主题介绍 ProfileXML 选项和架构，以及如何创建 ProfileXML VPN。 设置后的服务器基础结构，必须配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。

- [VPN 配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options):本主题介绍了 Windows 10 中的 VPN 配置文件设置，并了解如何配置 VPN 配置文件使用 Intune 或 SCCM。 可以在 Windows 10 中使用 VPNv2 CSP 中的 ProfileXML 节点来配置所有 VPN 设置。

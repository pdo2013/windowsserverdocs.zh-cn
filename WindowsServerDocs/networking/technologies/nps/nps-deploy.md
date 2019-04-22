---
title: 部署网络策略服务器
description: 本主题提供的 Windows Server 2016 中，网络策略服务器部署内容的链接，并包括有关 NPS 的附加指导的链接。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814838"
---
# <a name="deploy-network-policy-server"></a>部署网络策略服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于部署网络策略服务器的信息。

>[!NOTE]
>有关网络策略服务器的其他文档，可以使用以下库部分。  
>- [开始使用网络策略服务器](nps-getstart-top.md)
>- [规划网络策略服务器](nps-plan-top.md)
>- [管理网络策略服务器](nps-manage-top.md)

Windows Server 2016 核心网络指南包括一节介绍了规划和安装网络策略服务器\(NPS\)，并为系统必备组件部署在 Active Directory 域中的 NPS 提供的指南中介绍的技术。 有关详细信息，请参阅 Windows Server 2016 中的"部署 NPS1"一节[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)。

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>部署 NPS 证书对 VPN 和 802.1x 访问

如果你想要部署身份验证方法，如可扩展身份验证协议\(EAP\)和受保护的 EAP，需要使用服务器证书在 NPS 上，你可以使用本指南部署 NPS 证书[部署服务器证书用于 802.1x 有线和无线部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="deploy-nps-for-8021x-wireless-access"></a>将 NPS 部署为 802.1x 无线访问

若要将 NPS 部署无线访问的可以使用本指南[部署基于密码的 802.1 X 身份验证无线访问](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>将 NPS 部署为 Windows 10 VPN 访问

您可以使用 NPS 处理始终在虚拟专用网络的连接请求\(VPN\)连接使用的运行 Windows 10 计算机和设备的远程员工。

有关详细信息，请参阅[远程访问始终在 VPN 部署指南为 Windows Server 2016 和 Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。


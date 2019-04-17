---
title: 部署策略服务器的网络
description: 本主题提供的 Windows Server 2016，网络策略服务器部署内容的链接并包含指向有关 NPS 其他指南。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>部署策略服务器的网络

>适用于：Windows Server（半年通道），Windows Server 2016

有关部署网络策略服务器信息，你可以使用本主题。

>[!NOTE]
>有关其他网络策略服务器文档，你可以使用以下库部分。  
>- [网络策略服务器入门](nps-getstart-top.md)
>- [计划网络策略服务器](nps-plan-top.md)
>- [管理网络策略服务器](nps-manage-top.md)

Windows Server 2016 Core 网络指南包括在规划，并安装网络策略服务器 \(NPS\) 和部署 NPS Active Directory 域中的先决条件作为中指南服务提供的技术一个部分。 详细信息，请参阅 Windows Server 2016 中的"部署 NPS1"一节[Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)。

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>VPN 和 802.1 X 访问部署 NPS Server 证书

如果你想要部署可扩展身份验证协议 \(EAP\) 和保护 EAP 等需要 NPS 服务器上的服务器证书的使用的身份验证方法，你可以与指南部署 NPS server 证书[802.1 X 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="deploy-nps-for-8021x-wireless-access"></a>用于 802.1 X 无线接入部署 NPS

若要为无线接入部署 NPS，你可以使用指南[基于部署密码 802.1 X 身份验证无线访问](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。

## <a name="deploy-nps-for-windows-10-vpn-access"></a>对于 Windows 10 VPN 访问部署 NPS

你可以使用 NPS 处理始终在虚拟专用网络 \(VPN\) 连接远程计算机和运行 Windows 10 的设备正在使用的员工的连接请求。

有关详细信息，请参阅[远程访问始终在 VPN 部署 Windows Server 2016 和指南 Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。


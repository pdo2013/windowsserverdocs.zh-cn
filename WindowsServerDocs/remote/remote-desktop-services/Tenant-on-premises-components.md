---
title: 租户本地组件
description: 介绍了在 RDS 部署中的本地组件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857398"
---
# <a name="tenant-on-premises-components"></a>租户本地组件

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下信息介绍了组成桌面托管部署的本地组件。  
  
##  <a name="clients"></a>客户端  
若要访问的托管的桌面和应用程序，用户必须使用支持远程桌面协议 (RDP) 7.1 或更高版本的远程桌面客户端。 具体而言，客户端必须支持远程桌面网关和远程桌面连接代理。 若要传送到本地桌面应用程序，客户端还必须支持 RemoteApp 功能。 若要实现最高的网关规模，客户端必须支持纯 HTTP 传输连接到 RD 网关。  
  
其他信息：  
[已启用 RemoteFX 的设备](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[什么是 Windows Server 2012 R2 远程桌面网关中的新增功能](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Microsoft 远程桌面客户端](https://technet.microsoft.com/library/dn473009.aspx)  
[有关在 Microsoft Store 中的 Windows 远程桌面应用](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Microsoft 远程桌面的 Google Play 上的 Android 应用](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac 应用商店的 Microsoft 远程桌面](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[应用商店中的 Microsoft 远程桌面](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory 域服务  
某些较大、 更复杂的租户可以选择托管在本地 Active Directory 域服务 (AD DS) 服务器。 在这种情况下，租户的环境中的 AD DS 服务器通常会在租户的本地 AD DS 服务器的副本。 通过在租户的环境中创建虚拟网络并使用 Azure VPN 站点到站点连接创建从租户的本地网络连接到 Azure 数据中心中的租户的虚拟网络支持此功能。  
  
其他信息：  
[Microsoft Azure 虚拟网络概述](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[创建具有站点到站点 VPN 连接使用 Azure 门户的资源管理器 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  



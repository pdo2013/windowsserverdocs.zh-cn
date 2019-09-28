---
title: 租户本地组件
description: 介绍 RDS 部署中的本地组件。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a9ff1382d2a2e7e2acf0247fa2ba4ae8e9642162
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387841"
---
# <a name="tenant-on-premises-components"></a>租户本地组件

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

以下信息介绍了构成桌面托管部署的本地组件。  
  
##  <a name="clients"></a>客户端  
若要访问托管桌面和应用程序，用户必须使用支持远程桌面协议 (RDP) 7.1 或更高版本的远程桌面客户端。 具体而言，客户端必须支持远程桌面网关和远程桌面连接代理。 若要将应用程序传送到本地桌面，客户端还必须支持 RemoteApp 功能。 若要实现最高网关规模，客户端必须支持指向 RD 网关的纯 HTTP 传输连接。  
  
其他信息：  
[启用了 RemoteFX 的设备](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Windows Server 2012 R2 远程桌面网关的新增功能](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Microsoft 远程桌面客户端](https://technet.microsoft.com/library/dn473009.aspx)  
[Microsoft Store 中的 Windows 版远程桌面应用](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Microsoft 远程桌面 - Google Play 上的 Android 应用](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac 应用商店 - Microsoft 远程桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[应用商店中的 Microsoft 远程桌面](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory 域服务  
某些更大且更复杂的租户可能会选择托管本地 Active Directory 域服务 (AD DS) 服务器。 在这种情况下，租户环境中的 AD DS 服务器通常会成为租户的本地 AD DS 服务器的副本。 可通过以下方式支持此功能：在租户的环境中创建虚拟网络，并使用 Azure VPN 在 Azure 数据中心创建从租户的本地网络到租户的虚拟网络的站点到站点连接。  
  
其他信息：  
[Microsoft Azure 虚拟网络概述](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[使用 Azure 门户创建具有站点到站点 VPN 连接的资源管理器 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  



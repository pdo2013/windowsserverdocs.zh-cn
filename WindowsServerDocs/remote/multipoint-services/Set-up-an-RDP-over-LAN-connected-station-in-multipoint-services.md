---
title: 在 MultiPoint Services 中设置通过 LAN 的连接工作站
description: 了解如何在 MultiPoint Services 中设置基于 RDP 的 LAN 系统
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 40899b277ae60169a0eca34b359172941e5391fb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871560"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>在 MultiPoint Services 中设置通过 LAN 的连接工作站
使用远程桌面协议（RDP）连接到局域网（LAN）上的 MultiPoint 服务的瘦客户端、传统台式计算机或便携式计算机是一台瘦客户端、传统台式计算机或便携式计算机。 有关此类和其他工作站类型的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>使用计算机或 LAN 上的瘦客户端设置 MultiPoint 工作站  
  
1.  打开运行 MultiPoint 服务的计算机。  
  
2.  确保 MultiPoint 服务器计算机通过交换机、路由器或其他网络设备连接到 LAN，并具有正确的 IP 地址。 （以169.254 （APIPA 地址）开头的 IP 地址可能指示 LAN 连接出现问题，或者无法访问 DHCP 服务器或该服务器无法正常工作。）  
  
3.  将客户端计算机或瘦客户端连接到 LAN。  
  
4.  打开客户端计算机或瘦客户端。  
  
5.  在客户端计算机或瘦客户端上，启动远程桌面连接或等效的应用程序，然后输入运行 MultiPoint 服务的计算机的名称或 IP 地址。

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>使用连接器服务设置用于远程管理的 Windows 10 设备
可以远程管理任何运行 Windows 10 的电脑或便携式计算机，只要：
- 连接器服务已启用  
- 已将计算机添加到 MultiPoint 服务器上的被管理的计算机。  

在运行 Windows 10 的电脑上，按照以下步骤启用 MultiPoint Connector：

1. 在搜索框中，键入 "打开或关闭 Windows 功能"，然后选择正确的搜索结果。 

2. 在功能列表中，启用**MultiPoint 连接器**。 这将启用管理设备所需的 MultiPoint 连接器服务。 

在 MultiPoint server 上：
1. 打开 "MultiPoint 管理器" 并选择 "**添加或删除个人计算机**" 或 "**添加或删除 MultiPoint 服务**"。

2. 选择要管理的远程计算机，然后单击 **"确定"** 。  系统将提示你输入远程计算机上的管理员凭据。  完成此操作后，会在 MultiPoint 管理器的 "主页" 选项卡上看到远程计算机。

成功设置仪表板管理器后，可以监视使用托管设备的用户。

> [!IMPORTANT]  
> 监视托管 Windows 10 设备时，administratrive 无法监视用户，除非服务器设置已相应更改。 请参阅[编辑服务器设置](Edit-Server-Settings.md)
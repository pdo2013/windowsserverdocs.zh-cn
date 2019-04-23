---
title: 设置 MultiPoint Services 中的 RDP over LAN 连接工作站
description: 了解如何在 MultiPoint Services 中的 RDP over LAN 系统设置
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
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843698"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>设置 MultiPoint Services 中的 RDP over LAN 连接工作站
RDP over LAN 连接工作站是瘦客户端、 传统的桌面或通过使用远程桌面协议 (RDP) 在局域网 (LAN) 连接到 MultiPoint 服务的便携式计算机。 有关此设置和其他工作站类型的详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)。  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>若要设置 MultiPoint 工作站的 LAN 上使用的计算机或瘦客户端  
  
1.  打开正在运行 MultiPoint 服务的计算机。  
  
2.  请确保在 MultiPoint Server 计算机通过交换机、 路由器或其他网络设备连接到 LAN 并且具有正确的 IP 地址。 （开头 169.254 （APIPA 地址） 的 IP 地址可能指示与 LAN 连接出现问题或 DHCP 服务器无法访问或不正常。）  
  
3.  连接到 LAN 的客户端计算机或瘦客户端。  
  
4.  启用客户端计算机或瘦客户端。  
  
5.  在客户端计算机或瘦客户端上，启动远程桌面连接或等效的应用程序，并输入的名称或运行 MultiPoint 服务的计算机的 IP 地址。

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>使用连接器服务设置 Windows 10 设备进行远程管理
可以远程管理任何 PC 或便携式计算机运行 Windows 10，只要：
- 已启用连接器服务  
- 计算机已添加到多点服务器上被管理的计算机。  

在运行 Windows 10 PC 上，请执行以下步骤启用多点连接器：

1. 在搜索框中，键入"关闭 Windows 功能打开或关闭"并选择正确的搜索结果。 

2. 在功能列表中启用**多点连接器**。 这将启用多点连接器服务需要用它们来管理设备。 

在 MultiPoint server:
1. 打开 MultiPoint Manager，并选择**添加或删除个人计算机**或**添加或删除 MultiPoint 服务**。

2. 选择你想要管理和单击的远程计算机**确定**。  系统会提示输入管理员凭据在远程计算机上。  完成此操作后，您将看到在 MultiPoint 管理器主页选项卡上的远程计算机。

当成功组启动仪表板管理器可以监视托管设备上工作的用户。

> [!IMPORTANT]  
> 监视管理 Windows 10 设备 administratrive 用户无法监视除已相应地更改设置的服务器。 请参阅[编辑服务器设置](Edit-Server-Settings.md)
---
title: 更改远程桌面中的侦听端口
description: 了解如何更改远程桌面客户端的侦听端口。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882718"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>更改远程桌面计算机上的侦听端口

>适用于：Windows 10、 Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2008 R2

当连接到计算机 （Windows 客户端或 Windows Server） 通过远程桌面客户端时，您的计算机上的远程桌面功能"听到"连接请求通过定义的侦听端口 (默认情况下为 3389)。 可以通过修改注册表来更改 Windows 计算机上的侦听端口。

1. 启动注册表编辑器。 （在搜索框中键入 regedit。）
2. 导航到以下注册表子项：HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. 单击**编辑 > 修改**，然后单击**十进制**。
4. 键入新的端口号，然后单击**确定**。 
5. 关闭注册表编辑器中，并重新启动计算机。

下次您连接到此计算机使用远程桌面连接中，您必须键入新端口。 如果正在使用防火墙，请确保将防火墙配置为允许连接到新的端口号。

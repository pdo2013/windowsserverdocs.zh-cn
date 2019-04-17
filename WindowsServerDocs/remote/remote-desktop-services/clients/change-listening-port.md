---
title: 更改在远程桌面的侦听端口
description: 了解如何更改为远程桌面客户端侦听的端口。
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
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297315"
---
# 更改你的计算机上侦听的端口远程桌面

>适用于： Windows 10、 Windows 8.1、 Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2008 R2

当连接到计算机 （Windows 客户端或 Windows Server） 通过远程桌面客户端时，你的计算机上的远程桌面功能"听到"连接请求通过定义侦听端口 (默认情况下的 3389)。 你可以通过修改注册表更改 Windows 的计算机上该侦听端口。

1. 启动注册表编辑器。 （在搜索框中键入 regedit）。
2. 导航到以下注册表子项： HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP Tcp\PortNumber
3. 单击**编辑 > 修改**，然后单击**十进制**。
4. 键入新的端口号，然后单击**确定**。 
5. 打开注册表编辑器中，并重新启动计算机。

下次连接到此计算机通过使用远程桌面连接，你必须键入新的端口。 如果你使用防火墙，请确保配置防火墙以允许连接到新的端口号。

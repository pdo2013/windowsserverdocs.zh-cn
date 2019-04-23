---
title: 在 Windows Server Essentials 中管理 VPN
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ec367337318d12161a250572745d8f303d098ffe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849198"
---
# <a name="manage-vpn-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理 VPN

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
 虚拟专用网络 (VPN) 连接使在家中或旅途中工作的用户可以使用公用网络（例如 Internet）提供的基础结构来访问专用网络上的服务器。 要使用 VPN 访问服务器资源，你必须完成以下步骤：  
  
-   [启用 VPN 进行远程访问服务器上](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [设置网络用户的 VPN 权限](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [客户端计算机连接到服务器](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [使用 VPN 连接到 Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a> 启用 VPN 进行远程访问服务器上  
 完成以下过程，在 Windows Server Essentials 中配置 VPN 以启用远程访问。  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>在 Windows Server Essentials 中启用 VPN  
  
1.  打开“仪表板”。  
  
2.  单击“设置”，然后单击“随处访问”选项卡。  
  
3.  单击 **“配置”**。 此时会出现“设置随处访问”向导。  
  
4.  在“选择要启用的随处访问功能”页  上，选中“虚拟专用网络”  复选框。  
  
5.  按照说明完成向导。  
  
##  <a name="BKMK_2"></a> 设置网络用户的 VPN 权限  
 你可以使用 VPN 连接到 Windows Server Essentials 并访问存储在服务器上的所有资源。 如果你有一台通过网络帐户设置的客户端计算机，并且该网络帐户可通过 VPN 连接来连接到托管的 Windows Server Essentials 服务器，则上述方法尤其有用。 在托管 Windows Server Essentials 服务器上新创建的所有用户帐户均必须在首次使用时通过 VPN 登录到客户端计算机。  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>设置网络用户的 VPN 权限  
  
1.  打开“仪表板”。  
  
2.  在导航栏上，单击“用户” 。  
  
3.  在用户帐户列表中，选择要授予其远程访问桌面的权限的用户帐户。  
  
4.  在中 **< 用户帐户\>任务**窗格中，单击**属性**。  
  
5.  在中 **< 用户帐户\>属性**，单击**随处访问**选项卡。  
  
6.  在“随处访问”  选项卡上，若要允许用户通过 VPN 连接到服务器，请选中“允许虚拟专用网络 (VPN)”   复选框。  
  
7.  单击“应用”，然后单击“确定”。  
  
##  <a name="BKMK_Connect"></a> 客户端计算机连接到服务器  
 在运行 Windows Server Essentials 的服务器上启用 VPN 执行远程访问后，你可以使用 VPN 连接连接并访问服务器上存储的所有资源。 不过，你必须首先将计算机连接到该服务器。 使用“连接计算机与服务器向导”将计算机连接到该服务器时，客户端计算机上将自动生成 VPN 网络连接，这样即可在家中或旅途中工作时通过该连接访问服务器资源。 有关连接计算机与服务器的分步操作说明，请参阅 [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_3"></a> 使用 VPN 连接到 Windows Server Essentials  
 如果你有设置了网络帐户的客户端计算机，通过 VPN 连接，这些网络帐户可用来与运行 Windows Server Essentials 的托管服务器连接，则所有托管服务器上的新创建的用户帐户首次登录到客户端计算机时必须使用 VPN。 从连接到该服务器的客户端计算机中完成以下步骤。  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>使用 VPN 远程访问服务器资源  
  
1.  在客户端计算机上，按 Ctrl+Alt+Delete。  
  
2.  在登录屏幕上，单击“切换用户”  。  
  
3.  单击屏幕右下角的“网络登录”图标。  
  
4.  通过使用你的网络用户名和密码登录到 Windows Server Essentials 网络。  
  
## <a name="see-also"></a>请参阅  
  
-   [可以通过远程方式](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [管理随处访问](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)

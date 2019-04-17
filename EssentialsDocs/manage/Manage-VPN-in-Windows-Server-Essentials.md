---
title: "管理 Windows Server Essentials 中的 VPN"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 VPN

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials 
  
 虚拟专用网络 (VPN) 连接让用户工作在家中还是在路上使用的基础结构提供公共网络，如 Internet 访问专用网络上的服务器。 若要用于 VPN 访问服务器资源，您必须完成：  
  
-   [远程服务器上的访问启用 VPN](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [设置网络用户的 VPN 权限](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [连接到服务器的客户端计算机](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [使用 VPN 连接到 Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>远程服务器上的访问启用 VPN  
 完成以下过程 VPN 配置 Windows Server Essentials 以允许远程访问。  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>若要启用 Windows Server Essentials 中的 VPN  
  
1.  打开的面板。  
  
2.  单击**设置**，然后单击**任何地方访问**选项卡。  
  
3.  单击**配置**。 显示设置任何地方访问向导。  
  
4.  在**选择任何地方访问功能供**页上，选择**虚拟专用网络**复选框。  
  
5.  按照说明来完成向导。  
  
##  <a name="BKMK_2"></a>设置网络用户的 VPN 权限  
 你可以使用 VPN 可连接到 Windows Server Essentials 访问存储在服务器的你所有资源。 这是非常有用，如果你有可用于连接到 VPN 连接通过托管的 Windows Server Essentials 服务器的网络帐户与设置 client 计算机。 托管 Windows Server Essentials 服务器上的所有新创建的用户帐户必须使用 VPN 可首次登录到 client 计算机。  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>若要设置为网络用户的 VPN 权限  
  
1.  打开的面板。  
  
2.  在导航栏中，单击**用户**。  
  
3.  在用户帐户列表中，选择你想要授予权限远程访问桌面的用户帐户。  
  
4.  在**< 用户 Account\ > 任务**窗格中，单击**属性**。  
  
5.  在**< 用户 Account\ > 属性**，单击**任何地方访问**选项卡。  
  
6.  在**任何地方访问**选项卡上，可使用户可以通过使用 VPN 连接到服务器选择**允许虚拟专用网络 (VPN)**复选框。  
  
7.  单击**应用**，然后单击**确定**。  
  
##  <a name="BKMK_Connect"></a>连接到服务器的客户端计算机  
 VPN 运行 Windows Server Essentials 远程访问的服务器上启用后，你可以使用 VPN 连接访问存储在服务器的你所有资源，连接到。 但是，必须先将计算机连接到服务器。 当使用将我计算机连接到服务器向导将计算机连接到服务器 VPN 网络连接自动客户端计算机上生成，并可用于处理在家中还是在路上时，请访问服务器资源。 有关你的计算机连接到服务器的分步说明，请参阅[将计算机连接到服务器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_3"></a>使用 VPN 连接到 Windows Server Essentials  
 如果你有可用于连接到 VPN 连接，创建新的用户帐户通过运行 Windows Server Essentials 托管服务器的网络帐户与设置的客户端计算机托管的服务器必须使用 VPN 首次登录到 client 计算机。 完成以下过程中连接到服务器的客户端计算机。  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>若要使用 VPN 远程访问服务器资源  
  
1.  按 Ctrl + Alt + Delete 客户端计算机上。  
  
2.  单击**切换用户**登录屏幕上。  
  
3.  单击屏幕的右下角中的网络登录图标。  
  
4.  使用你的网络用户名和密码登录到 Windows Server Essentials 网络。  
  
## <a name="see-also"></a>请参阅  
  
-   [远程工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [管理任意位置的访问权限](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)

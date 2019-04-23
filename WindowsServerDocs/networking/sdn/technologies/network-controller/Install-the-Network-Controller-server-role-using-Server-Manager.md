---
title: 安装网络控制器服务器角色使用服务器管理器
description: 本主题将说明了如何在 Windows Server 2016 中使用服务器管理器安装网络控制器服务器角色。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859058"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>安装网络控制器服务器角色使用服务器管理器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题将说明了如何使用服务器管理器安装网络控制器服务器角色。

>[!IMPORTANT]
>不要部署物理主机上的网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色上的 HYPER-V 虚拟机\(VM\)安装在 HYPER-V 主机上。 在三个不同超上的 Vm 上安装网络控制器后\-V 主机，必须启用超\-V 软件定义的网络的主机\(SDN\)通过添加到网络控制器使用的主机Windows PowerShell 命令**新建 NetworkControllerServer**。 这样，要启用 SDN 软件负载均衡器函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
安装网络控制器后，必须额外的网络控制器配置为使用 Windows PowerShell 命令。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="to-install-network-controller"></a>若要安装网络控制器  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 添加角色和功能向导将打开。 单击“下一步” 。  
  
2.  在中**选择安装类型**，保留默认设置，然后单击**下一步**。  
  
3.  在中**选择目标服务器**，选择你想要安装网络控制器，然后单击的服务器**下一步**。  
  
4.  在中**选择服务器角色**，在**角色**，单击**网络控制器**。  
  
    ![网络控制器服务器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  **添加所需的网络控制器功能**对话框随即打开。 单击**将功能添加**。  
  
    ![为网络控制器中添加功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在中**选择服务器角色**，单击**下一步**。  
  
    ![单击“下一步”](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在中**选择功能**，单击**下一步**。  
  
8.  在中**网络控制器**单击**下一步**。  
  
9. 在中**确认安装选择**，查看你的选择。 安装网络控制器需要运行该向导后重新启动计算机。 正因为如此，单击**如果需要自动重新启动目标服务器**。 **添加角色和功能向导**对话框随即打开。 单击 **“是”**。  
  
    ![“添加角色和功能”向导](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在 **“确认安装选择”** 中，单击 **“安装”**。  
  
11. 网络控制器服务器角色安装在目标服务器上，然后在服务器的重新启动。  
  
12. 在计算机重新启动后，登录到计算机上，并通过服务器管理器中查看验证网络控制器的安装。  
  
    ![服务器管理器](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>请参阅  
[网络控制器](Network-Controller.md)  
  



---
title: 使用服务器管理器安装网络控制器服务器角色
description: 本主题提供有关如何使用 Windows Server 2016 中的服务器管理器安装网络控制器服务器角色的说明。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b656bbd823a10f1e36d1757bb53c4565d4e828c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405835"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>使用服务器管理器安装网络控制器服务器角色

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供有关如何使用服务器管理器安装网络控制器服务器角色的说明。

>[!IMPORTANT]
>不要将网络控制器服务器角色部署到物理主机上。 若要部署网络控制器，必须在安装在 Hyper-v 主机上的 Hyper-v 虚拟机 \(VM @ no__t-1 上安装网络控制器服务器角色。 在三个不同的 no__t 主机上的虚拟机上安装了网络控制器之后，必须通过使用 Windows PowerShell 将主机添加到网络控制器，为软件定义的网络启用超级 @ no__t-1V 主机 \(SDN @ no__t**NetworkControllerServer**命令。 这样做会使 SDN 软件负载均衡器正常工作。 有关详细信息，请参阅[NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
安装网络控制器之后，必须使用 Windows PowerShell 命令来配置其他网络控制器。 有关详细信息，请参阅[使用 Windows PowerShell 部署网络控制器](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="to-install-network-controller"></a>安装网络控制器  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 "添加角色和功能向导" 将打开。 单击“下一步”。  
  
2.  在 "**选择安装类型**" 中，保留默认设置，然后单击 "**下一步**"。  
  
3.  在 "**选择目标服务器**" 中，选择要在其中安装网络控制器的服务器，然后单击 "**下一步**"。  
  
4.  在 "**选择服务器角色**" 的 "**角色**" 中，单击 "**网络控制器**"。  
  
    ![网络控制器服务器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  此时将打开 "**添加网络控制器所需的功能**" 对话框。 单击 "**添加功能**"。  
  
    ![为网络控制器添加功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在 "**选择服务器角色**" 中，单击 "**下一步**"。  
  
    ![单击“下一步”](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在 "**选择功能**" 中，单击 "**下一步**"。  
  
8.  在**网络控制器**中，单击 "**下一步**"。  
  
9. 在 "**确认安装选择**" 中，检查你的选择。 安装网络控制器需要在向导运行后重新启动计算机。 为此，请单击 **"必要时自动重新启动目标服务器"** 。 此时将打开 "**添加角色和功能向导**" 对话框。 单击 **“是”** 。  
  
    ![“添加角色和功能”向导](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在 **“确认安装选择”** 中，单击 **“安装”** 。  
  
11. 网络控制器服务器角色安装在目标服务器上，然后重新启动服务器。  
  
12. 计算机重启后，通过查看服务器管理器登录到计算机并验证网络控制器安装。  
  
    ![服务器管理器](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>请参阅  
[网络控制器](Network-Controller.md)  
  



---
title: 安装网络控制器服务器角色使用服务器管理器
description: 本主题提供有关如何使用 Windows Server 2016 中服务器管理器中安装了网络控制器服务器角色说明进行操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>安装网络控制器服务器角色使用服务器管理器

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供有关如何安装网络控制器服务器角色使用服务器管理器的说明进行操作。

>[!IMPORTANT]
>不要部署物理主机上的了网络控制器服务器角色。 若要部署网络控制器，必须安装网络控制器服务器角色 Hyper-V 虚拟机上 \(VM\) Hyper-V 主机上安装。 通过添加到网络控制器使用 Windows PowerShell 命令主机上安装了网络控制器在虚拟机上三个不同的 Hyper\ V 主机后，必须启用软件定义网络 \(SDN\) Hyper\ V 主机**新建 NetworkControllerServer**。 通过执行此操作，你启用 SDN 软件负载平衡函数。 有关详细信息，请参阅[新建 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。
  
安装网络控制器后，你必须使用其他网络控制器配置的 Windows PowerShell 命令。 有关详细信息，请参阅[部署网络控制器使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="to-install-network-controller"></a>若要安装网络控制器  
  
1.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。 单击**下一步**。  
  
2.  在**选择安装类型**保持默认设置，请单击**下一步**。  
  
3.  在**选择目标服务器**，选择要安装网络控制器，然后单击的服务器**下一步**。  
  
4.  在**选择服务器角色**中**角色**，单击**网络控制器**。  
  
    ![网络控制器服务器角色](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  **添加所需的网络控制器的功能**对话框中打开。 单击**添加功能**。  
  
    ![网络控制器添加功能](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  在**选择服务器角色**，单击**下一步**。  
  
    ![单击下一页](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  在**选择功能**，单击**下一步**。  
  
8.  在**网络控制器**单击**下一步**。  
  
9. 在**确认安装选择**，查看你的选择。 安装网络控制器要求向导将运行后重启计算机。 出于此原因，请单击**必要时自动重新启动目标服务器**。 **添加角色和功能向导**对话框中打开。 单击**是**。  
  
    ![添加角色和功能向导](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. 在**确认安装选择**，单击**安装**。  
  
11. 目标服务器，在安装了网络控制器服务器角色，然后重新启动该服务器。  
  
12. 计算机重新启动后，登录到计算机并验证网络控制器安装通过查看服务器管理器。  
  
    ![服务器管理器](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>请参阅  
[网络控制器](Network-Controller.md)  
  



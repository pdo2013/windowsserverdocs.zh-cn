---
title: 为远程桌面准备虚拟机
description: 为远程桌面组件准备好 VM
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: 6a1f0bfef21351894d3b9c2cfd8d044491834f6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387248"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>为远程桌面创建虚拟机

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以在物理服务器或虚拟机上安装远程桌面服务组件。 

第一步是[在 Azure 中创建 Windows Server 虚拟机](/azure/virtual-machines/windows/quick-create-portal)。 需要创建三个 VM：一个用于 RD 会话主机，一个用于连接代理，一个用于 RD Web 和 RD 网关。 若要确保 RDS 部署的可用性，请创建可用性集（在 VM 创建过程中的“高可用性”下），并将该可用性集中的多个 VM 分组  。
 
创建 VM 后，使用以下步骤让它们为 RDS 做好准备。

1.  使用远程桌面连接 (RDC) 客户端连接到虚拟机：  
    1.  在 Azure 门户中，打开“资源组”视图，然后单击要用于部署的资源组。  
    2.  选择新的 RDSH 虚拟机（例如，Contoso-Sh1）。  
    3.  单击“连接”>“打开”以打开远程桌面客户端  。  
    4.  在客户端中，单击“连接”，然后单击“使用其他用户帐户”   。 输入本地管理员帐户的用户名和密码。  
    5.  当收到有关证书的警告时，单击“是”  。  
2.  启用远程管理：  
    1.  在服务器管理器中，单击“本地服务器”>“远程管理当前设置(已禁用)”  。  
    2.  选择“为此服务器启用远程管理”  。  
    3.  单击“确定”  。  
3.  可选：可以暂时将 Windows 更新设置为不自动下载和安装更新。 这有助于在部署 RDSH 服务器时防止更改和系统重启。  
    1.  在服务器管理器中，单击“本地服务器”>“Windows 更新当前设置”  。  
    2.  选择“高级选项”>“延迟升级”  。   
4.  将服务器添加到域：  
    1.  在服务器管理器中，单击“本地服务器”>“工作组当前设置”  。  
    2.  单击“更改”>“域”，然后输入域名（例如，Contoso.com）  。  
    3.  输入域管理员凭据。  
    4.  重新启动虚拟机。  
5.  对 RD Web 和 GW 虚拟机重复步骤 1 到 4。  
6.  对 RD 连接代理虚拟机重复步骤 1 到 4。  
7.  在 RD 连接代理虚拟机上初始化并格式化附加的磁盘：  
    1.  连接到 RD 连接代理虚拟机（上面的步骤 1）。  
    2.  在服务器管理器中，单击“工具”>“计算机管理”  。  
    3.  单击“磁盘管理”  。  
    4.  选择附加的磁盘，选择“MBR (主启动记录)”，然后单击“确定”   。  
    5.  右键单击新磁盘（标记为“未分配”），然后单击“新建简单卷”   。  
    6.  在“新建简单卷”向导中，接受默认值，但为“卷标”提供合适的名称（例如 Shares）   。  
8.  在 RD 连接代理虚拟机上，为用户配置文件磁盘和证书创建文件共享：   
    1.  打开文件资源管理器，单击“此电脑”，然后打开为文件共享添加的磁盘  。  
    2.  单击“主页”和“新建文件夹”   。  
    3.  输入用户磁盘文件夹的名称，例如“UserDisks”  。  
    4.  右键单击新文件夹，单击“属性”>“共享”>“高级共享”  。  
    5.  选择“共享此文件夹”，单击“权限”   。  
    6.  选择“任何人”，然后单击“删除”   。 现在单击“添加”，输入“域管理员”，然后单击“确定”    。  
    7.  选择“允许完全控制”，然后单击“确定”>“确定”>“关闭”   。  
    8.  重复步骤 c. 到 g. 为证书创建共享文件夹。   



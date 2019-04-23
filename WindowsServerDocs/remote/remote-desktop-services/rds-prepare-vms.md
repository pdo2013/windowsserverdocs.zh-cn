---
title: 为远程桌面准备虚拟机
description: 获取 Vm 准备好进行远程桌面组件
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871878"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>为远程桌面创建虚拟机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

物理服务器上或虚拟机上，可以安装远程桌面服务组件。 

第一步是[在 Azure 中创建 Windows Server 虚拟机](/azure/virtual-machines/windows/quick-create-portal)。 你将想要创建三个 Vm： 分别用于 RD 会话主机、 一个用于连接代理和一个用于 RD Web 和 RD 网关。 若要确保您的 RDS 部署的可用性，请创建一个可用性集 (下**高可用性**VM 创建过程中) 和组的多个 Vm，因为可用性集。
 
创建 Vm 后，使用以下步骤来准备 rds。

1.  连接到虚拟机使用远程桌面连接 (RDC) 客户端：  
    1.  在 Azure 门户中打开资源组视图中，，然后单击要用于部署的资源组。  
    2.  选择新的 RDSH 虚拟机 (例如，Contoso Sh1)。  
    3.  单击**Connect > 打开**打开远程桌面客户端。  
    4.  在客户端，单击**Connect**，然后单击**使用的其他用户帐户**。 输入本地管理员帐户的用户名和密码。  
    5.  单击**是**时收到有关证书的警告。  
2.  启用远程管理：  
    1.  在服务器管理器中，单击**本地服务器 > （禁用） 的远程管理当前设置**。  
    2.  选择**启用此服务器的远程管理**。  
    3.  单击 **“确定”**。  
3.  可选：暂时可以设置 Windows 更新不会自动下载并安装更新。 这有助于防止更改和系统重新启动时部署 RDSH 服务器。  
    1.  在服务器管理器中，单击**本地服务器 > Windows 更新当前设置**。  
    2.  选择**高级选项 > 延迟升级**。   
4.  将服务器添加到域：  
    1.  在服务器管理器中，单击**本地服务器 > 工作组当前设置**。  
    2.  单击**更改 > 域**，然后输入的域名 (例如，Contoso.com)。  
    3.  输入域管理员凭据。  
    4.  重新启动虚拟机。  
5.  RD Web 和网关虚拟机重复步骤 1 至 4。  
6.  RD 连接代理虚拟机重复步骤 1 至 4。  
7.  初始化和格式化 RD 连接代理虚拟机上附加的磁盘：  
    1.  连接到 RD 连接代理虚拟机 （上述第 1 步）。  
    2.  在服务器管理器中，单击**工具 > 计算机管理**。  
    3.  单击**磁盘管理**。  
    4.  选择附加的磁盘，然后**MBR （主引导记录）**，然后单击**确定**。  
    5.  右键单击新的磁盘 (标记为**未分配**)，单击**新建简单卷**。  
    6.  在中**新建简单卷**向导，接受默认值，但提供的适用名称**卷标**（如共享）。  
8.  RD 连接代理虚拟机上创建文件共享的用户配置文件磁盘和证书：   
    1.  打开文件资源管理器中，单击**此电脑**，并打开添加文件共享的磁盘。  
    2.  单击**主页**并**新文件夹**。  
    3.  例如，输入用户磁盘文件夹的名称**UserDisks**。  
    4.  右键单击新文件夹，然后单击**属性 > 共享 > 高级共享**。  
    5.  选择**共享此文件夹**然后单击**权限**。  
    6.  选择**每个人都**，然后单击**删除**。 现在，单击**外**，输入**Domain Admins**，然后单击**确定**。  
    7.  选择**允许完全控制**，然后单击**确定 > 确定 > 关闭**。  
    8.  重复步骤 c。 为 g。 若要创建证书的共享的文件夹。   



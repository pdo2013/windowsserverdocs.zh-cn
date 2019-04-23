---
title: 为远程桌面创建虚拟机
description: 创建虚拟机来托管在云中的远程桌面组件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: c5147636f73e8628aba549c4e05b9f23ab4ef9a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873928"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>为远程桌面创建虚拟机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

使用以下步骤将用于运行 Windows Server 2016 角色、 服务和功能所需的桌面托管部署的租户的环境中创建虚拟机。   
  
对于此示例中的基本部署，将创建 3 个虚拟机的最小值。 一台虚拟机将托管的远程桌面 (RD) 连接代理和许可证服务器角色服务和部署的文件共享。 第二个虚拟机将托管的 Web 访问和 RD 网关角色服务。  第三个虚拟机承载 RD 会话主机角色服务。 对于非常小的部署，可以通过使用 AAD 应用代理以消除从部署的所有公共终结点和合并到单个 VM 上的所有角色服务来降低 VM 成本。 对于大型部署，可以在单个虚拟机以允许更好的缩放上安装的各种角色服务。  
  
本部分概述了部署基于 Windows Server 映像中的每个角色的虚拟机所需的步骤[Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/)。 如果您需要创建虚拟机从一个自定义映像，需要 PowerShell，请查看[使用资源管理器和 PowerShell 创建 Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/)。 然后返回此处将附加的文件共享的 Azure 数据磁盘，以及为你的部署输入外部 URL。  
  
1.  [创建 Windows 虚拟机](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/)以承载 RD 连接代理、 远程桌面许可证服务器和文件服务器。  
  
    对于我们的目的，我们使用以下命名约定：  
    - RD 连接代理、 许可证服务器和文件服务器：   
        - VM:Contoso-Cb1  
        - 可用性集：CbAvSet    
    - RD Web 访问和 RD 网关服务器：   
        - VM:Contoso-WebGw1  
        - 可用性集：WebGwAvSet  
          
    - RD 会话主机：   
        - VM:Contoso-Sh1  
        - 可用性集：ShAvSet  
          
    每个 VM 使用相同的资源组。  
2.  创建并附加用户配置文件磁盘 (UPD) 共享的 Azure 数据磁盘：  
    1.  在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击为 RD 连接代理 (例如，Contoso Cb1) 创建的虚拟机。  
    2.  单击**设置 > 磁盘 > 附加新**。  
    3.  接受名称和类型的默认值。  
    4.  输入是足够大以保存有关租户的环境，包括用户配置文件磁盘和证书的网络共享的大小 （以 gb 为单位）。 每个用户计划，可以接近 5 GB  
    5.  接受位置和主机缓存的默认值，然后单击**确定**。  
3.  创建外部负载均衡器来访问外部部署：
    1. 在 Azure 门户中，单击**浏览 > 负载均衡器**，然后单击**添加**。
    2. 输入**名称**，选择**公共**作为**类型**的负载均衡器，然后选择相应**订阅**， **资源组**，并**位置**。
    3. 选择**选择公共 IP 地址**，**新建**，输入一个名称，然后选择**确定**。
    4. 选择**创建**创建负载均衡器。
4.  配置你的部署的外部负载均衡器
    1. 在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击创建部署的负载均衡器。
    2. 添加负载均衡器以将流量发送到后端池：
        1. 选择**后端池**并**添加**。
        2. 输入**名称**，然后选择**\+添加虚拟机**。
        3. 选择**可用性集**并**WebGwAvSet**。
        4. 选择**虚拟机**， **Contoso WebGw1**，**选择**，**确定**，并**确定**。
    3. 将探测添加这样的负载均衡器就会知道哪些计算机处于活动状态：
        1. 选择**探测**并**添加**。
        2. 输入**名称**（如 HTTPS)，选择**TCP**，输入**端口**443，然后选择**确定**。
    4. 输入负载均衡规则为传入的流量进行平衡：
        1. 选择**负载均衡规则**和**添加**
        2. 输入**名称**（如 HTTPS)，选择**TCP**，和两个 443**端口**并**后端端口**。
            - 对于 Windows 10 和 Windows Server 2016 部署中，将保留**会话暂留**作为**None**，否则为请选择**客户端 IP**。
        3. 选择**确定**接受 HTTPS 规则。
        4. 通过选择创建新的规则**添加**。
        5. 输入**名称**（例如 UDP)，选择**UDP**，和两个 3391 * * 端口并**后端端口**。
            - 对于 Windows 10 和 Windows Server 2016 部署中，将保留**会话暂留**作为**None**，否则为请选择**客户端 IP**。
        6. 选择**确定**接受 UDP 规则。
    5. 输入直接连接到 Contoso WebGw1 入站的 NAT 规则
        1. 选择**入站 NAT 规则**并**添加**。
        2. 输入**名称**（如 RDP-Contoso-WebGw1)，选择**Customm**服务， **TCP**作为协议，并输入为 14000**端口**。
        3. 选择**选择虚拟机**和 Contoso WebGw1。
        4. 选择**自定义**端口映射，请输入的 3389**目标端口**，然后选择**确定**。
5.  输入您的部署，从外部访问的外部 URL/DNS 名称：  
    1.  在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击创建为 RD Web 访问和 RD 网关的公共 IP 地址。  
    2.  单击**配置**，输入 DNS 名称标签 （如 contoso)，然后单击**保存**。 此 DNS 名称标签 (contoso.westus.cloudapp.azure.com) 是将用于连接到 RD Web 访问和 RD 网关服务器的 DNS 名称。  


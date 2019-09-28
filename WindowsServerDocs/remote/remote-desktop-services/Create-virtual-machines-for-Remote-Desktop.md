---
title: 为远程桌面创建虚拟机
description: 创建 VM 在云中托管远程桌面组件。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6db9973402578736b2e1283dcfa405642591eccc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387981"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>为远程桌面创建虚拟机

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

使用以下步骤在租户环境中创建虚拟机，这些虚拟机将用于运行桌面托管部署所需的 Windows Server 2016 角色、服务和功能。   
  
对于此基本部署示例，将创建最少 3 台虚拟机。 一个虚拟机将托管远程桌面 (RD) 连接代理和许可证服务器角色服务，以及用于部署的文件共享。 第二个虚拟机将托管 RD 网关和 Web 访问角色服务。  第三个虚拟机托管 RD 会话主机角色服务。 对于非常小的部署，可以通过使用 AAD 应用代理来消除部署中的所有公共终结点，并将所有角色服务组合到单个 VM 上，从而降低 VM 成本。 对于大型部署，可以在单个虚拟机上安装各种角色服务，以便更好地进行缩放。  
  
本部分概述了在 [Microsoft Azure 市场](https://azure.microsoft.com/marketplace/) 中基于 Windows Server 映像为每个角色部署虚拟机的必要步骤。 如果需要从需要 PowerShell 的自定义映像创建虚拟机，请查看[使用资源管理器和 PowerShell 创建 Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/)。 然后返回此处附加文件共享的 Azure 数据磁盘，并为部署输入外部 URL。  
  
1. [创建 Windows 虚拟机](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/)以托管 RD 连接代理、RD 许可证服务器和文件服务器。  
  
   出于本文讨论的目的，我们使用以下命名约定：  
   - RD 连接代理、许可证服务器和文件服务器：   
       - VM：Contoso-Cb1  
       - 可用性集：CbAvSet    
   - RD Web 访问和 RD 网关服务器：   
       - VM：Contoso-WebGw1  
       - 可用性集：WebGwAvSet  
          
   - RD 会话主机：   
       - VM：Contoso-Sh1  
       - 可用性集：ShAvSet  
          
   每个 VM 使用相同的资源组。  
2. 为用户配置文件磁盘 (UPD) 共享创建并附加 Azure 数据磁盘：  
   1.  在 Azure 门户中，单击“浏览”>“资源组”  ，单击部署的资源组，然后单击为 RD 连接代理创建的 VM（例如 Contoso-Cb1）。  
   2.  单击“设置”>“磁盘”>“附加新磁盘”  。  
   3.  接受名称和类型的默认值。  
   4.  输入足够大的大小（以 GB 为单位）来保存租户环境中的网络共享，包括用户配置文件磁盘和证书。 可以为每个计划拥有的用户提供大约 5 GB 的内存  
   5.  接受位置和主机缓存的默认值，然后单击“确定”  。  
3. 创建外部负载均衡器来访问外部部署：
   1. 在 Azure 门户中，单击“浏览”>“负载均衡器”  ，然后单击“添加”  。
   2. 输入“名称”  ，选择“公共”  作为负载均衡器的“类型”  ，然后选择相应“订阅”  、“资源组”  和“位置”  。
   3. 选择“选择公共 IP 地址”  ，“新建”  ，输入一个名称，然后选择“确定”  。
   4. 选择“创建”  创建负载均衡器。
4. 配置部署的外部负载均衡器
   1. 在 Azure 门户中，单击“浏览”>“资源组”  ，单击部署的资源组，然后单击为部署创建的负载均衡器。
   2. 添加负载均衡器向其发送流量的后端池：
       1. 选择“后端池”  和“添加”  。
       2. 输入“名称”  ，然后选择“\+添加虚拟机”  。
       3. 选择“可用性集”  和“WebGwAvSet”  。
       4. 选择“虚拟机”  、“Contoso-WebGw1”  、“选择”  、“确定”  ，然后选择“确定”  。
   3. 添加探测，这样负载均衡器就会知道哪些计算机处于活动状态：
       1. 选择“探测”  和“添加”  。
       2. 输入“名称”  （如 HTTPS)，选择“TCP”  ，输入“端口”  443，然后选择“确定”  。
   4. 输入负载均衡规则来平衡传入流量：
      1. 选择“负载均衡规则”  和“添加” 
      2. 输入“名称”  （如 HTTPS)，选择“TCP”  ，并为“端口”  和“后端端口”  输入 443。
          - 对于 Windows 10 和 Windows Server 2016 部署，将“会话暂留”  保留为“无”  ，否则请选择“客户端 IP”  。
      3. 选择“确定”  接受 HTTPS 规则。
      4. 通过选择“添加”  创建新规则。
      5. 输入“名称”  （例如 UDP)，选择“UDP”  ，并为“端口”和“后端端口”输入 3391。
          - 对于 Windows 10 和 Windows Server 2016 部署，将“会话暂留”  保留为“无”  ，否则请选择“客户端 IP”  。
      6. 选择“确定”  接受 UDP 规则。
   5. 输入入站 NAT 规则以直接连接到 Contoso-WebGw1
       1. 选择“入站 NAT 规则”  和“添加”  。
       2. 输入“名称”  （如 RDP-Contoso-WebGw1)，为服务选择“自定义”  ，选择“TCP”  作为协议，并为“端口”  输入 14000。
       3. 选择“选择虚拟机”  和 Contoso-WebGw1。
       4. 为端口映射选择“自定义”  ，为“目标端口”  输入 3389，然后选择“确定”  。
5. 为部署输入外部 URL/DNS 名称，以便从外部访问它：  
   1.  在 Azure 门户中，单击“浏览”>“资源组”  ，单击部署的资源组，然后单击为 RD Web 访问和 RD 网关创建的公共 IP 地址。  
   2.  单击“配置”  ，输入 DNS 名称标签（如 contoso)，然后单击“保存”  。 此 DNS 名称标签 (contoso.westus.cloudapp.azure.com) 是将用于连接到 RD Web 访问和 RD 网关服务器的 DNS 名称。  


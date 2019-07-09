---
title: 通过添加 RD 会话主机场横向扩展 RDS 部署
description: 将第二个 RD 会话主机添加到 RDS 环境。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0e3852b4ea5f1080a3798c0806e5c87ca808c3be
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446519"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>通过添加 RD 会话主机场横向扩展远程桌面服务部署

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

通过添加远程桌面会话主机 (RDSH) 场，可以改善 RDS 部署的可用性和规模。   
  
 
使用以下步骤将另一个 RD 会话主机添加到部署：  
  
1. 创建用于托管第二个 RD 会话主机的服务器。 如果使用的是 Azure 虚拟机，请确保将新的 VM 包含在持有第一个 RD 会话主机的同一可用性集中。
2. 启用新服务器或虚拟机上的远程管理：
   1. 在“服务器管理器”中，单击“本地服务器”>“远程管理当前设置（禁用）”  。 
   2. 选择“启用此服务器的远程管理”  ，然后单击“确定”  。 
   3. 可选：可以暂时将 Windows 更新设置为不自动下载和安装更新。 这有助于在部署 RDSH 服务器时防止更改和系统重启。 在“服务器管理器”中，单击“本地服务器”>“Windows 更新当前设置”  。 单击“高级选项”>“延迟升级”  。 
3. 将服务器或 VM 添加到域：
   1. 在“服务器管理器”中，单击“本地服务器”>“工作组当前设置”  。 
   2. 单击“更改”>“域”  ，然后输入域名（例如 Contoso.com）。 
   3. 输入域管理员凭据。 
   4. 重启服务器或 VM。
4. 将新的 RD 会话主机添加到场：
   >[!NOTE] 
   > 步骤 1，为 RDMS 虚拟机创建公共 IP 地址，仅在为 RDMS 使用 VM 并且尚未分配 IP 地址时才需要。
   
   1. 为运行远程桌面管理服务 (RDMS) 的虚拟机创建公共 IP 地址。 RDMS 虚拟机通常是运行 RD 连接代理角色的第一个实例的虚拟机。  
       1. 在 Azure 门户中，单击“浏览”>“资源组”  ，单击部署的资源组，然后单击 RDMS 虚拟机（例如 Contoso-Cb1）。  
       2. 单击“设置”>“网络接口”  ，然后单击相应的网络接口。   
       3. 单击“设置”>“IP 地址”  。
       4. 对于“公共 IP 地址”  ，选择“已启用”  ，然后单击“IP 地址”  。   
       5. 如果要使用现有的公共 IP 地址，请从列表中选择它。 否则，单击“新建”  ，输入名称，然后单击“确定”  ，最后单击“保存”  。   
   2. 登录到 RDMS。
   3. 将新的 RDSH 服务器添加到服务器管理器：   
       1. 启动“服务器管理器”，单击“管理”>“添加服务器”  。   
       2. 在“添加服务器”对话框中，单击“立即查找”  。   
       3. 选择要用于 RD 会话主机或新创建的虚拟机的服务器（例如 Contoso-Sh2），然后单击“确定”  。
   4. 将 RDSH 服务器添加到部署
       1. 启动服务器管理器。  
       2. 单击“远程桌面服务”>“概述”>“部署服务器”>“任务”>“添加 RD 会话主机服务器”  。   
       3. 选择新服务器（例如 Contoso-Sh2），然后单击“下一步”  。  
       4. 在“确认”页上，选择“根据需要重启远程计算机”  ，然后单击“添加”  。   
   5. 将 RDSH 服务器添加到集合场：
       1. 启动服务器管理器。   
       2. 单击“远程桌面服务”  ，然后单击要添加新创建的 RDSH 服务器（例如 ContosoDesktop）的集合。   
       3. 在“主机服务器”  下，单击“任务”>“添加 RD 会话主机服务器”  。   
       4. 选择新创建的服务器（例如 Contoso-Sh2），然后单击“下一步”  。   
       5. 在“确认”页上，单击“添加”  。   


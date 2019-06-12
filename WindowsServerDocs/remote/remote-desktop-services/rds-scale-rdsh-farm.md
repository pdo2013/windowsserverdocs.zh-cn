---
title: 通过添加 RD 会话主机场横向扩展你的 RDS 部署
description: 添加第二个 RD 会话主机到 RDS 环境。
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446519"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>通过添加 RD 会话主机场横向扩展你的远程桌面服务部署

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

通过添加远程桌面会话主机 (RDSH) 场，即可改善的可用性和 RDS 部署的规模。   
  
 
使用以下步骤添加到你的部署的另一台 RD 会话主机：  
  
1. 创建用于托管第二个 RD 会话主机服务器。 如果使用 Azure 虚拟机，请务必保存您的第一个 RD 会话主机在同一可用性集中包括新的 VM。
2. 启用新的服务器或虚拟机上的远程管理：
   1. 在服务器管理器中，单击**本地服务器 > （禁用） 的远程管理当前设置**。 
   2. 选择**启用此服务器的远程管理**，然后单击**确定**。 
   3. 可选：暂时可以设置 Windows 更新不会自动下载并安装更新。 这有助于防止更改和系统重新启动时部署 RDSH 服务器。 在服务器管理器中，单击**本地服务器 > Windows 更新当前设置**。 单击**高级选项 > 延迟升级**。 
3. 向域添加的服务器或虚拟机：
   1. 在服务器管理器中，单击**本地服务器 > 工作组当前设置**。 
   2. 单击**更改 > 域**，然后输入的域名 (例如，Contoso.com)。 
   3. 输入域管理员凭据。 
   4. 重新启动服务器或 vm。
4. 添加新的 RD 会话主机场：
   >[!NOTE] 
   > 步骤 1，正在创建 RDMS 虚拟机的公共 IP 地址，如果将 vm 用于 RDMS，如果它尚不包含分配的 IP 地址才有必要。
   
   1. 创建运行远程桌面管理服务 (RDMS) 的虚拟机的公共 IP 地址。 RDMS 虚拟机通常会运行 RD 连接代理角色的第一个实例的虚拟机。  
       1. 在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击 RDMS 虚拟机 (例如，Contoso Cb1)。  
       2. 单击**设置 > 网络接口**，然后单击相应的网络接口。   
       3. 单击**设置 > IP 地址**。
       4. 有关**公共 IP 地址**，选择**已启用**，然后单击**IP 地址**。   
       5. 如果你有想要使用现有公共 IP 地址，它从列表中选择。 否则，请单击**新建**，输入一个名称，然后单击**确定**，然后**保存**。   
   2. 登录到 RDMS。
   3. 将新的 RDSH 服务器添加到服务器管理器：   
       1. 启动服务器管理器中，单击**管理 > 添加服务器**。   
       2. 在添加服务器对话框中，单击**立即查找**。   
       3. 选择你想要使用的 RD 会话主机或新创建的虚拟机 (例如，Contoso Sh2)，并单击的服务器**确定**。
   4. 向部署添加 RDSH 服务器
       1. 启动服务器管理器。  
       2. 单击**远程桌面服务 > 概述 > 部署服务器 > 任务 > 添加 RD 会话主机服务器**。   
       3. 选择新服务器 (例如，Contoso-Sh2)，然后依次**下一步**。  
       4. 在确认页上选择**根据需要重新启动远程计算机**，然后单击**添加**。   
   5. 将 RDSH 服务器添加到集合场：
       1. 启动服务器管理器。   
       2. 单击**远程桌面服务**，然后单击你想要添加新创建的 RDSH 服务器 (例如，ContosoDesktop) 的集合。   
       3. 下**主机服务器**，单击**任务 > 添加 RD 会话主机服务器**。   
       4. 选择新创建的服务器 (例如，Contoso-Sh2)，然后依次**下一步**。   
       5. 在确认页上单击**添加**。   


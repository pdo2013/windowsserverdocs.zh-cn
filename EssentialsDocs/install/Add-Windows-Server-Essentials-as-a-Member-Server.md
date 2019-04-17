---
title: "添加 Windows Server Essentials 为成员服务器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>添加 Windows Server Essentials 为成员服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题适合安装了 Windows Server Essentials 体验角色与运行 Windows Server 2012 R2 标准、 Windows Server 2012 R2 Datacenter 或 Windows Server 2016 的服务器。 在本文的其余部分中，Windows Server Essentials 体验角色称为 Windows Server Essentials。  
  
> [!NOTE]
>   只能与域控制器部署 Windows Server Essentials。 在本文中，Windows Server Essentials 不包括 Windows Server Essentials。  
  
 Windows Server Essentials 不需要进行 Windows 域中的主服务器。 你可以将 Windows Server Essentials 作为成员服务器添加到现有的 Active Directory 域环境，并充分利用简单的数据保护、 安全远程访问，并且它提供的云集成功能。 此外，Windows Server Essentials 可以将环境中部署现有 Active Directory 而无需域控制器。 这使你可以扩展存储或使用本地存储和管理分支机构。  
  
 你可以在以下情况下添加 Windows Server Essentials:  
  
-   在 office 位置 branch 添加 Windows Server Essentials 并加入的域控制器上的不同位置总部位于使用原始工具。 你可以打开此成员服务器上的最佳带宽使用量分支缓存功能。  
  
-   添加 Windows Server Essentials 成员服务器内的 Windows Server Essentials 网络来通过添加成员服务器上的其他 server 文件夹扩展存储在你的网络。  
  
-   Windows Server Essentials 添加与本地 office 中的成员服务器，如果你运行的 Windows Server Essentials 的主服务器存放在 Microsoft Azure 或由第三方宿主托管。 在 Windows Server Essentials 遇到为在你的本地 office 站点帮助优化带宽使用量成员服务器。  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>添加 Windows Server Essentials 为成员服务器  
 若要为成员服务器的 Windows Server Essentials 添加到主服务器现有的 Active Directory 环境中运行 Windows Server 2012 R2 或 Windows Server Essentials，必须完成以下步骤：  
  
1.  将运行 Windows Server Essentials 工作组到服务器的加入。  
  
2.  连接到主服务器 Windows Server Essentials 域运行 Windows Server Essentials 的服务器。  
  
3.  配置 Windows Server Essentials 体验中服务器管理器。  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>若要向工作组或域加入 Windows Server Essentials  
  
1.  在第二个服务器上的 Windows Server Essentials 安装后，关闭配置 Windows Server Essentials 向导。  
  
2.  在**搜索**框中，键入**系统设置**，然后在搜索结果中，单击**查看高级系统设置**。  
  
3.  在**系统属性**，单击**计算机名称**选项卡。  
  
4.  在**计算机名称**中**域**部分中，单击**更改**。  
  
5.  在**计算机名月域更改**中**成员**部分中，选择你是否希望将运行 Windows Server Essentials 到服务器加入**工作组**或**域**。  
  
    -   若要添加到在工作组中，键入**工作组**，然后单击**确定**。  
  
    -   此服务器加入现有 Active Directory 域中，键入域的名称，然后单击**确定**。  
  
6.  重新启动以应用更改服务器。  
  
 到主服务器的域加入服务器后，你可以继续通过运行配置 Windows Server Essentials 向导中服务器管理器配置 Windows Server Essentials。  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>若要配置成员服务器上的 Windows Server Essentials 体验  
  
1.  （可选）如果需要，更改服务器名称。  
  
    > [!IMPORTANT]
    >  配置 Windows Server Essentials 体验后，你无法更改服务器的名称。  
  
2.  使用你的域管理员帐户登录到服务器。  
  
3.  打开服务器管理器。  
  
4.  在通知区域中的标志**服务器管理器**、 单击标志，，然后单击**配置 Windows Server Essentials**。  
  
5.  选择要成员服务器，为配置服务器，然后单击**下一步**。  
  
6.  单击**配置**开始进行配置。 配置过程需要大约 10 分钟的时间才能完成。  
  
7.  在桌面上，请单击仪表板图标以启动服务器仪表板。 在主页上，填写**入门**列出的任务**设置**选项卡。  
  
## <a name="see-also"></a>请参阅  
  

-   [安装 Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [安装 Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)


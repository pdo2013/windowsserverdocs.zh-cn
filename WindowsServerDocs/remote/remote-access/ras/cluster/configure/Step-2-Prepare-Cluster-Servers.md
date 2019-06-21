---
title: 第 2 步准备群集服务器
description: 本主题是指南的在 Windows Server 2016 中的群集中部署远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10a40b30acbf022ed34f454d753884cb8c5c97d4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281200"
---
# <a name="step-2-prepare-cluster-servers"></a>第 2 步准备群集服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以配置群集部署之前，你准备将添加到群集的其他服务器。  
  
|任务|描述|  
|----|--------|  
|[2.1 配置远程访问基础结构](#BKMK_config)|每个服务器上你想要添加到群集，配置服务器拓扑、 IP 寻址、 路由和转发。 如果配置负载平衡的群集的虚拟机，则必须配置虚拟机使用 MAC 地址欺骗。<br /><br />此外，将每个服务器加入到同一个域，并将所有服务器都连接到同一子网。|  
|[2.2 安装远程访问角色](#BKMK_Install)|每个其他服务器上你想要添加到群集，安装远程访问角色|  
|[2.3 Install NLB](#BKMK_NLB)|已部署的远程访问服务器上和你想要添加到群集的每个其他服务器上，安装 NLB 功能。 请注意，此步骤不需要使用一个外部负载均衡器时。|  
  
## <a name="BKMK_config"></a>2.1 配置远程访问基础结构  
若要配置远程访问群集，必须配置服务器拓扑、 IP 寻址、 路由和转发将形成群集的一部分的每个服务器上。  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>若要配置远程访问基础结构  
  
1.  每个将具有相同的拓扑为第一个远程访问服务器的群集中的服务器配置。  
  
2.  每个将具有相应 IP 寻址、 路由和转发基于第一个远程访问服务器的配置的群集中的服务器配置。 请注意，在群集中的所有服务器必须都连接到同一子网。  
  
3.  将每台服务器将与第一台远程访问服务器位于同一域到群集中的联接。  
  
## <a name="BKMK_Install"></a>2.2 安装远程访问角色  
若要配置远程访问群集，必须在将形成群集的一部分的每个服务器上安装远程访问角色。  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Always On VPN 服务器上安装远程访问角色  
  
1.  在 DirectAccess 服务器上，在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击 **“下一步”** 三次以打开服务器角色选择屏幕。  
  
3.  上**选择服务器角色**对话框中，选择**远程访问**，然后单击**下一步**。  
  
4.  单击**下一步**三次。  
  
5.  上**选择角色服务**对话框中，选择**DirectAccess 和 VPN (RAS)** ，然后单击**添加功能**。  
  
6.  选择**路由**，选择**Web 应用程序代理**，单击**添加功能**，然后单击**下一步**。  
  
7. 单击 **“下一步”** ，然后单击 **“安装”** 。  
  
8.  在“安装进度”  对话框中，验证安装是否成功，然后单击“关闭”  。  
  
9.  重复此过程在你想要成为群集成员的所有服务器上。  
  
## <a name="BKMK_NLB"></a>2.3 安装 NLB  
若要配置远程访问群集，必须在将形成群集的一部分的每个服务器上安装网络负载平衡功能。  
  
> [!NOTE]  
> 如果使用外部负载均衡器，则不需要此步骤。  
  
#### <a name="to-install-the-nlb-role"></a>若要安装 NLB 角色  
  
1.  在 DirectAccess 服务器上，在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击**下一步**四次以转到服务器的功能选择屏幕上。  
  
3.  上**选择的功能**对话框中，选择**网络负载平衡**，单击**添加功能**，单击**下一步**，然后单击**安装**。  
  
4.  在“安装进度”  对话框中，验证安装是否成功，然后单击“关闭”  。  
  
5.  重复此过程在你想要成为群集成员的所有服务器上。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 3：配置负载平衡群集](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  



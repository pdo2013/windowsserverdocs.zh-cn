---
title: 步骤2准备群集服务器
description: 本主题是在 Windows Server 2016 的群集中部署远程访问指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87983076ee8a7d5546a5ac491ed4ca88153798f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367406"
---
# <a name="step-2-prepare-cluster-servers"></a>步骤2准备群集服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

在配置群集部署之前，准备要添加到群集中的其他服务器。  
  
|任务|描述|  
|----|--------|  
|[2.1 配置远程访问基础结构](#BKMK_config)|在要添加到群集的每个服务器上，配置服务器拓扑、IP 寻址、路由和转发。 如果配置虚拟机的负载平衡群集，则必须将虚拟机配置为使用 MAC 地址欺骗。<br /><br />此外，将每个服务器联接到同一个域，并将所有服务器连接到同一子网。|  
|[2.2 安装远程访问角色](#BKMK_Install)|在要添加到群集的每个其他服务器上，安装远程访问角色|  
|[2.3 安装 NLB](#BKMK_NLB)|在部署的远程访问服务器和每个要添加到群集中的其他服务器上，安装 NLB 功能。 请注意，使用外部负载均衡器时，不需要执行此步骤。|  
  
## <a name="BKMK_config"></a>2.1 配置远程访问基础结构  
若要配置远程访问群集，必须在将形成群集一部分的每个服务器上配置服务器拓扑、IP 寻址、路由和转发。  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>配置远程访问基础结构  
  
1.  使用与第一个远程访问服务器相同的拓扑配置将位于群集中的每台服务器。  
  
2.  根据第一台远程访问服务器的配置，使用相应的 IP 寻址、路由和转发配置将位于群集中的每台服务器。 请注意，群集中的所有服务器必须连接到同一子网。  
  
3.  将群集中的每个服务器加入到第一个远程访问服务器所在的域中。  
  
## <a name="BKMK_Install"></a>2.2 安装远程访问角色  
若要配置远程访问群集，必须在将形成群集一部分的每台服务器上安装远程访问角色。  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>在 Always On VPN 服务器上安装远程访问角色  
  
1.  在 DirectAccess 服务器上的 "服务器管理器" 控制台的 "**仪表板**" 中，单击 "**添加角色和功能**"。  
  
2.  单击 **“下一步”** 三次以打开服务器角色选择屏幕。  
  
3.  在 "**选择服务器角色**" 对话框中，选择 "**远程访问**"，然后单击 "**下一步**"。  
  
4.  单击 "**下一步**" 三次。  
  
5.  在 "**选择角色服务**" 对话框中，选择 " **DirectAccess 和 VPN （RAS）** "，然后单击 "**添加功能**"。  
  
6.  依次选择 "**路由**"、" **Web 应用程序代理**"、"**添加功能**"，然后单击 "**下一步**"。  
  
7. 单击 **“下一步”** ，然后单击 **“安装”** 。  
  
8.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
9.  在要作为群集成员的所有服务器上重复此过程。  
  
## <a name="BKMK_NLB"></a>2.3 安装 NLB  
若要配置远程访问群集，必须在将形成群集一部分的每台服务器上安装网络负载平衡功能。  
  
> [!NOTE]  
> 如果使用外部负载均衡器，则不需要执行此步骤。  
  
#### <a name="to-install-the-nlb-role"></a>安装 NLB 角色  
  
1.  在 DirectAccess 服务器上的 "服务器管理器" 控制台的 "**仪表板**" 中，单击 "**添加角色和功能**"。  
  
2.  单击 "**下一步**" 四次以转到服务器功能选择屏幕。  
  
3.  在 "**选择功能**" 对话框中，依次选择 "**网络负载平衡**"、"**添加功能**"、"**下一步**" 和 "**安装**"。  
  
4.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
5.  在要作为群集成员的所有服务器上重复此过程。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 3：配置负载平衡群集 @ no__t-0  
  



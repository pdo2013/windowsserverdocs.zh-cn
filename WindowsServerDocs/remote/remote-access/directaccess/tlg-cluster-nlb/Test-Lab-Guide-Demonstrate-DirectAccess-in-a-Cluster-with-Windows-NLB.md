---
title: 测试实验室指南-使用 Windows NLB 在群集中演示 DirectAccess
description: 本主题是测试实验室指南的一部分-使用 windows Server 2016 的 Windows NLB 在群集中演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0c82f9f56ea680c11cd612e17326fe7cf96aeca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388427"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>测试实验指南：使用 Windows NLB 在群集中演示 DirectAccess

>适用于：Windows Server（半年频道）、Windows Server 2016

远程访问是 Windows Server 2016 中的一种服务器角色，Windows Server 2012 R2 和 microsoft Server 2012 操作系统，使远程用户可以使用 DirectAccess 或 RRAS VPN 安全访问内部网络资源。 本指南包含扩展 @no__t 实验室指南的分步说明：演示具有混合 IPv4 和 IPv6 @ no__t 的 DirectAccess 单服务器安装程序，以演示 DirectAccess 网络负载平衡和群集配置。  
  
## <a name="about-this-guide"></a>关于本指南  
本指南包含使用六个服务器和两个客户端计算机配置和演示远程访问的指导。 使用 NLB 完成的远程访问测试实验室模拟内网、Internet 和家庭网络，并演示远程访问在不同 Internet 连接方案中的功能。  
  
> [!IMPORTANT]  
> 此实验室是一个使用最小量计算机的概述证明。 本指南中详细介绍的配置仅适用于测试实验室目的，不可用于生产环境。  
  
## <a name="KnownIssues"></a>已知问题  
下面是配置群集方案时的已知问题：  
  
-   使用单个网络适配器在仅使用 IPv4 的部署中配置 DirectAccess，并在网络适配器上自动配置默认的 DNS64（包含“:3333::”的 IPv6 地址）后，尝试通过远程访问管理控制台启用负载平衡会导致出现要求用户提供 IPv6 DIP 的提示。 如果提供的是 IPv6 DIP，则单击“提交”后配置将失败，并出现以下错误：参数不正确。  
  
    解决此问题：  
  
    1.  从 [备份和还原远程访问配置](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)中下载备份和还原脚本。  
  
    2.  使用下载的脚本 Backup-RemoteAccess.ps1 备份远程访问 GPO  
  
    3.  尝试启用负载平衡，直至达到失败的步骤。 在“启用负载平衡”对话框中，展开详细信息区域，在详细信息区域中右键单击，然后单击“复制脚本”。  
  
    4.  打开记事本，然后粘贴剪贴板的内容。 例如：  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    5.  关闭处于打开状态的所有“远程访问”对话框，并关闭远程访问管理控制台。  
  
    6.  编辑所粘贴的文本并删除 IPv6 地址。 例如：  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    7.  在提升的 PowerShell 窗口中，运行之前步骤的命令。  
  
    8.  如果在运行时该 cmdlet 失败（并非由于输入值错误引起），请运行 Restore-RemoteAccess.ps1 命令，并按照说明进行操作以确保维持原始配置的完整性。  
  
    9. 现在可以重新打开远程访问管理控制台。  
  



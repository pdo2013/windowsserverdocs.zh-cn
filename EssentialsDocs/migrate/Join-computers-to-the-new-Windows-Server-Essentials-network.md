---
title: "新的 Windows Server Essentials 网络 1 到加入计算机"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c6abc11ba2ce8a9f1d32c6a884db6332586de78b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>新的 Windows Server Essentials 网络 1 到加入计算机

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 迁移进程中的下一步是加入到新的 Windows Server Essentials 网络的客户端计算机和更新组策略设置。  
  
### <a name="domain-joined-client-computers"></a>已加入域的客户端计算机  
 浏览到**http://***目标服务器名***/ 连接**并安装 Windows Server 连接器软件，因为如果这是一台新电脑。 在安装过程均为已加入域，或者未加入域的客户端计算机一致。  
  
> [!NOTE]
>  Windows Server 连接器软件不支持运行 Windows XP 或 Windows Vista 的计算机。 如果你有已加入域的计算机运行的 Windows XP 或 Windows Vista，你可以跳过此步骤。  
  
### <a name="non-domain-joined-client-computers"></a>非-已加入域的客户端计算机  
 浏览到**http://***目标服务器名***/ 连接**并安装 Windows Server 连接器软件，因为如果这是一台新电脑。 在安装过程均将一致加入域或连接非域的客户端计算机。  
  
> [!NOTE]
>  Windows Server 连接器软件不支持运行 Windows XP 或 Windows Vista 的计算机。 如果你有已加入域的计算机运行的 Windows XP 或 Windows Vista，你可以跳过此步骤。  
  
### <a name="ensure-that-group-policy-has-updated"></a>确保已更新组策略  
  
> [!NOTE]
>  这是可选的步骤，并且仅所需如果源服务器配置使用自定义的组策略设置文件夹重定向等。  
  
 源服务器和目的地服务器仍联机时，你应确保组策略设置已复制到客户端计算机目标服务器。 每个客户端计算机上执行以下步骤：  
  
1.  打开 Command Prompt 窗口。  
  
2.  在命令提示符下，键入**GPRESULT /R**，然后按 ENTER。  
  
3.  查看组策略应用从部分的生成输出:，并确保它列出目标服务器上，如**DestinationSrv.Domain.local**。 例如：  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  如果未列出的目标服务器，在命令提示符下，键入**gpupdate /force**，然后按 ENTER 以刷新组策略设置。 然后再次执行上述步骤。  
  
5.  如果仍未显示目标服务器，这可能是中的组策略设置的错误或将它们应用于此特定的客户端计算机中的错误。 如果未显示目标服务器，请执行以下步骤：  
  
    1.  单击**开始**，单击**运行**，类型**rsop.msc**（设置策略的结果），然后按 ENTER。  
  
    2.  展开与它 X 树，直到你访问了节点。  
  
    3.  右键单击该节点，然后单击**查看错误**组策略设置失败列出的计算机上有关为什么的信息。

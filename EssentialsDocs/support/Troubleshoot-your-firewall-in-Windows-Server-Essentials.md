---
title: 使用 Windows Server Essentials 排除防火墙故障
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 11372589528fcc78e0053bc7002449b53cb3181d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436043"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>使用 Windows Server Essentials 排除防火墙故障
 
>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 如果遇到远程访问问题，请运行修复随处访问向导。  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>运行修复随处访问向导  
  
1. 打开“仪表板”。  
  
2. 单击“设置”  、单击“随处访问”  选项卡，然后单击“修复”  。  
  
3. 按照“修复随处访问向导”中的说明操作。  
  
   如果你使用的是高级网络设置或非 Microsoft 防火墙，则可能需要打开防火墙上的其他端口。 下表中的端口通过 Internet 编号分配机构 (IANA) 进行注册。  
  
|端口号|描述|  
|-----------------|-----------------|  
|65500|证书 Web 服务|  
|65510 和 65515|客户端计算机部署网站|  
|65520|适用于 Mac 客户端计算机的 Web 服务|  
|65532|用于服务器环回通信的提供程序框架|  
|6602|用于服务器和客户端计算机间通信的提供程序框架|  
  
## <a name="see-also"></a>请参阅  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理远程 Web 访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理随处访问](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支持 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)


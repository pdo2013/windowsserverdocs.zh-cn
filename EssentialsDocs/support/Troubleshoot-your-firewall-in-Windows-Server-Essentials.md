---
title: "解决了在 Windows Server Essentials 防火墙"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>解决了在 Windows Server Essentials 防火墙
 
>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 如果你遇到问题远程访问，运行修复任何地方访问向导。  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>若要运行修复任何地方访问向导  
  
1.  打开的面板。  
  
2.  单击**设置**，单击**任何地方访问**选项卡，然后单击**修复**。  
  
3.  按照修复任何地方访问向导中的说明进行操作。  
  
 如果你使用的高级的网络设置，或使用非 Microsoft 防火墙，你可能需要打开其他防火墙端口。 下表中的端口注册与 Internet 分配数字机构 (IANA)。  
  
|端口号|描述|  
|-----------------|-----------------|  
|65500|证书 web 服务|  
|65510 和 65515|客户端计算机部署网站|  
|65520|Mac 客户端计算机的 web 服务|  
|65532|提供商的服务器回送通信的框架|  
|6602|提供商的服务器和客户端计算机之间进行通信的框架|  
  
## <a name="see-also"></a>请参阅  
  
-   [使用远程 Web 访问](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Web 远程访问](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [管理任意位置的访问权限](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [支持 Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [支持 Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)


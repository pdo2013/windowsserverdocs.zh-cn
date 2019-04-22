---
title: 步骤 5：启用文件夹重定向目标服务器以进行 Windows Server Essentials 迁移
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 613ff4c80a80ed4f3207cb0c1ead6db12c723e85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815378"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>步骤 5：启用文件夹重定向目标服务器以进行 Windows Server Essentials 迁移

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如果在源服务器上启用了文件夹重定向，则可以在目标服务器上也启用同样功能，然后删除旧的“文件夹重定向组策略”设置。  
  
 首先，使用 Windows Server Essentials 仪表板启用在目标服务器上的文件夹重定向。 然后，删除旧的文件夹重定向组策略设置。  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>在目标服务器上启用文件夹重定向  
  
1.  在目标服务器上打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击“设备” 。  
  
3.  在“设备任务”窗格中，单击“实现组策略”。  
  
4.  在“启用文件夹重定向组策略”页上，选择要重定向的文件夹，然后单击“下一步”。  
  
5.  在“启用安全策略设置”  页上，单击“完成” 。  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>删除旧的文件夹重定向组策略设置  
  
1.  在目标服务器上，打开“组策略管理”管理工具。  
  
2.  在中**组策略管理**，展开 **林: * * * YourNetworkDomainName*，展开**域**，展开*YourNetworkDomainName*然后展开**组策略对象**。  
  
3.  右键单击要删除的策略，然后单击“删除” 。  
  
4.  阅读警告，然后单击“是” 。  
  
5.  关闭“组策略管理”。  
  
 若要将更改应用于文件夹重定向，则网络用户必须注销其计算机，然后再重新登录。 这可确保所有重定向的文件夹都传输到目标服务器。  
  
## <a name="next-steps"></a>后续步骤  
 已在目标服务器上启用文件夹重定向。 现在，转到[步骤 6:降级和删除源服务器从新的 Windows Server Essentials 网络](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。


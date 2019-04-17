---
title: "第 5 步： 启用文件夹重定向，Windows Server Essentials 的目标 Server 迁移"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>第 5 步： 启用文件夹重定向，Windows Server Essentials 的目标 Server 迁移

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如果源服务器上启用了文件夹重定向，你可以启用目的地服务器上的文件夹重定向和然后再删除旧的文件夹重定向组策略设置。  
  
 首先，使用 Windows Server Essentials 仪表板启用文件夹重定向目的地服务器上。 然后，将删除旧的文件夹重定向组策略设置。  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>若要启用目标服务器上的文件夹重定向  
  
1.  在目标服务器上，打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击**设备**。  
  
3.  在**设备任务**窗格中，单击**实现组策略**。  
  
4.  在**启用文件夹重定向组策略**页上，选择以重定向，然后单击文件夹**下一步**。  
  
5.  在**启用安全策略设置**页上，单击**完成**。  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>若要删除旧的文件夹重定向组策略设置  
  
1.  在目标服务器上，打开**组策略管理**管理工具。  
  
2.  在**组策略管理**，展开**森林：***YourNetworkDomainName*，展开**域**，展开*YourNetworkDomainName*，然后展开**组策略对象**。  
  
3.  右键单击你想要删除，然后单击的策略**删除**。  
  
4.  阅读警告，，然后单击**是**。  
  
5.  关闭**组策略管理**。  
  
 以应用更改文件夹重定向，网络用户必须注销计算机，并且然后重新登录。 这将确保将所有重定向的文件夹传送到目标服务器。  
  
## <a name="next-steps"></a>后续步骤  
 你已启用文件夹重定向目的地服务器上。 现在转到[第 6 步： 将降级，从新的 Windows Server Essentials 网络删除源服务器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。


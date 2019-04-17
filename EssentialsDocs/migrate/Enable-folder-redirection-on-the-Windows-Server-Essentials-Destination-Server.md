---
title: "启用 Windows Server Essentials 目标 server 1 上的文件夹重定向"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: "启用 Windows Server Essentials 目标服务器上的文件夹重定向"
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f93d7b28177f96725f2e62c40f9c81cbf186ee6d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>启用 Windows Server Essentials 目标 server 1 上的文件夹重定向

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如果文件夹重定向的启用了源代码服务器上，你可以执行此任务中。  
  
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
  
3.  右键单击**W7PVP 文件夹重定向**，然后单击**删除**。  
  
4.  阅读警告，，然后单击**是**。  
  
5.  关闭**组策略管理**。  
  
 若要适用于文件夹重定向的更改，网络用户必须注销计算机，，然后再重新登录。 这将确保将所有重定向的文件夹传送到目标服务器。

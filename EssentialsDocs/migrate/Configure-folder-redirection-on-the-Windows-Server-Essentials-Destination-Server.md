---
title: 在 Windows Server Essentials 目标服务器上配置文件夹重定向
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7cc1207f36d3a921b49cc3ecd02acf3fe4fa243c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827368"
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>在 Windows Server Essentials 目标服务器上配置文件夹重定向

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

如果在源服务器上启用了文件夹重定向，则执行此任务。  
  
 首先，删除旧的文件夹重定向组策略设置。 然后使用 Windows Server Essentials 仪表板启用在目标服务器上的文件夹重定向。  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>删除旧的文件夹重定向组策略设置  
  
1.  在目标服务器上，打开“组策略管理”管理工具。  
  
2.  在中**组策略管理**，展开 **林:***YourNetworkDomainName*，展开 **域**，展开 *YourNetworkDomainName* 然后展开 **组策略对象** 。  
  
3.  右键单击“SBS 组策略文件夹重定向” ，然后单击“删除” 。  
  
4.  右键单击“SBS 组策略安全模板”，然后单击“删除”。  
  
5.  阅读警告，然后单击“是” 。  
  
6.  关闭“组策略管理”。  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>在目标服务器上启用文件夹重定向  
  
1.  在目标服务器上打开 Windows Server Essentials 仪表板。  
  
2.  在导航栏中，单击“设备” 。  
  
3.  在“设备任务”窗格中，单击“实现组策略”。  
  
4.  在“启用文件夹重定向组策略”页上，选择要重定向的文件夹，然后单击“下一步”。  
  
5.  在“启用安全策略设置”  页上，单击“完成” 。  
  
 若要将更改应用于文件夹重定向，则网络用户必须注销其计算机，然后再重新登录。 这可确保所有重定向的文件夹都传输到目标服务器。

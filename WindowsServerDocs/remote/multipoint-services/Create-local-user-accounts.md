---
title: 创建本地用户帐户
description: 了解 MultiPoint Services 中的 abou thte 三种用户帐户
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: 874d9cbdb56c59693cb5843a898b9ebd6893d674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405110"
---
# <a name="create-local-user-accounts"></a>创建本地用户帐户
可以使用 MultiPoint 管理器在中创建三个级别的本地用户帐户：标准用户帐户;具有有限管理权限的 MultiPoint 仪表板用户;和完全管理用户帐户。  
  
使用以下过程在 MultiPoint 服务器上创建本地用户帐户。 如果你的环境包含多个多点服务器，并且你希望该用户能够登录到任何服务器上的任何工作站，则需要在每个服务器上创建一个本地用户帐户。 该安装程序存在一些限制。 在域环境中，你还可以让用户使用其域帐户。 有关选项的概述，请参阅为[Windows MultiPoint Services 环境规划用户帐户](Plan-user-accounts-for-your-MultiPoint-services-environment.md)。  
   
1.  以管理员身份登录到服务器，然后打开 "MultiPoint 管理器"。  
  
2.  单击 "**用户**" 选项卡，然后单击 "**添加用户帐户**"。  
  
    此时将打开 "添加用户帐户" 向导。  
  
3.  输入新用户帐户的帐户名和密码，然后单击 "**下一步**"。  
  
4.  选择要创建的用户帐户类型：  
  
    -   **标准用户**-可以登录到工作站并执行用户任务，但不能访问 Multipoint 管理器或 Multipoint Server 仪表板，并且无法关闭系统。  
  
    -   **MultiPoint 仪表板用户**-拥有有限的管理权限。 仪表板用户可以打开仪表板并执行任务，如将用户注销系统或关闭 MultiPoint 服务器计算机，但用户不具有对 MultiPoint 管理器的访问权限。  
  
    -   **管理用户**具有 MultiPoint Server 中的完全管理权限。 例如，管理用户可以运行 MultiPoint 管理器，添加和删除用户，修改系统设置和更新驱动程序。  
  
5.  单击 "**下一步**"，然后单击 "**完成**" 以创建用户帐户。
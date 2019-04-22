---
title: 创建本地用户帐户
description: 了解 abou 数值三种类型的 MultiPoint Services 中的用户帐户
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823668"
---
# <a name="create-local-user-accounts"></a>创建本地用户帐户
可以使用 MultiPoint 管理器中创建三个级别的本地用户帐户：标准用户帐户;MultiPoint 仪表板用户，这些用户具有有限的管理权限;和完整的管理用户帐户。  
  
使用以下过程在多点服务器上创建本地用户帐户。 如果您的环境包括多个 MultiPoint server，并且你希望用户能够登录到任何服务器上的任何工作站上，需要在每个服务器上创建本地用户帐户。 安装程序具有一些限制。 在域环境中，您还可以让用户使用其域帐户。 有关选项的概述，请参阅[计划在 Windows MultiPoint Services 环境的用户帐户](Plan-user-accounts-for-your-MultiPoint-services-environment.md)。  
   
1.  登录到服务器以管理员身份，并打开 MultiPoint 管理器。  
  
2.  单击**用户**选项卡，然后依次**将用户帐户添加**。  
  
    添加用户帐户向导将打开。  
  
3.  对于新的用户帐户，输入帐户名和密码，然后单击**下一步**。  
  
4.  选择你想要创建的用户帐户的类型：  
  
    -   **标准用户**-可以登录到工作站上以及执行用户任务，但不具有访问权限 MultiPoint 管理器或 MultiPoint Server 仪表板，并不能关闭系统。  
  
    -   **MultiPoint 仪表板用户**的有限管理权限。 仪表板用户可以打开仪表板并执行任务，例如日志记录系统注销用户或关闭 MultiPoint Server 计算机，但用户没有对 MultiPoint 管理器的访问。  
  
    -   **管理用户**具有 MultiPoint Server 中的完全管理权限。 例如，管理用户可以运行 MultiPoint 管理器、 添加和删除用户、 修改系统设置和更新驱动程序。  
  
5.  单击**下一步**，然后单击**完成**创建用户帐户。
---
title: 启用或禁用磁盘保护
description: 了解如何对 MultiPoint 服务使用磁盘保护
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d037b0843f5ba50c98e0d6e7cb10836c8d6fa23a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871747"
---
# <a name="enable-or-disable-disk-protection"></a>启用或禁用磁盘保护
磁盘保护功能允许你在每次重启系统时将你的 MultiPoint Services 系统重置为指定状态。 使用磁盘保护，用户可以临时对 MultiPoint Services 系统进行更改，然后在重启服务器时，这些更改将被丢弃。 服务器重新启动时将放弃的更改的示例包括个性化用户的配置文件、保存文件、更改设置或安装应用程序。  
  
## <a name="enable-disk-protection"></a>启用磁盘保护  
  
1.  在 MultiPoint 管理器中，单击 "**主页**" 选项卡，然后在 "计算机名称 ***任务**" 下，单击 "**启用磁盘保护**"。  
  
2.  查看信息，然后单击“确定”。  
  
系统重启后，对系统所作的任何更改（包括新安装的应用程序）将在每次后续重启时被丢弃。  
  
## <a name="disable-disk-protection"></a>禁用磁盘保护  
  
1.  在 MultiPoint 管理器中，单击 "**主页**" 选项卡，然后在 "计算机名称 ***任务**" 下，单击 "**禁用磁盘保护**"。  
  
2.  查看信息，然后单击“确定”。  
  
在系统重启后，对系统所做的任何更改（包括在服务器上安装的应用程序）都是永久性的，并且不会在下次系统重启时被丢弃。  
  

---
title: 切换模式
description: 了解如何在 MultiPoint Services 中切换工作站和控制台模式
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 181324bff3227f7b071b8ae32f756d365b059a74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389243"
---
# <a name="switch-between-modes"></a>切换模式
MultiPoint 管理器包括以下模式，可帮助你执行不同类型的 MultiPoint 服务系统管理：  
  
-   *工作站模式*：默认情况下，MultiPoint 服务系统在工作站模式下启动。 在工作站模式下，MultiPoint 服务工作站行为就如同每个工作站是运行 Windows 的一台单独计算机，并且多个用户可以同时使用该系统。 你和你的用户可以共享文件并执行必要的工作。  
  
-   *控制台模式*：当 MultiPoint 服务系统处于控制台模式时，你可以安装和更新软件和驱动程序，或执行其他维护任务。 当系统处于控制台模式时，*工作站*均不可供其他计算机用户使用。 此类工作站不会显示在 MultiPoint 管理器中。 直接连接到服务器的所有监视器都作为此计算机系统的显示。   
  
> [!NOTE]
> 通过更改服务器设置中的默认值，你可以强制系统在控制台模式下启动。  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>从工作站模式切换到控制台模式  
  
1.  在工作站模式下打开 "MultiPoint 管理器"，然后单击 "**主页**" 选项卡。  
  
2.  在**计算机**列中，单击要更改其模式的计算机。  
  
3.  下*计算机名* **任务**，单击**切换到控制台模式**。 计算机将重启，所有工作站将不可用。  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>从控制台模式切换到工作站模式  
  
1.  在控制台模式下打开 MultiPoint Manager，然后单击 "**主页**" 选项卡。  
  
2.  在**计算机**列中，单击要更改其模式的计算机。  
  
3.  下*计算机名* **任务**，单击**切换到工作站模式**。 计算机将重启，所有工作站将可用。  
  
## <a name="see-also"></a>请参阅  
[使用 MultiPoint 管理器管理系统任务](Manage-System-Tasks-Using-MultiPoint-Manager.md)
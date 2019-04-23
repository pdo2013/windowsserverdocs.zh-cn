---
title: 更改选项卡的顺序和分组
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887758"
---
# <a name="change-the-order-and-grouping-of-tabs"></a>更改选项卡的顺序和分组

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

可以更改仪表板中选项卡的顺序，使你的选项卡成为选项卡行中的第一个选项卡（左侧）。 为此，需要向注册表中添加项。 还可以通过向注册表添加项来影响选项卡的分组。 选项卡可以按以下顺序依次排列：你的主选项卡、Microsoft 内置选项卡、你的任何其他选项卡以及第三方选项卡。  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>更改仪表板中选项卡的顺序  
 为了定义选项卡的顺序，必须在注册表中添加用于显示你的选项卡的加载项标识符。  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>在选项卡列表中将你的选项卡显示为第一个选项卡  
  
1.  在引用计算机上，单击 **“开始”**，输入 **regedit**，然后按 **Enter**。  
  
2.  在左侧窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、 **“SOFTWARE”**、 **“Microsoft”** 和 **“Windows Server”**。 如果 **“OEM”** 项不存在，则你必须完成下列步骤来创建它：  
  
    1.  右键单击 **“Windows Server”**，指向 **“新建”**，然后单击 **“项”**。  
  
    2.  键入 **OEM** 作为该项的名称。  
  
3.  右键单击 **“OEM”**，指向 **“新建”**，然后单击 **“字符串值”**。  
  
4.  输入 **DashboardMainTabID** 作为字符串名称，然后按 **Enter**。  
  
5.  右键单击右侧窗格中的新字符串，然后单击 **“修改”**。  
  
6.  输入为顶级选项卡定义的 GUID，然后按 **Enter**。  
  
     有关创建和标识顶级选项卡的详细信息，请参阅 Windows Server 解决方案 SDK 中的 [创建顶级选项卡](https://msdn.microsoft.com/library/gg513957) 。  
  
7.  将更改保存到注册表中。  
  
8.  还必须在标识符列表中包含主顶级选项卡的 GUID，以用于选项卡分组。 若要执行此操作，请执行 **更改仪表板中选项卡的分组**中列出的步骤。  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>更改仪表板中选项卡的分组  
 可以通过将标识符添加到注册表中，确保将你的选项卡分组在一起并包含在 Microsoft 内置选项卡列表中。  
  
#### <a name="to-change-the-grouping-of-tabs"></a>更改选项卡的分组  
  
1.  如果未打开 regedit，请单击 **“开始”**，输入 **regedit**，然后按下 **Enter**。  
  
2.  在左侧窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、 **“SOFTWARE”**、 **“Microsoft”** 和 **“Windows Server”**。  
  
3.  右键单击 **“OEM”**，指向 **“新建”**，然后单击 **“项”**。  
  
4.  输入 **DashboardAddins** 作为项名称，然后按 **Enter**。  
  
5.  右键单击 **“DashboardAddins”**，指向 **“新建”**，然后单击 **“字符串值”**。  
  
6.  输入你的选项卡的 GUID 标识符作为字符串名称。 不需要值。  
  
7.  对你的每个选项卡和子选项卡重复步骤 5 和 6。  
  
8.  保存注册表更改。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)
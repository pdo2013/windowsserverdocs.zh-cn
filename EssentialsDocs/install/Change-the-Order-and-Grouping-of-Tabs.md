---
title: "更改订单和分组的选项卡"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>更改订单和分组的选项卡

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以更改仪表板中的选项卡的顺序，以便你的选项卡中的选项卡行（在左侧）的第一个。 你添加对注册表项来执行此操作。 你还可以通过添加注册表项影响组合在一起的选项卡。 选项卡的顺序可以你主的选项卡跟 Microsoft 内置的选项卡，跟有任何你其他选项卡，然后跟第三方的选项卡。  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>更改的顺序仪表板中的选项卡  
 你必须添加外接程序，其中对注册表定义订单显示您的选项卡的标识符。  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>若要在列表中的选项卡第一次显示您的选项卡  
  
1.  在参考计算机上，单击**开始**，输入**regedit**，然后按**Enter**。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，然后展开**Windows Server**。 如果**OEM**键不存在，则你必须完成以下步骤来创建它：  
  
    1.  右键单击**Windows Server**，指向**新建**，然后单击**键**。  
  
    2.  键入**OEM**键的名称。  
  
3.  右键单击**OEM**，指向**新建**，然后单击**字符串值**。  
  
4.  键入**DashboardMainTabID**字符串的名称，然后按作为**Enter**。  
  
5.  新的字符串右侧窗格中，右键单击，然后单击**修改**。  
  
6.  键入定义顶级选项卡上，GUID，然后按**Enter**。  
  
     有关如何创建和识别顶级选项卡的详细信息，请参阅[创建顶级选项卡](https://msdn.microsoft.com/library/gg513957)Windows Server 解决方案 SDK 中。  
  
7.  将更改保存到注册表中。  
  
8.  你必须还包括在列表中进行分组选项卡的标识符你主的顶级选项卡 GUID。 若要执行此操作，请执行中列出的步骤**仪表板中的选项卡组合在一起的更改**。  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>更改仪表板中的选项卡分组  
 你可以确保您的选项卡是组合在一起，并包含在列表中的内置 Microsoft 选项卡上，通过添加到注册表标识符。  
  
#### <a name="to-change-the-grouping-of-tabs"></a>若要更改的选项卡分组  
  
1.  如果未打开 regedit，请单击**开始**，输入**regedit**，然后按**Enter**。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，然后展开**Windows Server**。  
  
3.  右键单击**OEM**，指向**新建**，然后单击**键**。  
  
4.  键入**DashboardAddins**作为键名称，然后按**Enter**。  
  
5.  右键单击**DashboardAddins**，指向**新建**，然后单击**字符串值**。  
  
6.  作为字符串名称中键入你的选项卡上的 GUID 标识符。 不需要的值。  
  
7.  对于每个标签页和标签页的子重复步骤 5 和 6。  
  
8.  保存注册表更改。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)
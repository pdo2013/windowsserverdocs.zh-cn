---
title: 创建和管理服务器组
description: 服务器管理器
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889478"
---
# <a name="create-and-manage-server-groups"></a>创建和管理服务器组

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何创建在服务器管理器中在 Windows Server 中的自定义用户定义的服务器组。

## <a name="BKMK_groups"></a>服务器组
会显示将添加到服务器池的服务器**的所有服务器**页在服务器管理器。 你可以为已添加的服务器创建一个自定义组。 服务器组允许您查看和管理服务器池作为一个逻辑单元; 较小子集例如，可以创建一个名为组**记帐服务器**对于你的组织中的所有服务器的会计部门或组被称为**芝加哥**的地理位置的所有服务器在芝加哥。 创建服务器组后，该组的主页服务器管理器中显示有关事件、 服务、 性能计数器、 最佳做法分析器结果信息，并作为一个整体安装角色和功能组。

服务器可同时为多个组的成员。

#### <a name="to-create-a-new-server-group"></a>创建新的服务器组

1.  上**管理**菜单上，单击**创建服务器组**。

2.  在“服务器组名称”  文本框中，为你的服务器组键入一个友好名称，如“记帐服务器” 。

3.  将服务器添加到**所选**从服务器池中，列表中或使用其他服务器添加到组**active directory**， **DNS**，或**导入**选项卡。 有关如何使用这些选项卡的详细信息，请参阅[将服务器添加到服务器管理器](add-servers-to-server-manager.md)本指南中。

4.  向组添加完服务器时，单击“确定” 。 新组会显示在服务器管理器导航窗格下**的所有服务器**组。

#### <a name="to-edit-an-existing-server-group"></a>编辑现有服务器组的步骤

1.  请执行以下操作之一。

    -   在服务器管理器导航窗格中，右键单击服务器组，然后依次**编辑服务器组**。

    -   在该服务器组的主页上，打开**任务**菜单上的**服务器**磁贴，，然后单击**编辑服务器组**。

2.  更改组名称，或添加或从组中删除服务器。

    > [!NOTE]
    > 从服务器组删除服务器不会删除服务器从服务器管理器。 从组中删除的服务器仍会保留在服务器池的“所有服务器”组中。

3.  对组作完更改时，单击“确定” 。

#### <a name="to-delete-an-existing-server-group"></a>删除现有服务器组的步骤

1.  请执行以下操作之一。

    -   在服务器管理器导航窗格中，右键单击服务器组，然后依次**删除服务器组**。

    -   在该服务器组的主页上，打开**任务**菜单上的**服务器**磁贴，，然后单击**删除服务器组**。

2.  当系统提示你确定是否要删除该服务器组时，单击“是” 。

    > [!NOTE]
    > 删除服务器组不会删除服务器从服务器管理器。 已删除的组中的服务器仍会保留在服务器池的“所有服务器”  组中。

3.  对组作完更改时，单击“确定” 。

## <a name="see-also"></a>请参阅
[将服务器添加到服务器管理器](add-servers-to-server-manager.md)
[服务器管理器](server-manager.md)




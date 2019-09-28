---
title: 创建和管理服务器组
description: 服务器管理器
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 74075f091cd707f6faf73567c1dce6f22a2f6753
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383216"
---
# <a name="create-and-manage-server-groups"></a>创建和管理服务器组

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

本主题介绍如何在 Windows Server 中的服务器管理器中创建自定义的用户定义服务器组。

## <a name="BKMK_groups"></a>服务器组
添加到服务器池的服务器显示在服务器管理器中的 "**所有服务器**" 页上。 你可以为已添加的服务器创建一个自定义组。 服务器组允许你将服务器池的一个较小子集作为逻辑单元查看和管理;例如，你可以为组织会计部门的所有服务器创建一个名为 "**记帐服务器**" 的组，或为地理位置在芝加哥的所有服务器创建一个名为 "**芝加哥**" 的组。 创建服务器组后，服务器管理器中的组主页会显示有关事件、服务、性能计数器、最佳做法分析器结果以及整个组的已安装角色和功能的信息。

服务器可同时为多个组的成员。

#### <a name="to-create-a-new-server-group"></a>创建新的服务器组

1.  在 "**管理**" 菜单上，单击 "**创建服务器组**"。

2.  在“服务器组名称” 文本框中，为你的服务器组键入一个友好名称，如“记帐服务器”。

3.  使用 " **active directory**"、" **DNS**" 或 "**导入**" 选项卡将服务器添加到**所选**列表中的服务器池，或者将其他服务器添加到组。 有关如何使用这些选项卡的详细信息，请参阅本指南中的[将服务器添加到服务器管理器](add-servers-to-server-manager.md)。

4.  向组添加完服务器时，单击“确定”。 新组显示在 "**所有服务器**" 组下的服务器管理器导航窗格中。

#### <a name="to-edit-an-existing-server-group"></a>编辑现有服务器组的步骤

1.  请执行以下操作之一。

    -   在服务器管理器导航窗格中，右键单击服务器组，然后单击 "**编辑服务器组**"。

    -   在服务器组的主页上，打开 "**服务器**" 磁贴上的 "**任务**" 菜单，然后单击 "**编辑服务器组**"。

2.  更改组名称，或者在组中添加或删除服务器。

    > [!NOTE]
    > 从服务器组删除服务器不会从服务器管理器中删除服务器。 从组中删除的服务器仍会保留在服务器池的“所有服务器”组中。

3.  对组作完更改时，单击“确定”。

#### <a name="to-delete-an-existing-server-group"></a>删除现有服务器组的步骤

1.  请执行以下操作之一。

    -   在服务器管理器导航窗格中，右键单击服务器组，然后单击 "**删除服务器组**"。

    -   在服务器组的主页上，打开 "**服务器**" 磁贴上的 "**任务**" 菜单，然后单击 "**删除服务器组**"。

2.  当系统提示你确定是否要删除该服务器组时，单击“是”。

    > [!NOTE]
    > 删除服务器组不会从服务器管理器中删除服务器。 已删除的组中的服务器仍会保留在服务器池的“所有服务器” 组中。

3.  对组作完更改时，单击“确定”。

## <a name="see-also"></a>请参阅
[将服务器添加到服务器管理器](add-servers-to-server-manager.md)
[服务器管理器](server-manager.md)




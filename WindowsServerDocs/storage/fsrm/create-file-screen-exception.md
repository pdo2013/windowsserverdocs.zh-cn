---
title: 创建文件屏蔽异常
description: 本文介绍如何创建文件屏蔽异常
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6a0fa660db6b03104b585c8ee78a4f20aafe5c88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403150"
---
# <a name="create-a-file-screen-exception"></a>创建文件屏蔽异常

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

有时候，你需要允许文件屏蔽异常。 例如，你可能希望阻止来自某文件服务器的视频文件，但又需要允许培训小组保存其计算机相关培训的视频文件。 若要允许访问其他文件屏蔽所阻止的文件，请创建*文件屏蔽异常*。

文件屏蔽异常是一种特殊的文件屏蔽类型，可替代应用于指定屏蔽路径下的文件夹及其所有子文件夹的任何文件屏蔽。 也就是说，文件屏蔽异常会针对派生自父文件夹的任何规则创建异常。

> [!Note]
> 你无法在早已定义文件屏蔽的父文件夹上创建文件屏蔽异常。 你必须将异常分配至一个子文件夹，或者更改现有的文件屏蔽。

<br />
你可以分配文件组以确定文件屏蔽异常中将允许哪些文件类型。

## <a name="to-create-a-file-screen-exception"></a>创建文件屏蔽异常

1.  在**文件屏蔽管理**中，单击**文件屏蔽**节点。

2.  右键单击**文件屏蔽**，然后单击**创建文件屏蔽异常**（或者从**操作**窗格中选择**创建文件屏蔽异常**）。 此操作将打开**创建文件屏蔽异常**对话框。

3.  在**异常路径**文本框中，键入或选择异常将应用到的路径。 异常将应用于所选文件夹及其所有子文件夹。

4.  若要指定从文件屏蔽中排除的文件：

    -   在**文件组**下，选择希望从文件屏蔽中排除的所有文件组。 （若要选中文件组复选框，请双击文件组标签。）
    -   若要查看文件组包含和排除的文件类型，请单击 "文件组" 标签，然后单击 " **编辑**"。
    -   若要新建文件组，请单击**创建**。

5.  单击 **“确定”** 。

## <a name="see-also"></a>请参阅

-   [文件屏蔽管理](file-screening-management.md)
-   [定义用于屏蔽的文件组](define-file-groups-for-screening.md)



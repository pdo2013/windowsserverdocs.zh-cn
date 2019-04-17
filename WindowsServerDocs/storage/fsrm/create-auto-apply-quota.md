---
title: "创建自动应用配额"
description: "本文介绍如何基于配额模板创建自动应用配额"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2837df448434252470d783a6c06f0690ba09021
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-auto-apply-quota"></a>创建自动应用配额

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

通过使用自动应用配额，可以将配额模板分配至父卷或父文件夹。 然后，文件服务器资源管理器会基于该模板自动生成配额。 可为每个现有子文件夹和将来创建的子文件夹生成配额。

例如，你可以针对漫游配置文件用户或新用户，定义按需创建的子文件夹的自动应用配额。 每次创建子文件夹时均可使用父文件夹中的模板自动生成新的配额项。 这些自动生成的配额项将作为独立的项，可在**配额**节点下进行查看。 可以单独保留每个配额项。

## <a name="to-create-an-auto-apply-quota"></a>若要创建自动应用配额，请执行以下操作：

1.  单击**配额管理**中的**配额**节点。

2.  右键单击**配额**，然后单击**创建配额**（或从**操作**窗格中选择**创建配额**）。 此操作将打开**创建配额**对话框。

3.  在**配额路径**下，键入配额配置文件将应用到的父文件夹名称，或者浏览至该父文件夹。 自动应用配额将应用于该文件夹中的各个（当前和未来的）子文件夹。

4.  单击**自动应用模板以创建现有子文件夹和新子文件夹的配额**。

5.  在**从此配额模板派生属性**下，从下拉列表中选择要应用的配额模板。 请注意，**配额属性摘要**下会显示每个模板的属性。

6.  单击**创建**。

> [!Note]
> 选择**配额**节点，然后选择**刷新**，即可验证所有自动生成的配额。 每个子文件夹的单个配额和父卷或父文件夹中的自动应用配额配置文件均会列出。

## <a name="see-also"></a>另请参阅

-   [配额管理](quota-management.md)
-   [编辑自动应用配额属性](edit-auto-apply-quota-properties.md)
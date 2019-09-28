---
title: 编辑自动应用配额属性
description: 本文介绍了如何编辑自动应用配额属性
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4b4fda5cdfeed8df02fee922c8dc5fddc75c56ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403118"
---
# <a name="edit-auto-apply-quota-properties"></a>编辑自动应用配额属性

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

更改自动应用配额时，可以选择将这些更改扩展至自动应用配额路径中的现有配额。 无论在创建配额后对配额进行了哪些修改，都可以选择仅修改仍与原始自动应用配额匹配的配额或修改自动应用配额路径中的所有配额。 此功能通过提供一个可进行所有更改的中心点，简化了更新从自动应用配额派生的配额的属性的过程。

> [!Note]
> 如果选择将更改应用于自动应用配额路径中的所有配额，将覆盖已创建的任何自定义配额属性。

## <a name="to-edit-an-auto-apply-quota"></a>编辑自动应用配额

1.  在**配额**中，选择希望修改的自动应用配额。 可以筛选配额以仅显示自动应用配额。

2.  右键单击配额项，然后单击**编辑配额属性**（或者在**操作**窗格中，在**所选配额**下选择**编辑配额属性**）。 此操作将打开**编辑自动应用配额**对话框。

3.  在**从此配额模板派生属性**下，选择希望应用的配额模板。 可以在摘要列表框中查看每个配额模板的属性。

4.  单击 **“确定”** 。 此操作将打开**更新从自动应用配额派生的配额**对话框。

5.  选择希望应用的更新类型：

    -   如果拥有在自动生成后已经过修改的配额，并且不希望更改这些配额，请选择**仅将自动应用配额应用于与原始自动应用配额匹配的派生配额**。 此选项将仅更新自动应用配额路径中在自动生成后未经过编辑的配额。
    -   如果希望修改自动应用配额路径中的所有现有配额，请选择**将自动应用配额应用于所有派生配额**。
    -   如果希望保持现有配额不变，但修改自动应用配额路径中新子文件夹中的自动应用配额，请选择**不要将自动应用配额应用于派生配额**。

6.  单击 **“确定”** 。

## <a name="see-also"></a>请参阅

-   [配额管理](quota-management.md)
-   [创建自动应用配额](create-auto-apply-quota.md)



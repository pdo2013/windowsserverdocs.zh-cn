---
title: "编辑文件屏蔽模板属性"
description: "本文介绍了如何编辑文件屏蔽模板属性"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 31ca46707a32d23a5dd9606c57bcaec5d6e53a80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="edit-file-screen-template-properties"></a>编辑文件屏蔽模板属性

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

更改文件屏蔽模板时，可以选择将这些更改扩展至使用原始文件屏蔽模板创建的文件屏蔽。 无论在创建文件屏蔽后对文件屏蔽进行了哪些修改，都可以选择仅修改与原始模板匹配的文件屏蔽或修改从原始模板派生的所有文件屏蔽。 此功能通过提供一个可进行所有更改的中心点，简化了更新文件屏蔽属性的过程。

> [!Note]
> 如果将更改应用于从原始模板派生的所有文件屏蔽，将覆盖创建的任何自定义文件屏蔽属性。

## <a name="to-edit-file-screen-template-properties"></a>编辑文件屏蔽模板属性

1.  在**文件屏蔽模板**中，选择希望修改的模板。

2.  右键单击文件屏蔽模板，然后单击**编辑模板属性**（或者在**操作**窗格中，在**所选文件屏蔽模板**下选择**编辑模板属性**。）此操作将打开**文件屏蔽模板属性**对话框。

3.  如果要复制其他模板的属性以用作修改后模板的基础，请从**从模板复制属性**下拉列表中选择模板。 然后单击**复制**。

4.  执行所有必需的更改。 设置和通知选项与创建文件屏蔽模板时的可用选项相同。

5.  完成编辑模板属性后，单击**确定**。 此操作将打开**更新从模板派生的文件屏蔽**对话框。

6.  选择希望应用的更新类型：

    -   如果拥有在使用原始模板创建后已经过修改的文件屏蔽，并且不希望更改这些文件屏蔽，请单击**仅将模板应用于与原始模板匹配的派生文件屏蔽**。 此选项将仅更新在使用原始模板属性创建后未经过编辑的文件屏蔽。
    -   如果希望修改使用原始模板创建的所有现有文件屏蔽，请单击**将模板应用于所有派生的文件屏蔽**。
    -   如果希望保持现有文件屏蔽不变，请单击**不要将模板应用于派生文件屏蔽**。

7.  单击**确定**。

## <a name="see-also"></a>另请参阅

-   [文件屏蔽管理](file-screening-management.md)
-   [创建文件屏蔽模板](create-file-screen-template.md)



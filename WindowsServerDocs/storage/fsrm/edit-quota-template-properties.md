---
title: 编辑配额模板属性
description: 本文介绍了如何编辑配额模板属性以将更改扩展至使用原始配额模板创建的配额
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 37719656e107869b97045af98c1a63744e4f6b38
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403025"
---
# <a name="edit-quota-template-properties"></a>编辑配额模板属性

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

更改配额模板时，可以选择将这些更改扩展至使用原始配额模板创建的配额。 无论在创建配额后对配额进行了哪些修改，都可以选择仅修改仍与原始模板匹配的配额或修改从原始模板派生的所有配额。 此功能通过提供一个可进行所有更改的中心点，简化了更新配额属性的过程。

> [!Note]
> 如果选择将更改应用于从原始模板派生的所有配额，将覆盖创建的任何自定义配额属性。

## <a name="to-edit-quota-template-properties"></a>编辑配额模板属性

1.  在**配额模板**中，选择希望修改的模板。

2.  右键单击配额模板，然后单击**编辑模板属性**（或者在**操作**窗格中，在**所选配额模板**下选择**编辑模板属性**）。 此操作将打开**配额模板属性**对话框。

3.  执行所有必需的更改。 设置和通知选项与创建配额模板时设置的选项相同。 根据需要，可以复制其他模板的属性并进行修改。

4.  完成编辑模板属性后，单击**确定**。 此操作将打开**更新从模板派生的配额**对话框。

5.  选择希望应用的更新类型：

    -   如果拥有在使用原始模板创建后已经过修改的配额，并且不希望更改这些配额，请单击**仅将模板应用于与原始模板匹配的派生配额**。 此选项将仅更新在使用原始模板创建后未经过编辑的配额。
    -   如果希望修改使用原始模板创建的所有现有配额，请选择**将模板应用于所有派生的配额**。
    -   如果希望保持现有配额不变，请选择**不要将模板应用于派生配额**。

6.  单击 **“确定”** 。

## <a name="see-also"></a>请参阅

-   [配额管理](quota-management.md)
-   [创建配额模板](create-quota-template.md)



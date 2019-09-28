---
title: 定义用于屏蔽的文件组
description: 本文介绍了如何定义文件组以为文件屏蔽、文件屏蔽异常或按文件组分类的文件存储报告创建命名空间
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b56d7b0439e3dc6f1a2e0a1c96f761dbb77cb0a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394385"
---
# <a name="define-file-groups-for-screening"></a>定义用于屏蔽的文件组

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

*文件组*用于为文件屏蔽、文件屏蔽异常或**按文件组分类的文件**存储报告定义命名空间。 文件组包含一组文件名模式，并按以下分组：

-   **要包含的文件**：归入文件组的文件
-   **要排除的文件**：不属于文件组的文件

> [!Note]
> 为方便起见，可以在编辑文件屏蔽、文件屏蔽异常、文件屏蔽模板和**按文件组分类的文件**报告的属性时创建和编辑文件组。 从这些属性表中进行的任何文件组更改不限于所操作的当前项。

## <a name="to-create-a-file-group"></a>创建文件组

1.  在**文件屏蔽管理**中，单击**文件组**节点。

2.  在**操作**窗格中，单击**创建文件组**。 此操作将打开**创建文件组属性**对话框。

    （或者，可以在编辑文件屏蔽、文件屏蔽异常、文件屏蔽模板或**按文件组分类的文件**报告的属性时，在**维护文件组**下单击**创建**。）

3.  在**创建文件组属性**对话框中，键入文件组名称。

4.  添加要包含的文件和要排除的文件：

    -   对于希望包含在文件组中的每一组文件，请在**要包含的文件**框中输入文件名模式，然后单击**添加**。
    -   对于希望从文件组中排除的每一组文件，请在**要排除的文件**框中输入文件名模式，然后单击**添加**。
        请注意，标准通配符规则适用，例如 **\*** 选择所有可执行文件。

5.  单击 **“确定”** 。

## <a name="see-also"></a>请参阅

-   [文件屏蔽管理](file-screening-management.md)
-   [创建文件屏蔽](create-file-screen.md)
-   [创建文件屏蔽例外](create-file-screen-exception.md)
-   [创建文件屏蔽模板](create-file-screen-template.md)
-   [存储报告管理](storage-reports-management.md)



---
title: 创建分类属性
description: 本文介绍用于为指定文件夹或卷中的文件分配值的分类属性。
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d330f896c71cced8e97701af2c1008b3531e065d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394221"
---
# <a name="create-a-classification-property"></a>创建分类属性

> 适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

分类属性用于为指定文件夹或卷中的文件分配值。 有多种属性类型可选，具体取决于你的需求。 下表定义了可选的属性类型。

|属性 | 描述 |
| --- | --- |
| 是/否 | 布尔属性可设置为**是**或**否**。 分类过程中组合多个值或从文件内容中组合多个值时，**是**将覆盖**否**。 |
| 日期和时间 | 简单的日期/时间属性。 分类过程中组合多个值或从文件内容中组合多个值时，冲突的值将阻止重新分类。 |
| 编号 | 简单的数字属性。 分类过程中组合多个值或从文件内容中组合多个值时，冲突的值将阻止重新分类。 |
| 经过排序的列表 | 固定值列表。 每次只可以为一个属性分配一个值。 分类过程中组合多个值或从文件内容中组合多个值时，将使用列表中数值最高的值。 |
| 字符串 | 简单的字符串属性。 分类过程中组合多个值或从文件内容中组合多个值时，冲突的值将阻止重新分类。 |
| 多选择 | 可分配给属性的值列表。 每次可以为属性分配多个值。 分类过程中组合多个值或从文件内容中组合多个值时，列表中的每个值都可使用。 |
| 多字符串 | 可分配给属性的字符串列表。 每次可以为属性分配多个值。 分类过程中组合多个值或从文件内容中组合多个值时，列表中的每个值都可使用。 |

<br />

下面的过程会引导你完成创建分类属性的过程。

## <a name="to-create-a-classification-property"></a>若要创建分类属性，请执行以下操作：

1.  单击**分类管理**中的**分类属性**节点。

2.  右键单击**分类属性**，然后单击**新建属性**（或单击**操作**窗格中的**新建属性**）。 此操作将打开**分类属性定义**对话框。

3.  在**属性名称**文本框中键入属性的名称。

4.  在**描述**文本框中添加属性的可选描述。

5.  从**属性类型**下拉菜单的列表中选择属性类型。

6.  单击 **“确定”** 。

## <a name="see-also"></a>请参阅

-   [创建自动分类规则](create-automatic-classification-rule.md)
-   [分类管理](classification-management.md)
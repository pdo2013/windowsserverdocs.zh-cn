---
title: 创建配额
description: 本文介绍如何创建基于模板的配额
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f3c677f5ebf7dda44f4b99a64d0fbf8d2c72b92e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883188"
---
# <a name="create-a-quota"></a>创建配额

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

可以根据模板或自定义属性创建配额。 以下过程介绍如何创建基于模板的配额（推荐）。 如果需要使用自定义属性创建配额，可以将这些属性另存为模板以待之后重新使用。

创建配额的时候，需要选择配额路径，即存储限制会应用到的卷或文件夹。 在既定配额路径中，可以使用模板创建以下类型的配额之一：

-   限制整个卷或文件夹空间的单个配额。
-   自动应用配额，可将配额模板分配至文件夹或卷。 将自动生成基于此模板的配额并应用于所有子文件夹。 有关创建自动应用配额的详细信息，请参阅[创建自动应用配额](create-auto-apply-quota.md)。


> [!Note]
> 如果完全通过模板创建配额，则可以通过更新模板来集中管理配额，而不必分别更新每个配额。 之后可以将更改应用于所有基于已修改模板的配额。 此功能通过提供一个可进行所有更新的中心点，简化存储策略更改的实现。

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>创建基于模板的配额

1.  在“配额管理”中，单击“配额模板”节点。

2.  在“结果”窗格中，选择将用作新配额基础的模板。

3.  右键单击模板，然后单击**根据模板创建配额**（或者从**操作**窗格中选择**根据模板创建配额**。 此操作将打开**创建配额**对话框，并显示配额模板的摘要属性。

4.  在**配额路径**下，键入或浏览至配额将应用到的文件夹。

5.  单击**在路径上创建配额**选项。 请注意，配额属性将应用于整个文件夹。

     > [!Note]
     > 若要创建自动应用配额，请单击**在现有子文件夹和新的子文件夹中自动应用模板并创建配额**选项。 有关自动应用配额的详细信息，请参阅[创建自动应用配额](create-auto-apply-quota.md)

6.  在**从此配额模板派生属性**下，在步骤 2 中用于创建新配额的模板已预选中（或者可以从列表中选择其他模板）。 请注意，模板属性显示在**配额属性摘要**下。

7.  单击“创建”。

## <a name="see-also"></a>请参阅

-   [配额管理](quota-management.md)
-   [创建自动应用配额](create-auto-apply-quota.md)
-   [创建配额模板](create-quota-template.md)



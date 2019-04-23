---
title: 创建自定义文件管理任务
description: 本文介绍如何创建自定义文件管理任务和自定义任务。
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dd52a94657fb73d28b3bc1552a058b7f3ca954ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867258"
---
# <a name="create-a-custom-file-management-task"></a>创建自定义文件管理任务

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

并非始终需要对文件执行过期操作。 借助文件管理任务也可运行自定义命令。

> [!Note]
> 此过程假定你已熟悉文件管理任务，因此仅介绍用于配置自定义设置的**操作**选项卡。

## <a name="to-create-a-custom-task"></a>若要创建自定义任务，请执行以下操作：

1.  单击“文件管理任务”节点。

2.  右键单击**文件管理任务**，然后单击**创建文件管理任务**（或单击**窗格**中的**创建文件管理任务**）。 此操作将打开“创建文件管理任务”对话框。

3.  在“操作”选项卡上，输入下列信息：

    -   “类型”。 从下拉菜单中选择**自定义**。
    -   **可执行文件**。 键入或浏览至文件管理任务处理文件时需运行的命令。 该可执行文件仅可由管理员和系统设置为可写。 如果任何其他用户具有该可执行文件的写入权限，则此权限将无法正常运行。
    -   **命令设置**。 若要配置文件管理作业处理文件时传递至可执行文件的参数，请编辑标记为**参数**的文本框。 若要在文本中插入其他变量，请将光标放置在希望插入变量的文本框内，选择希望插入的变量，然后单击**插入变量**。 方括号中的文本会插入可执行文件可收到的变量信息。 例如，\[源文件路径\]变量插入应处理由可执行文件的文件的名称。 或者，单击**工作目录**按钮以指定自定义可执行文件的位置。
    -   **命令安全**。 配置适用于该可执行文件的安全设置。 默认情况下，此命令作为本地服务运行，这是策略最严格的可用帐户。

4.  单击 **“确定”**。

## <a name="see-also"></a>请参阅

-   [分类管理](classification-management.md)
-   [文件管理任务](file-management-tasks.md)
---
title: "配置存储报告"
description: "本文介绍如何配置存储报告的默认参数"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f62109a8d3ea3e4e6386956789d276f9aa911e80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="configure-storage-reports"></a>配置存储报告

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

你可以配置存储报告的默认参数。 这些默认参数将用于配额或文件屏蔽事件发生期间生成的事件报告。 这些参数还适用于计划报告和按需报告，可覆盖这些报告的默认参数，从而为其定义特定的属性。

> [!Important]
> 若要更改某一类型的报告默认参数，则作出的更改将影响所有事件报告及任何使用默认参数的现有计划报告任务。

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>若要配置存储报告的默认参数，请执行以下操作：

1. 在控制台树中，右键单击**文件服务器资源管理器**，然后单击**配置选项**。 此时将打开**文件服务器资源管理器选项**对话框。

2. 在**存储报告**选项卡下的**配置默认参数**中选择要修改的报告类型。

3. 单击**编辑参数**。

4. 所选报告类型不同，则可供编辑的报告参数不同。 执行所有必要的修改，然后单击**确定**，将修改后的参数保存为此类报告的默认参数。

5.  对要编辑的每一类报告重复步骤 2 至 4。

6. 若要查看所有报告的默认参数列表，请单击**查看报告**。 然后单击**关闭**。

7.  单击**确定**。

## <a name="see-also"></a>另请参阅

-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)
-   [存储报告管理](storage-reports-management.md)
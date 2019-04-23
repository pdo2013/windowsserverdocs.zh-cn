---
title: 文件管理任务
description: 本文介绍了自动执行文件管理任务的过程
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e83d0b79117144d42a0aff748f482f3c181cb300
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874048"
---
# <a name="file-management-tasks"></a>文件管理任务

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

文件管理任务会自动执行在服务器上查找文件子集和应用简单命令的过程。 这些任务可被安排为定期执行以降低重复操作的成本。 将由文件管理任务处理的文件可通过以下任一属性进行定义：

-   Location
-   分类属性
-   创建时间
-   修改时间
-   上次访问时间

文件管理任务还可配置为通知文件所有者将被应用到其文件的任何即将出现的策略。

> [!Note]
> 单个文件管理任务按独立的计划运行。

<br />
本部分包括以下主题：

-   [创建文件过期任务](create-file-expiration-task.md)
-   [创建自定义文件管理任务](create-custom-file-management-task.md)

> [!Note]
> 若要设置电子邮件通知和特定的报告功能，必须首先配置文件服务器资源管理器常规选项。

## <a name="see-also"></a>请参阅

-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)



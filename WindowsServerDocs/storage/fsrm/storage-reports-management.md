---
title: 存储报告管理
description: 本文介绍如何生成、计划和监视存储报告
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c78718508194cec834d248f30459b7e50a32b3b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403071"
---
# <a name="storage-reports-management"></a>存储报告管理

> 适用于：Windows Server 2019，Windows Server 2016，Windows Server （半年频道），Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

在文件服务器资源管理器 Microsoft<sup>®</sup> 管理控制台 (MMC) 管理单元的**存储报告管理**节点上，可执行以下任务：

-   计划定期存储报告以确定磁盘使用趋势。
-   为所有用户或一组所选用户监视尝试保存未授权文件的操作。
-   立即生成存储报告。

例如，你可以：

-   安排将在每个星期天午夜运行的报告，生成包含自前两天以来最近访问的文件的列表。 借助此信息，可以监视周末存储活动，以及计划将对周末在家进行连接的用户产生较少影响的服务器停机时间。
-   随时运行报告以确定服务器上卷中的所有重复文件，从而可以迅速回收磁盘空间而不会丢失任何数据。
-   运行按文件组分类的文件报告以确定不同文件组的存储资源如何进行分段 
-   运行按所有者分类的文件报告以分析各个用户如何使用共享存储资源。

本部分包括以下主题：

-   [计划一组报告](schedule-set-of-reports.md)
-   [按需生成报告](generate-reports-on-demand.md)

> [!Note]
> 若要设置电子邮件通知和特定的报告功能，必须首先配置文件服务器资源管理器常规选项。

## <a name="see-also"></a>请参阅

-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)



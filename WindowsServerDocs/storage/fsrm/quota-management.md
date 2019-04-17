---
title: "配额管理"
description: "本文介绍了如何创建和管理配额"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 60436f12e07b8a3f16312829d53a2885c98f30ed
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="quota-management"></a>配额管理

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

可在文件服务器资源管理器 Microsoft<sup>®</sup> 管理控制台 (MMC) 管理单元的**配额管理**节点上执行以下任务：

-   创建配额以限制卷或文件夹拥有的空间，并在接近或超过配额限制时生成通知。
-   生成自动应用配额，以便应用于卷或文件夹中所有现有子文件夹，及将来创建的任何子文件夹。
-   定义可轻松应用于新卷或文件夹以及可在组织中使用的配额模板。

例如，你可以：

-   为用户的个人服务器文件夹设置 200 兆字节 (MB) 的限额，当存储内容超过 180 MB 时，你和用户将收到电子邮件通知。
-   为组共享文件夹设置可灵活调节的 500 MB 配额。 达到此存储限制时，组中的所有用户将收到存储配额已临时扩展到 520 MB 的电子邮件通知，以便用户可以删除不必要的文件并遵守预设的 500 MB 配额策略。
-   当所使用的临时文件夹达到 2 千兆字节 (GB) 时，系统会发出通知，但不会限制该文件夹的配额，因为在服务器上运行服务需要一定空间。

本部分包括下列主题：

-   [创建配额](create-quota.md)
-   [创建自动应用配额](create-auto-apply-quota.md)
-   [创建配额模板](create-quota-template.md)
-   [编辑配额模板属性](edit-quota-template-properties.md)
-   [编辑自动应用配额属性](edit-auto-apply-quota-properties.md)

> [!Note]
> 若要设置电子邮件通知和报告功能，首先必须配置文件服务器资源管理器常规选项。

## <a name="see-also"></a>另请参阅

-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)



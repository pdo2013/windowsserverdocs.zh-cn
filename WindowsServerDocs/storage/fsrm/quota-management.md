---
title: 配额管理
description: 本文介绍了如何创建和管理配额
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: febcd6ab0744a7fddd024e1f0afdb93711e8939a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829538"
---
# <a name="quota-management"></a>配额管理

> 适用于：Windows 服务器 （半年频道），Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

可在文件服务器资源管理器 Microsoft<sup>®</sup> 管理控制台 (MMC) 管理单元的**配额管理**节点上执行以下任务：

-   创建配额以限制卷或文件夹拥有的空间，并在接近或超过配额限制时生成通知。
-   生成自动应用配额，以便应用于卷或文件夹中所有现有子文件夹，及将来创建的任何子文件夹。
-   定义可轻松应用于新卷或文件夹以及可在组织中使用的配额模板。

例如，你可以：

-   用户的个人服务器文件夹，使用电子邮件通知时已超过 180 MB 存储发送给你和用户限制为 200 兆字节 (MB) 的位置。
-   在组的共享文件夹上设置一个灵活的 500 MB 配额。 达到此存储限制后，通过存储配额已临时扩展到 520 MB，以便用户可以删除不必要的文件并符合预设的 500 MB 配额策略的电子邮件通知组中的所有用户。
-   当所使用的临时文件夹达到 2 千兆字节 (GB) 时，系统会发出通知，但不会限制该文件夹的配额，因为在服务器上运行服务需要一定空间。

本部分包括以下主题：

-   [创建配额](create-quota.md)
-   [创建自动应用配额](create-auto-apply-quota.md)
-   [创建配额模板](create-quota-template.md)
-   [编辑配额模板属性](edit-quota-template-properties.md)
-   [编辑自动应用配额属性](edit-auto-apply-quota-properties.md)

> [!Note]
> 若要设置电子邮件通知和报告功能，首先必须配置文件服务器资源管理器常规选项。

## <a name="see-also"></a>请参阅

-   [设置文件服务器资源管理器选项](setting-file-server-resource-manager-options.md)



---
title: 管理基本卷
description: 本文介绍如何管理基本卷。
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c75d887a6427673319999522b890d523f4276871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870958"
---
# <a name="manage-basic-volumes"></a>管理基本卷

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

基本磁盘是包含主分区、扩展分区或逻辑驱动器的物理磁盘。 基本磁盘上的分区和逻辑驱动器称为基本卷。 你只能在基本磁盘上创建基本卷。

通过将现有主分区和逻辑驱动器扩展到相同磁盘上的相邻未分配连续空间，你可以为现有主分区和逻辑驱动器添加更多空间。 若要扩展基本卷，必须使用 NTFS 文件系统对其进行格式化。 你可以在包含逻辑驱动器的扩展分区中的连续可用空间内扩展该逻辑驱动器。 如果将逻辑驱动器扩展到扩展分区中提供的可用空间之外，那么只要扩展分区后面是连续的未分配空间，扩展分区就会扩大以包含逻辑驱动器。

## <a name="see-also"></a>请参阅

-   [向驱动器分配装入点文件夹路径](assign-a-mount-point-folder-path-to-a-drive.md)
-   [扩展基本卷](extend-a-basic-volume.md)
-   [收缩基本卷](shrink-a-basic-volume.md)
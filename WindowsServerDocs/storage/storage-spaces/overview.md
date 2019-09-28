---
title: 存储空间概述
ms.prod: windows-server
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 5de8b36916624b595d8f2ac9ddb0c000e015ee65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366062"
---
# <a name="storage-spaces-overview"></a>存储空间概述

存储空间是 Windows 和 Windows Server 中的一项技术，可帮助保护你的数据免受驱动器故障影响。 它在概念上类似于在软件中实现的 RAID。 你可以使用存储空间将三个或更多驱动器组合到一个存储池中，然后使用该池中的容量来创建存储空间。 这些数据通常存储数据的额外副本，因此，如果其中一个驱动器发生故障，你仍可以使用完整的数据副本。 如果你的容量不足，只需将更多驱动器添加到存储池。

使用存储空间有四种主要方式：

- **在 WINDOWS 电脑上**-有关详细信息，请参阅[Windows 10 中的存储空间](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10)。
- **在单个服务器上具有所有存储的独立服务器上**-有关详细信息，请参阅在[独立服务器上部署存储空间](deploy-standalone-storage-spaces.md)。
- **在群集服务器上使用存储空间直通，并在每个群集节点中使用直接连接的存储**-有关详细信息，请参阅[存储空间直通概述](storage-spaces-direct-overview.md)。
- **在具有一个或多个共享 sas 存储机箱的群集服务器上保存所有驱动器**-有关详细信息，请参阅[具有共享 SAS 的群集上的存储空间概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11))。


---
title: 存储空间概述
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832908"
---
# <a name="storage-spaces-overview"></a>存储空间概述

存储空间是一种技术在 Windows 和 Windows Server，可帮助保护数据免受驱动器故障。 从概念上讲类似于 RAID，在软件中实现它。 可以使用存储空间分组放入存储池的三个或多个驱动器，然后使用从该池容量创建存储空间。 这些通常存储数据的额外副本，因此如果你的驱动器出现故障时，您仍然可以保持不变的数据副本。 如果容量不足，只需将更多驱动器添加到存储池。

有四个主要的方法可以使用存储空间：

- **在 Windows PC 上**-有关详细信息，请参阅[Windows 10 中的存储空间](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10)。
- **具有一台服务器中的所有存储的独立服务器上**-有关详细信息，请参阅[独立服务器上部署存储空间](deploy-standalone-storage-spaces.md)。
- **与本地直连存储，每个群集节点中使用存储空间直通群集服务器上**-有关详细信息，请参阅[存储空间直通概述](storage-spaces-direct-overview.md)。
- **具有一个或多个共享 SAS 存储机箱持有的所有驱动器的群集服务器上**-有关详细信息，请参阅[具有共享 SAS 概述的群集上的存储空间](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11))。


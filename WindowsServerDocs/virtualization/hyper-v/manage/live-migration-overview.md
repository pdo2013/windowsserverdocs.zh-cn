---
title: 实时迁移概述
description: Windows Server 2016 中提供的实时迁移功能的概述。
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887848"
---
# <a name="live-migration-overview"></a>实时迁移概述

实时迁移是 Windows Server 中的 HYPER-V 功能。  它允许你以透明方式移动正在运行虚拟机从一台 HYPER-V 主机到另一个没有假设停机时间。  实时迁移的主要优点是灵活性;正在运行的虚拟机不受限于单个主机。  这样的操作，例如排出特定主机的虚拟机之前解除授权或对其进行升级。  如果与 Windows 故障转移群集搭配使用，则实时迁移允许创建高度可用和容错容错系统。 

## <a name="related-technologies-and-documentation"></a>相关的技术和文档

实时迁移通常与故障转移群集和 System Center Virtual Machine Manager 等的几个相关技术结合使用。  如果您通过这些技术使用实时迁移，以下是其最新文档的指针：
* [故障转移群集](../../../failover-clustering/failover-clustering-overview.md)(Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

如果你正在使用较旧版本的 Windows Server 或需要在较旧版本的 Windows Server 中引入的功能的详细信息，以下是指向历史文档： 
* [实时迁移](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx)(Windows Server 2008 R2)  
* [实时迁移](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx)(Windows Server 2012 R2) 
* [故障转移群集](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx)(Windows Server 2012 R2)
* [故障转移群集](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx)(Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Windows Server 2016 中的实时迁移

在 Windows Server 2016 中，有实时迁移部署受限制较少。  现在可没有故障转移群集。  其他功能保持不变的实时迁移的先前版本中。  有关配置和使用没有故障转移群集的实时迁移的详细信息： 
* [设置为没有故障转移群集的实时迁移的主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [使用没有故障转移群集的实时迁移移动虚拟机](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)
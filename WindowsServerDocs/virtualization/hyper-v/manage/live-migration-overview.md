---
title: 实时迁移概述
description: 概述 Windows Server 2016 中的实时迁移功能。
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2842435f1bdb0aeb82bbcf1ae02be66e242dc9eb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872050"
---
# <a name="live-migration-overview"></a>实时迁移概述

实时迁移是 Windows Server 中的 Hyper-v 功能。  它可让你以透明方式将运行中的虚拟机从一个 Hyper-v 主机移到另一个主机，而无需停机。  实时迁移的主要优点是灵活性;运行的虚拟机未绑定到一台主机。  这样就可以在解除或升级虚拟机之前排出虚拟机的特定主机。  与 Windows 故障转移群集配对后，实时迁移允许创建高度可用的容错系统。 

## <a name="related-technologies-and-documentation"></a>相关技术和文档

实时迁移通常与一些相关技术（如故障转移群集和 System Center Virtual Machine Manager）结合使用。  如果在通过这些技术使用实时迁移，以下是指向其最新文档的指针：
* [故障转移群集](../../../failover-clustering/failover-clustering-overview.md)（Windows Server 2016） 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/)（系统中心2016） 

如果你使用的是较旧版本的 Windows Server，或需要有关更早版本的 Windows Server 中引入的功能的详细信息，请参阅以下指向历史文档的指针： 
* [实时迁移](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx)（Windows Server 2008 R2）  
* [实时迁移](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx)（Windows Server 2012 R2） 
* [故障转移群集](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx)（Windows Server 2012 R2）
* [故障转移群集](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx)（Windows Server 2008 R2）
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx)（System Center 2012 R2）
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx)（System Center 2008 R2）

## <a name="live-migration-in-windows-server-2016"></a>Windows Server 2016 中的实时迁移

在 Windows Server 2016 中，实时迁移部署的限制更少。  它现在无需故障转移群集即可工作。  其他功能在以前版本的实时迁移中保持不变。  有关在不使用故障转移群集的情况下配置和使用实时迁移的详细信息： 
* [为无故障转移群集的实时迁移设置主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [使用没有故障转移群集的实时迁移来移动虚拟机](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)
---
title: 网络卸载和优化技术
description: 本主题提供的卸载和 Windows Server 2016 中的优化技术的概述，并包括有关这些技术的附加指导的链接。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847848"
---
# <a name="network-offload-and-optimization-technologies"></a>网络卸载和优化技术

在本主题中，我们为您提供 Windows Server 2016 中提供的不同的网络卸载和优化功能的概述，并讨论它们帮助使网络更加高效。 这些技术包括软件仅 (SO) 功能和技术，软件和硬件 (SH) 集成功能和技术，以及硬件仅 （主机） 功能和技术。

Windows Server 2016 中可用的网络功能的三个类别如下： 

1.  [仅软件 (SO) 功能和技术](hpn-software-only-features.md):这些功能作为操作系统的一部分实现的并独立于基础个 NIC。 有时这些功能将需要一些调整的最佳操作的 NIC。 其中的示例包括的 hyper-v 功能，例如 vmQoS、 Acl 和非 HYPER-V 功能，例如 NIC 组合。   

2.  [软件和硬件 (SH) 集成功能和技术](hpn-software-hardware-features.md):这些功能有软件和硬件组件。 该软件联系紧密绑定到所需的功能，若要运行的硬件功能。 其中的示例包括 VMMQ、 VMQ，发送端 IPv4 校验和卸载和 RSS。   

3.  [硬件仅 （主机） 功能和技术](hpn-hardware-only-features.md):这些硬件加速功能提高配合该软件中的网络性能，但不联系紧密的任何软件功能的一部分。 其中的示例包括中断裁决、 流控制和接收方 IPv4 校验和卸载。 

4. [高级属性的 NIC](hpn-nic-advanced-properties.md):你可以管理 Nic 和通过 Windows PowerShell 中使用 NetAdapter cmdlet 的所有功能。  此外可以管理 Nic 和使用网络控制面板 (ncpa.cpl) 的所有功能。 

>[!TIP]
>- 因此功能和技术中提供了所有硬件体系结构，而不考虑 NIC 速度或 NIC 功能。
>
>- 仅当你的网络适配器支持的功能或技术时 SH 和 HO 功能可用。

---
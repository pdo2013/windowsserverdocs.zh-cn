---
title: 与基于 Windows Server 虚拟化的代码完整性保护兼容的硬件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820368"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>与基于 Windows Server 虚拟化的代码完整性保护兼容的硬件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

Windows Server 2016 引入新的基于虚拟化的代码保护来帮助保护物理和虚拟机从修改系统代码的攻击。 若要实现此高保护级别，Microsoft 与协同工作的计算机硬件制造商 （原始设备制造商或 Oem） 以防止恶意写入到系统执行代码。 这种保护可应用于任何系统，并用作构建基块实现的受防护的虚拟机 (Vm) 的 HYPER-V 主机运行状况。 

与任何基于硬件的保护，某些系统可能不会由于问题，例如作为可执行文件或通过实际尝试修改在运行时，可能会导致意外故障，包括数据丢失或蓝色的代码不正确的内存页数标记符合屏幕 （也称为停止错误） 错误。 

若要为兼容并完全支持新的安全功能，Oem 需要实现 UEFI 2.6，在 2016 年 1 月发布中定义的内存地址表。 采用新的 UEFI 标准时间同时，若要防止客户时遇到问题，我们想要提供有关系统和我们测试了此功能集的配置，以及我们知道是不兼容的系统的信息。 

## <a name="non-compatible-systems"></a>非兼容的系统

以下配置已知不兼容的基于虚拟化的保护与代码完整性，不能用作受防护的 Vm 主机：

- Dell PowerEdge 服务器正在运行 PERC H330 RAID 控制器的详细信息，请参阅以下文章从 Dell 支持[H330 – 启用"主机保护者 HYPER-V 支持"或 Win 2016 OS 上的"Device Guard"会导致 OS 启动故障](http://www.dell.com/Support/Article/us/en/19/QNA44045)。  


## <a name="compatible-systems"></a>兼容的系统

这些是我们和我们的合作伙伴已经在测试环境中的系统。 请确保你验证系统是否正常工作，这是你的环境中按预期方式： 

- 虚拟机 – 你可以从 Windows Server 2016 开始在 HYPER-V 主机运行的虚拟机上的代码完整性的基于虚拟化的保护。




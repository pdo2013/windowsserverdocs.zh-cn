---
title: 服务器硬件电源注意事项
description: 服务器硬件电源注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: a9d4653824d497ea0c42337260aa788bab354ba3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355023"
---
# <a name="server-hardware-power-considerations"></a>服务器硬件电源注意事项

了解企业和数据中心环境中能效的重要性，这一点很重要。 高性能和低能耗通常是矛盾的目标，但通过仔细选择服务器组件，可以在它们之间实现正确的平衡。 以下部分列出了有关服务器硬件组件的电源特性和功能的准则。

## <a name="processor-recommendations"></a>处理器建议

频率、操作电压、缓存大小和处理技术会影响处理器的能耗。 处理器具有散热设计点（TDP）级别，它提供了相对于其他模型的能源消耗的基本指示。

通常，可以选择符合性能目标的最低 TDP 处理器。 另外，较新版本的处理器通常会更有效地工作，并且它们可能会为 Windows 电源管理算法公开更多的电源状态，从而在所有性能级别上实现更好的电源管理。 或者，他们可以使用 Microsoft 与硬件制造商合作开发的一些新的 "协作" 电源管理技术。

有关协作电源管理技术的详细信息，请参阅[高级配置和电源接口规范](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf)中名为协作处理器性能控制的部分。


## <a name="memory-recommendations"></a>内存建议
内存帐户的总系统电量增加部分。 许多因素会影响内存 DIMM 的能耗，如内存技术、纠错码（ECC）、总线频率、容量、密度和排名。 因此，最好是在购买大量内存之前比较预期的电源分级。

低功耗内存现已可用，但你必须考虑性能和成本的权衡。 如果要对服务器进行分页，还应考虑分页磁盘的能源成本。


## <a name="disks-recommendations"></a>磁盘建议
RPM 越大，消耗的能耗就会增加。 SSD 驱动器比旋转驱动器更为强大。 而且，2.5 英寸驱动器通常需要比3.5 英寸更少的驱动器。

## <a name="network-and-storage-adapter-recommendations"></a>网络和存储器适配器建议
某些适配器在空闲期间会降低能耗。 对于 10 Gb 网络适配器和高带宽（4-8 Gb）存储链接，这是一个重要的考虑因素。 此类设备可能会消耗大量能源。


## <a name="power-supply-recommendations"></a>电源建议
提高电源效率是减少能耗而不影响性能的绝佳方式。 高效率电源可节省每年的每个服务器每年的千瓦时小时数。


## <a name="fan-recommendations"></a>风扇建议
与电源一样，风扇也可以降低能耗，而不会影响系统性能。 在系统负载降低的情况下，变速风扇可以减少 RPM，从而消除不必要的能耗。


## <a name="usb-devices-recommendations"></a>USB 设备建议
默认情况下，Windows Server 2016 启用对 USB 设备的选择性挂起。 不过，编写质量低劣的设备驱动程序仍然可以通过就前往的利润来中断系统能源效率。 若要避免潜在的问题，请断开 USB 设备的连接、在 BIOS 中禁用它们或选择不需要 USB 设备的服务器。


## <a name="remotely-managed-power-strip-recommendations"></a>远程管理的 Power 磁条建议
配电盘并不是服务器硬件的有机组成部分，但它们可能会在数据中心中产生很大的差异。 度量值表明，已接通电源、但表面上关机的卷服务器可能仍需要高达30瓦的功率。

为了避免浪费电量，你可以为每个服务器机架部署远程管理的配电盘，以编程方式断开特定服务器的电源。

## <a name="processor-terminology"></a>处理器术语
本主题中使用的处理器术语反映了下图中可用的组件层次结构。 从最大到最小的组件粒度使用的术语如下：

-   处理器插座
-   NUMA 节点
-   Core
-   逻辑处理器

![处理器术语](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>请参阅
- [服务器硬件性能注意事项](index.md)
- [电源和性能优化](power/power-performance-tuning.md)
- [处理器电源管理优化](power/processor-power-management-tuning.md)
- [建议的平衡计划参数](power/recommended-balanced-plan-parameters.md)

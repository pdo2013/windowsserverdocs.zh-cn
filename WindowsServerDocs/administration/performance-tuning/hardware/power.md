---
title: 服务器硬件功能的注意事项
description: 服务器硬件功能的注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5261c856a0a29f9f58526e4f9580a16bbed5be56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874298"
---
# <a name="server-hardware-power-considerations"></a>服务器硬件功能的注意事项

请务必识别企业和数据中心环境中的能源效率的重要性不断增加。 高性能和低能源使用情况通常是有冲突的目标，但通过仔细选择服务器组件，可获得正确平衡。 以下各节列出了实现 power 特征和功能的服务器硬件组件的准则。

## <a name="processor-recommendations"></a>处理器建议

操作系统电压、 缓存大小和处理技术的频率会影响处理器的能源消耗。 处理器都有指向基本地指示出相对于其他模型的能源消耗 (TDP) 分级的热量设计。

一般情况下，选择将满足性能目标的最低 TDP 处理器。 此外，较新代的处理器是通常更高的能效，而且它们可能会暴露更多的电源状态，对于 Windows 电源管理算法，这样在所有级别的性能更好的电源管理。 也可以使用一些新的"协作？ Microsoft 已开发出与硬件制造商合作关系中的电源管理方法。

协作式电源管理方法的详细信息，请参阅名为协作处理器性能控件中的部分[高级配置和电源接口规范](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf)。


## <a name="memory-recommendations"></a>内存建议
内存中占了总系统电源的不断增加小数。 许多因素会影响的能源消耗的内存 DIMM，如内存技术、 错误更正代码 (ECC)、 总线频率、 容量、 密度和数是多少。 因此，最好要购买大量的内存之前比较预期的 power 分级。

低功耗内存现已可用，但必须考虑性能和成本的权衡。 如果你的服务器将分页，你还应该考虑的分页磁盘的能源成本。


## <a name="disks-recommendations"></a>磁盘的建议
更高版本的 RPM 意味着增加的能源消耗。 SSD 驱动器的更多电源效率比旋转驱动器。 此外，2.5 英寸驱动器通常需要较少的电量比 3.5 英寸驱动器。

## <a name="network-and-storage-adapter-recommendations"></a>网络和存储适配器的建议
某些适配器减少能源消耗在空闲时段。 这是 10 Gb 网络适配器和高带宽 (4-8 Gb) 的存储链接的重要考虑事项。 此类设备可以使用大量能源。


## <a name="power-supply-recommendations"></a>电源供应的建议
改进电源效率是降低能耗，而不会影响性能的好办法。 高效率电源都可以保存许多 kilowatt-hours 每年，每个服务器。


## <a name="fan-recommendations"></a>风扇建议
风扇、 电源都像是位置，可以减少能源消耗而不会影响系统性能的区域。 变量速度赛事的球迷们可以作为系统负载降低，消除其它不必要的能源消耗减少 RPM。


## <a name="usb-devices-recommendations"></a>USB 设备的建议
默认情况下，Windows Server 2016 允许选择性挂起 USB 设备的。 但是，编写不佳的设备驱动程序可能仍会破坏系统能效由相当大的边距。 若要避免潜在问题，断开 USB 设备、 在 BIOS 中禁用它们，或选择不需要 USB 设备的服务器。


## <a name="remotely-managed-power-strip-recommendations"></a>远程管理电源条建议
电源电源不是不可或缺的一部分的服务器硬件，但它们会使数据中心中较大的差异。 度量值显示卷服务器插入，但具有表面上的电源已关闭，仍可能需要最多 30 瓦的功率。

若要避免浪费电能，你可以部署要以编程方式断开电源连接来自特定服务器的服务器的每个机架远程管理的接线板。

## <a name="processor-terminology"></a>处理器术语
在本主题中使用的处理器术语反映了可在下图中的组件的层次结构。 从大到最小粒度的组件使用的术语如下所示：

-   处理器套接字
-   NUMA 节点
-   核心版
-   逻辑处理器

![处理器术语](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>请参阅
- [服务器硬件的性能注意事项](index.md)
- [电源和性能优化](power/power-performance-tuning.md)
- [处理器电源管理优化](power/processor-power-management-tuning.md)
- [建议平衡的计划参数](power/recommended-balanced-plan-parameters.md)

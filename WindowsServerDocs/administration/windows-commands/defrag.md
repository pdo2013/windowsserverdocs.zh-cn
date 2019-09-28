---
title: defrag
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d930e224ac1610b5e49cbf5701778bfb6f14b2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378713"
---
# <a name="defrag"></a>defrag

>适用于：Windows 10，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

查找并合并本地卷上的零碎文件以提高系统性能。
本地**Administrators**组中的成员身份或等效身份是运行此命令所需的最低要求。

## <a name="syntax"></a>语法
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|`<volume>`|指定要进行碎片整理或分析的卷的驱动器号或装入点路径。|
|A|在指定的卷上执行分析。|
|C|对所有卷执行此操作。|
|D|执行传统碎片整理（这是默认值）。 但在分层卷上，传统碎片整理仅在容量层上执行。|
|E|在除指定的卷之外的所有卷上执行该操作。|
|G|优化指定卷上的存储层。|
|H|以普通优先级运行操作（默认值较低）。|
|I n|每个卷上最多可运行 n 个层优化。|
|K|在指定的卷上执行碎片合并。|
|L|对指定的卷执行重新剪裁。|
|M [n]|在后台并行运行每个卷上的操作。 最多 n 个线程并行优化存储层。|
|O|为每种媒体类型执行适当的优化。|
|T|跟踪指定卷上已在进行的操作。|
|U|在屏幕上打印操作进度。|
|V|打印包含碎片统计信息的详细输出。|
|X|在指定卷上执行可用空间合并。|
|?|显示此帮助信息。|

## <a name="remarks"></a>备注
- 不能对特定类型的文件系统卷或驱动器进行碎片整理：
  -   不能对文件系统锁定的卷进行碎片整理。
  -   不能对文件系统已标记为脏的卷进行碎片整理，这表明可能已损坏。 必须先在脏卷上运行**chkdsk** ，然后才能对其进行碎片整理。 您可以使用**fsutil** dirty query 命令确定卷是否已更新。 有关**chkdsk**和**fsutil**脏的详细信息，请参阅 "[其他参考](defrag.md#BKMK_additionalRef)"。
  -   不能对网络驱动器进行碎片整理。
  -   不能对 cdROMs 进行碎片整理。
  -   不能对**NTFS**、 **ReFS**、 **Fat**或**Fat32**以外的文件系统卷进行碎片整理。
- 使用 Windows Server 2008 R2、Windows Server 2008 和 Windows Vista，您可以计划对卷进行碎片整理。 但是，不能计划在驻留于 SSD 上的虚拟硬盘（VHD）上对固态驱动器（SSD）或卷进行碎片整理。
- 若要执行该过程，你必须是本地计算机上Administrators 组的成员，或你必须已被委派适当的权限。 如果计算机已加入某个域，则 Domain Admins 组的成员可能会执行该过程。 作为最佳安全方案，请考虑使用**运行方式**来执行此过程。
- 卷必须至少具有 15% 的可用空间，**碎片整理**才能完整地对其进行碎片整理。 **defrag**使用此空间作为文件片段的排序区域。 如果卷的可用空间小于 15%，则**defrag**只对其进行部分碎片整理。 若要增加卷上的可用空间，请删除不需要的文件或将其移到另一个磁盘上。
- 当**碎片整理**正在分析卷并对其进行碎片整理时，它会显示闪烁的光标。 当**defrag**完成分析并对卷进行碎片整理时，它会显示分析报告、碎片整理报告或两个报告，然后退出到命令提示符。
- 默认情况下，如果未指定 **/a**或 **/v**参数，则**defrag**将显示分析和碎片整理报告的摘要。
- 您可以通过键入 **>** <em>filename .txt</em>将报告发送给文本文件，其中*FileName .txt*是您指定的文件名称。 例如： `defrag volume /v > FileName.txt`
- 若要中断碎片整理进程，请在命令行上按**CTRL + C**。
- 运行**defrag**命令和磁盘碎片整理程序是相互排斥的。 如果使用磁盘碎片整理程序对卷进行碎片整理，并在命令行中运行**defrag**命令，则**defrag**命令将失败。 相反，如果运行**defrag**命令并打开磁盘碎片整理程序，磁盘碎片整理程序中的碎片整理选项将不可用。

## <a name="BKMK_examples"></a>示例
若要在提供进度和详细输出时对驱动器 C 上的卷进行碎片整理，请键入：
```
defrag C: /U /V
```
若要在后台并行对驱动器 C 和 D 中的卷进行碎片整理，请键入：
```
defrag C: D: /M
```
若要对驱动器 C 上装载的卷执行碎片分析并提供进度，请键入：
```
defrag C: mountpoint /A /U
```
若要对具有普通优先级的所有卷进行碎片整理并提供详细的输出，请键入：
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>计划任务
碎片整理的计划任务作为维护任务运行，通常计划每周运行一次。 管理员可以使用 "**优化驱动器**" 应用程序更改频率。
- 从计划任务运行时， **defrag**的 ssd 策略如下：
   - **传统碎片整理**（即移动文件以使它们合理连续），并且**重新剪裁**每月仅运行一次。
   - 如果忽略**传统碎片整理**和**重新剪裁**，则不会运行**分析**。
      - 如果用户在 SSD 上手动运行了**传统碎片整理**，则在上次计划任务运行后的3周内，下一个计划任务运行将执行**分析**和**重新剪裁**，但跳过该 SSD 上的**传统碎片整理**。
   - 如果跳过**分析**，则不会更新**优化驱动器**中的**上次运行**时间。  因此对于 Ssd，**优化驱动器**中的**上次运行**时间可能是一个月。
- 此维护任务可能不会对所有卷进行碎片整理，因为此任务会执行以下操作：
   - 不唤醒计算机以运行碎片整理
   - 仅在计算机使用交流电源时启动，并且在计算机切换到电池电源时停止
   - 如果计算机不再处于空闲状态，则停止

## <a name="BKMK_additionalRef"></a>其他参考
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil 脏](fsutil-dirty.md)
-   [命令行语法项](command-line-syntax-key.md)

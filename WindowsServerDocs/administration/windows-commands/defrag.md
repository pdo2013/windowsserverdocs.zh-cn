---
title: defrag
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813188"
---
# <a name="defrag"></a>defrag

>适用于：Windows 10 中，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

定位并整理碎片的文件来提高系统性能的本地卷上。
本地成员资格**管理员**组或等效身份是运行此命令所需的最低。

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
|A|指定卷上执行分析。|
|C|执行的所有卷上的操作。|
|D|执行传统的碎片整理 （这是默认值）。 分层卷上，传统的碎片整理是仅在层上执行容量。|
|E|执行除指定的所有卷上的操作。|
|G|优化指定的卷上的存储层。|
|H|按正常优先级运行操作 （默认值为低）。|
|我 n|层优化会在每个卷上运行最多 n 秒。|
|K|指定卷上执行碎片合并。|
|L|指定卷上执行重新剪裁。|
|M [n]|在后台中并行运行每个卷上的操作。 最多 n 个线程来优化并行中的存储层。|
|O|为每种媒体类型执行适当的优化。|
|T|指定卷上跟踪已在进行中的操作。|
|U|打印在屏幕上操作的进度。|
|V|打印包含碎片统计信息的详细输出。|
|X|执行指定的卷上的可用空间合并。|
|?|显示此帮助的信息。|

## <a name="remarks"></a>备注
-   您不能对特定类型的文件系统卷或驱动器进行碎片整理：
    -   您不能对已锁定的文件系统的卷进行碎片整理。
    -   无法整理卷文件系统已标记为已更新，以指示可能已损坏。 您必须运行**chkdsk**之前对其进行的脏数据卷上。 你可以确定卷是否使用脏**fsutil**脏查询命令。 有关详细信息**chkdsk**并**fsutil**脏页，请参阅[其他参考](defrag.md#BKMK_additionalRef)。
    -   您不能对网络驱动器进行碎片整理。
    -   您不能对光驱进行碎片整理。
    -   不能对不是文件系统卷进行碎片整理**NTFS**， **ReFS**， **Fat**或者**Fat32**。
-   使用 Windows Server 2008 R2 中，Windows Server 2008 和 Windows Vista 中，可以计划以对卷进行碎片整理。 但是，您不能计划进行碎片整理固态硬盘 (SSD) 或卷上虚拟硬盘 (VHD) 驻留在 SSD 上。
-   若要执行该过程，你必须是本地计算机上Administrators 组的成员，或你必须已被委派适当的权限。 如果计算机已加入某个域，则 Domain Admins 组的成员可能会执行该过程。 作为安全性最佳做法，请考虑使用**运行方式**才能执行此过程。
-   卷必须至少 15%的可用空间**碎片整理**以完整和充分地进行碎片整理。 **碎片整理**用作此空间的文件碎片排序的区域。 如果卷的可用空间，不超过 15%**碎片整理**将仅部分进行碎片整理。 若要增加的卷上的可用空间，删除不需要的文件或将它们移动到另一个磁盘。
-   虽然**碎片整理**是分析并对卷进行碎片整理，它将显示一个闪烁的光标。 当**碎片整理**完成分析并对卷进行碎片整理，它显示分析报表、 碎片整理报表或这两个报表，然后在命令提示符下退出。
-   默认情况下**碎片整理**如果不指定显示的分析和碎片整理报告摘要 **/a**或 **/v**参数。
-   您可以通过键入将报告发送给文本文件 **> ***FileName.txt*，其中*文件名.txt*是你指定的文件名称。 例如：`defrag volume /v > FileName.txt`
-   若要中断碎片整理过程中的，在命令行中，按**CTRL + C**。
-   运行**碎片整理**命令和磁盘碎片整理程序是互相排斥。 如果使用磁盘碎片整理程序来对卷进行碎片整理，并且运行**碎片整理**命令在命令行**碎片整理**命令会失败。 相反，如果在运行**碎片整理**命令，并打开磁盘碎片整理程序，磁盘碎片整理程序中的碎片整理选项将不可用。

## <a name="BKMK_examples"></a>示例
若要对进行碎片整理驱动器 C 上的卷时提供进度和详细输出，请键入：
```
defrag C: /U /V
```
若要对在后台中并行 C 和 D 驱动器上的卷进行碎片整理，请键入：
```
defrag C: D: /M
```
若要执行的驱动器 C 上装载的卷的碎片分析，并提供进度，请键入：
```
defrag C: mountpoint /A /U
```
若要以正常优先级对所有卷进行碎片都整理，并提供详细输出，请键入：
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>计划的任务
碎片整理的维护任务为计划任务运行，并通常计划每周运行。 管理员可以更改频率使用**优化驱动器**应用程序。
- 从计划任务运行时**碎片整理**Ssd，具有以下策略：
   - **繁体碎片整理**（即移动文件，才能合理地连续） 和**retrim**运行一次每个月。
   - 如果这两个**传统碎片整理**并**retrim**会跳过**analysis**则不会运行。
      - 如果用户运行**传统碎片整理**手动在 SSD 中说 3 周后最后的计划任务运行，则下一步计划的任务运行将执行**analysis**并**retrim**但跳过**传统碎片整理**SSD 上。
   - 如果**分析**会跳过**上次运行**时间**优化驱动器**将不会更新。  因此，对于 Ssd**上次运行**时间**优化驱动器**可以是一个月。
- 此维护任务可能不碎片整理的所有卷，有时由于此任务执行以下任务：
   - 不会唤醒计算机以运行碎片整理
   - 仅当计算机交流电源，且如果计算机转用电池电源，则停止启动
   - 如果计算机不再处于空闲状态，请停止

## <a name="BKMK_additionalRef"></a>其他参考
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil 脏](fsutil-dirty.md)
-   [命令行语法解答](command-line-syntax-key.md)

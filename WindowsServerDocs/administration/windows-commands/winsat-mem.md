---
title: winsat 内存优化
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc041c8a6c85b46913904ce80494f54cb818ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878338"
---
# <a name="winsat-mem"></a>winsat 内存优化



中的较大内存到内存缓冲副本，因为多媒体处理中使用反射的方式测试系统内存带宽。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-up|强制使用只有一个线程进行测试的内存。 默认值是运行每个物理 CPU 或核心一个线程。|
|-rn|指定评估的线程应按正常优先级运行。 默认值是运行优先级 15。|
|-nc|指定评估应分配内存并标记为未缓存。 这意味着，对于复制操作，将跳过处理器的缓存。 默认值是在缓存空间中运行。|
|-do \<n>|指定以字节为单位，源缓冲区末尾的目标缓冲区的开头之间的距离。 默认值为 64 个字节。 最大允许目标偏移量为 16 MB。 指定无效的目标偏移量将导致错误。</br>注意：零是有效值 **\<n >**，但不是负数。|
|-mint \<n >|指定以秒为单位评估运行时间最小值。 默认值为 2.0。 最小值为 1.0。 最大值是 30.0。</br>注意：指定 **-mint**值大于 **-maxt**时结合使用的两个参数的值将导致错误。|
|-maxt \<n>|指定的最大运行时间以秒为单位进行评估。 默认值为 5.0。 最小值为 1.0。 最大值是 30.0。 如果与结合 **-mint**参数，评估将开始执行定期的统计检查其结果中指定的时间段之后 **-mint**。 如果通过统计检查，则评估将在指定的时间段之前完成 **-maxt**已过。 如果评估的运行中指定的时间段内 **-maxt**不满足统计的检查，则评估将在该时间完成，并返回结果已收集。|
|-buffersize \<n >|指定的缓冲区大小，应使用内存复制测试。 两次将每个 CPU，确定从一个缓冲区复制到另一个数据量分配此数量。 默认值为 16 MB。 此值舍入到最接近的 4 KB 边界。 最大值为 32 MB。 最小值为 4 KB。 指定缓冲区大小无效，将导致错误。|
|-v|将详细的输出发送到 STDOUT，包括状态和进度信息。 任何错误也将写入命令窗口。|
|xml\<文件名称 >|将评估的输出保存为指定的 XML 文件。 如果指定的文件存在，它将被覆盖。|
|-idiskinfo|保存有关物理卷和逻辑磁盘的信息作为的一部分 **\<SystemConfig >** XML 输出中的部分。|
|-iguid|在 XML 输出文件中创建全局唯一标识符 (GUID)。|
|-请注意"注意 text"|请注意将文本添加到**\<注意 >** XML 输出文件中的部分。|
|-icn|在 XML 输出文件中包含本地计算机的名称。|
|-eef|枚举 XML 输出文件中的额外的系统信息。|

## <a name="BKMK_examples"></a>示例

-   下面的示例运行至少是 4 秒和时间不超过 12 秒、 使用 32 MB 的缓冲区大小，并将结果保存到文件的 XML 格式的评估**memtest.xml**。  
    ```
    winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
    ```

## <a name="remarks"></a>备注

-   本地 Administrators 组或同等成员资格是使用所需的最低**winsat**。 该命令必须在提升的命令提示符窗口中执行。
-   若要打开提升的命令提示符窗口，请单击**启动**，单击**附件**，右键单击**命令提示符下**，然后单击**以管理员身份运行**.

#### <a name="additional-references"></a>其他参考


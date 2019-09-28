---
title: winsat mem
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 23bbaf8551031a2e573b117e672a656213efde12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361984"
---
# <a name="winsat-mem"></a>winsat mem



以在多媒体处理中使用的方式，以多种方式测试系统内存带宽和内存缓冲区副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-向上|仅通过一个线程强制进行内存测试。 默认为每个物理 CPU 或核心运行一个线程。|
|-rn|指定评估的线程应以普通优先级运行。 默认值为在优先级15运行。|
|-nc|指定评估应分配内存，并将其标记为未缓存。 这意味着复制操作将绕过处理器的缓存。 默认值为在缓存空间中运行。|
|-do \<n >|指定源缓冲区结尾与目标缓冲区开头之间的距离（以字节为单位）。 默认值为64字节。 允许的最大目标偏移量为16MB。 指定无效的目标偏移量将导致错误。</br>注意:零是 **\<n >** 的有效值，但负数不是。|
|-mint \<n >|指定评估的最小运行时间（以秒为单位）。 默认值为2.0。 最小值为1.0。 最大值为30.0。</br>注意:当两个参数组合使用时，如果将 **-mint**值指定为大于 **-maxt**值，则会导致错误。|
|-maxt \<n >|指定评估的最长运行时间（以秒为单位）。 默认值为5.0。 最小值为1.0。 最大值为30.0。 如果与 **-mint**参数结合使用，则在 **-mint**中指定的时间段后，评估将开始定期统计检查其结果。 如果统计检查通过，则评估将在 **-maxt**中指定的时间段过后完成。 如果评估在 **-maxt**中指定的时间段内运行，而无需满足统计检查，则评估将在该时间完成，并返回它收集的结果。|
|-buffersize \<n >|指定内存复制测试应使用的缓冲区大小。 将每个 CPU 分配两次此数量，确定从一个缓冲区复制到另一个缓冲区的数据量。 默认值为16MB。 此值舍入为最接近的 4 KB 边界。 最大值为 32 MB。 最小值为 4 KB。 指定无效的缓冲区大小将导致错误。|
|-v|向 STDOUT 发送详细输出，包括状态和进度信息。 任何错误也将写入 "命令" 窗口。|
|-xml \<文件名 >|将评估的输出另存为指定的 XML 文件。 如果指定的文件存在，则将覆盖该文件。|
|-idiskinfo|将有关物理卷和逻辑磁盘的信息保存为 XML 输出中 **\<SystemConfig >** 部分的一部分。|
|-iguid|在 XML 输出文件中创建一个全局唯一标识符（GUID）。|
|-注意 "注释文本"|将注释文本添加到 XML 输出文件中的 "  **\<note >** " 部分。|
|-icn|在 XML 输出文件中包含本地计算机名称。|
|-eef|在 XML 输出文件中枚举额外的系统信息。|

## <a name="BKMK_examples"></a>示例

- 下面的示例使用32MB 缓冲区大小并将 XML 格式的结果保存到文件**memtest**，运行至少4秒且不超过12秒的评估。  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>备注

-   本地 Administrators 组中的成员身份或等效身份是使用**winsat**的最低要求。 必须从提升的命令提示符窗口执行该命令。
-   若要打开提升的命令提示符窗口，请单击 "**开始**"，单击 "**附件**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。

#### <a name="additional-references"></a>其他参考


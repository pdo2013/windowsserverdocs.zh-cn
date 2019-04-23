---
title: winsat mfmedia
description: Windows 命令
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69ce4fac127a6af8a94f3800d62c45989cf7020b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845428"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



测量的视频解码 （播放） 使用 Media Foundation 框架的性能。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Parameters

|Parameters|描述|
|----------|-----------|
|-输入\<文件名称 >|必需：指定包含要播放或编码的视频剪辑的文件。 文件可以是任何可以呈现的媒体基础的格式。|
|-dumpgraph|指定筛选器关系图应保存到与 GraphEdit 兼容的文件中，评估开始之前。|
|-ns|指定筛选器图形应该运行正常播放速度的输入文件。 默认情况下，筛选器关系图以尽快，忽略表示时间运行。|
|-play|运行中的评估解码模式和任何提供中指定的文件的音频内容的播放 **-输入**使用默认 DirectSound 设备。 默认情况下，禁用音频播放。|
|-nopmp|禁止使用的 Media Foundation 受保护的媒体管道 (MFPMP) 进程在评估过程。|
|-pmp|请始终使用 MFPMP 的评估过程中处理。</br>注意：如果 **-pmp**或 **-nopmp**未指定，则仅在必要时，将使用 MFPMP。|
|-v|将详细的输出发送到 STDOUT，包括状态和进度信息。 任何错误也将写入命令窗口。|
|xml\<文件名称 >|将评估的输出保存为指定的 XML 文件。 如果指定的文件存在，它将被覆盖。|
|-idiskinfo|保存有关物理卷和逻辑磁盘的信息作为的一部分 **\<SystemConfig >** XML 输出中的部分。|
|-iguid|在 XML 输出文件中创建全局唯一标识符 (GUID)。|
|-请注意"注意 text"|请注意将文本添加到**\<注意 >** XML 输出文件中的部分。|
|-icn|在 XML 输出文件中包含本地计算机的名称。|
|-eef|枚举 XML 输出文件中的额外的系统信息。|

## <a name="BKMK_examples"></a>示例

-   下面的示例使用过程中使用的输入文件运行评估**winsat 正式**评估，而无需使用 Media Foundation 受保护的媒体管道 (MFPMP)，c:\windows 所在的位置的计算机上Windows 文件夹中。  
    ```
    winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
    ```

## <a name="remarks"></a>备注

-   本地 Administrators 组或同等成员资格是使用所需的最低**winsat**。 该命令必须在提升的命令提示符窗口中执行。
-   若要打开提升的命令提示符窗口，请单击**启动**，单击**附件**，右键单击**命令提示符下**，然后单击**以管理员身份运行**.

#### <a name="additional-references"></a>其他参考


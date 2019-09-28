---
title: winsat mfmedia
description: Windows 命令
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3684d4b23ba6d34febe226f54b8b2ab2204f610c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361897"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



使用媒体基础框架测量视频解码（播放）的性能。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Parameters

|Parameters|描述|
|----------|-----------|
|-input \<file name >|必需：指定包含要播放或编码的视频剪辑的文件。 该文件可以是任何可由媒体基础呈现的格式。|
|-dumpgraph|指定在评估开始之前，筛选器关系图应保存到 GraphEdit 兼容文件。|
|-ns|指定筛选器关系图应以输入文件的正常播放速度运行。 默认情况下，"筛选器" 图会尽可能快地运行，而忽略显示时间。|
|-play|在解码模式下运行评估，并使用默认 DirectSound 设备在 **-input**中指定的文件中播放任何提供的音频内容。 默认情况下，音频播放处于禁用状态。|
|-nopmp|请不要在评估期间使用媒体基础受保护的媒体管道（MFPMP）过程。|
|-pmp|在评估期间，始终使用 MFPMP 进程。</br>注意:如果未指定 **-pmp**或 **-nopmp** ，则仅在必要时才使用 MFPMP。|
|-v|向 STDOUT 发送详细输出，包括状态和进度信息。 任何错误也将写入 "命令" 窗口。|
|-xml \<文件名 >|将评估的输出另存为指定的 XML 文件。 如果指定的文件存在，则将覆盖该文件。|
|-idiskinfo|将有关物理卷和逻辑磁盘的信息保存为 XML 输出中 **\<SystemConfig >** 部分的一部分。|
|-iguid|在 XML 输出文件中创建一个全局唯一标识符（GUID）。|
|-注意 "注释文本"|将注释文本添加到 XML 输出文件中的 "  **\<note >** " 部分。|
|-icn|在 XML 输出文件中包含本地计算机名称。|
|-eef|在 XML 输出文件中枚举额外的系统信息。|

## <a name="BKMK_examples"></a>示例

- 下面的示例在**winsat 正式**评估过程中使用的输入文件运行评估，而不使用媒体基础受保护的媒体管道（MFPMP），该计算机上的 C：\windows 是 windows 文件夹的位置。  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>备注

-   本地 Administrators 组中的成员身份或等效身份是使用**winsat**的最低要求。 必须从提升的命令提示符窗口执行该命令。
-   若要打开提升的命令提示符窗口，请单击 "**开始**"，单击 "**附件**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。

#### <a name="additional-references"></a>其他参考


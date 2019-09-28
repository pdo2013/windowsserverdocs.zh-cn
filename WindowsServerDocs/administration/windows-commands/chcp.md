---
title: chcp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5784b052ff1d7084d68cca0589caf518b8e44a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379535"
---
# <a name="chcp"></a>chcp



更改活动控制台代码页。 如果使用不带参数， **chcp** 显示活动控制台代码页的数目。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
chcp [<NNN>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<NNN >|指定代码页。|
|/?|在命令提示符下显示帮助。|

下表列出了每个受支持的代码页及其国家/地区或语言︰

|代码页|国家/地区或语言|
|---------|--------------------------|
|437|美国|
|850|多语言 (拉丁文我)|
|852|西里尔语 （俄语）|
|855|西里尔语 （俄语）|
|857|土耳其语|
|860|葡萄牙语|
|861|冰岛语|
|863|加拿大法语|
|865|北欧|
|866|俄语|
|869|现代希腊语|
|936|中文|

## <a name="remarks"></a>备注

-   仅与 Windows 一起安装的原始设备制造商 (OEM) 代码页正常显示在命令提示符窗口使用光栅字体。 在全屏幕模式下或使用 TrueType 字体的命令提示符窗口中，其他代码页正确显示。
-   不需要准备 （如上所示 MS-DOS) 的代码页。
-   之后启动的程序，将分配新的代码页使用新的代码页。 但是，在之前启动的程序 （除了 Cmd.exe) 分配新的代码页使用原来的代码页。

## <a name="BKMK_examples"></a>示例

若要查看活动的代码页设置，请键入︰
```
chcp
```
将显示一条类似于以下内容的消息：

`Active code page: 437`

若要将活动代码页更改为850（多语言），请键入：
```
chcp 850
```
如果指定的代码页无效，将显示以下错误消息︰

`Invalid code page`

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

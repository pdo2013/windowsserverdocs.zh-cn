---
title: break
description: 适用于**break_2**的 Windows 命令主题-将卷影副本卷与 VSS 解除相关，并使其作为常规卷进行访问。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5789e3442152c705b3197bf1ce5e63dc782a15c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379785"
---
# <a name="break"></a>break



将卷影副本卷与 VSS 解除映射，并使其作为常规卷进行访问。 然后，可以使用驱动器号（如果已指定）或卷名访问该卷。 如果不使用参数， **break**将在命令提示符下显示帮助。

> [!NOTE]
> 此命令仅适用于导入后的硬件卷影副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
break [writable] <SetID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|编写|启用对卷的读/写访问。|
|\<SetID >|指定卷影副本集的 ID。|

## <a name="remarks"></a>备注

-   公开的卷（例如它们源自的卷影副本）默认为只读。
-   可以在*SetID*参数中使用卷影副本 ID 的别名，该 ID 由**load metadata**命令存储为环境变量。

## <a name="BKMK_examples"></a>示例

若要在操作系统中使用别名 Alias1 作为可写卷访问的卷影副本，请键入：
```
break writable %Alias1%
```

> [!NOTE]
> 对卷的访问直接对硬件提供程序进行，而不记录卷的卷影副本。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
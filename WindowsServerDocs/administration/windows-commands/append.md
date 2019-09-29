---
title: append
description: 'Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdc4243bee8055888b023a56921cef757dda6b7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382750"
---
# <a name="append"></a>append



允许程序在指定的目录中打开数据文件，就好像这些文件位于当前目录中一样。 如果在没有参数的情况下使用，则**append**将显示附加的目录列表。

> [!NOTE]
> Windows 10 不支持此命令。
>

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                                 描述                                                                                 |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [@no__t >：] <Path> |                                                                 指定要追加的驱动器和目录。                                                                  |
|       /x：开启       |                                                  将附加的目录应用于文件搜索和启动应用程序。                                                  |
|      /x： off       |                                     仅将附加的目录应用于打开文件的请求。</br>**/x： off**为默认设置。                                     |
|     /path： on      |                               将附加的目录应用到已指定路径的文件请求。 **/path： on**是默认设置。                               |
|     /path： off     |                                                                    关闭 **/path： on**的效果。                                                                    |
|        /e         | 将附加目录列表的副本存储在名为 APPEND 的环境变量中。 **/e**只能在启动系统后首次使用**append**时使用。 |
|         ;         |                                                                     清除追加的目录列表。                                                                     |
|        /?         |                                                                    在命令提示符下显示帮助。                                                                     |

## <a name="BKMK_examples"></a>示例

若要清除附加目录列表，请键入：
```
append ;
```
若要将追加目录的副本存储到名为 APPEND 的环境变量中，请键入：
```
append /e
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

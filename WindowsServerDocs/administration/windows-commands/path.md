---
title: path
description: 了解如何设置 PATH 环境变量。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81e8441e7c67e42bdf929e703c8fe780a6f8aff8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372421"
---
# <a name="path"></a>path



设置 PATH 环境变量（用于搜索可执行文件的目录集）中的命令路径。 如果在没有参数的情况下使用，则**path**显示当前命令路径。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                                                     描述                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [@no__t >：] <Path> |                                                                            指定要在命令路径中设置的驱动器和目录。                                                                             |
|         ;         | 分隔命令路径中的目录。 如果在没有其他参数的情况下使用，**则为;** 将从 PATH 环境变量中清除现有命令路径，并指示 cmd.exe 仅在当前目录中进行搜索。 |
|      通道       |                                                         将命令路径追加到 PATH 环境变量中列出的现有目录集。                                                         |
|        /?         |                                                                                         在命令提示符下显示帮助。                                                                                         |

## <a name="remarks"></a>备注

-   在语法中包含 **% PATH%** 时，cmd.exe 会将其替换为 PATH 环境变量中的命令路径值，从而无需在命令提示符处手动输入这些值。
-   当前目录始终在命令路径中指定的目录之前搜索。
-   目录中的文件可能共享相同的文件名，但扩展名不同。 例如，你可能有一个名为 Accnt.com 的文件，该文件启动一个记帐程序，另一个名为 Accnt 的文件将服务器连接到记帐系统网络。

    Windows 操作系统会按以下优先级顺序使用默认的文件扩展名搜索文件： .exe、.com、.bat 和 .cmd。 若要在同一目录中存在 Accnt.com 时运行 Accnt，必须在命令提示符下包含 .bat 扩展名。
-   如果命令路径中的两个或多个文件具有相同的文件名和扩展名，则**路径**将首先在当前目录中搜索指定的文件名。 然后，它将按照路径环境变量中列出的顺序搜索命令路径中的目录。
-   如果将**path**命令放在 autoexec.bat 文件中，则每次登录到计算机时，Windows 操作系统会自动追加指定的 MS-DOS 子系统搜索路径。 Cmd.exe 不使用 Autoexec.bat 文件。 从快捷方式开始时，Cmd.exe 继承在我的电脑/Properties/Advanced/环境中设置的环境变量。

## <a name="BKMK_examples"></a>示例

若要在路径 C:\User\Taxes、B:\User\Invest 和 B:\Bin 中搜索外部命令，请键入：

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
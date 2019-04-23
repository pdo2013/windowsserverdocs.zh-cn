---
title: path
description: 了解如何设置 PATH 环境变量。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 65ccaf23b0e19319383952f3a1ca436aaf4d06fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856458"
---
# <a name="path"></a>path



设置 PATH 环境变量 （用于搜索可执行文件的目录集） 中的命令路径。 如果使用不带参数，**路径**显示当前的命令路径。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:]<Path>|指定的驱动器和目录中的命令路径设置。|
|;|用于分隔命令路径中的目录。 如果使用不带其他参数， **;** 清除路径环境变量中的现有命令路径并指示 Cmd.exe 仅在当前目录中搜索。|
|%PATH%|将命令路径追加到现有的 PATH 环境变量中列出的目录集。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   当包括 **%PATH%** 在语法中，Cmd.exe 它的命令路径值替换位于 PATH 环境变量，从而无需手动输入这些值在命令提示符处。
-   之前的命令路径中指定的目录始终搜索当前目录。
-   您可能有文件的目录中共享相同的文件名，但具有不同的扩展名。 例如，可能有一个名为 Accnt.com 启动会计程序文件和名为 Accnt.bat 将你的服务器连接到记帐系统网络的另一个文件。

    Windows 操作系统通过以下优先顺序使用默认文件扩展名搜索文件：.exe、.com、.bat、 和。 cmd。 若要运行 Accnt.bat Accnt.com 存在相同的目录中时，必须包括在命令提示符下.bat 扩展名。
-   如果命令路径中的两个或多个文件具有相同的文件名和扩展名，**路径**第一个搜索指定的文件命名为当前目录中。 然后在 PATH 环境变量中列出的顺序的命令路径中搜索的目录。
-   如果将置于**路径**命令在 Autoexec.nt 文件中，Windows 操作系统在每次登录到您的计算机时自动将追加指定的 MS-DOS 子系统搜索路径。 Cmd.exe 不使用 Autoexec.nt 文件。 从一种快捷方式启动时，Cmd.exe 继承设置我的计算机/属性/高级/环境中的环境变量。

## <a name="BKMK_examples"></a>示例

若要搜索 C:\User\Taxes、 B:\User\Invest 和 B:\Bin 外部命令的路径，请键入：

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
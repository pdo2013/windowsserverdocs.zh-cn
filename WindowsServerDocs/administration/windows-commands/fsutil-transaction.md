---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil 事务
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c225c99919a2558559b1ec7a47b61d716e199a73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438998"
---
# <a name="fsutil-transaction"></a>Fsutil 事务
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7，Windows 2008 中，Windows Vista

管理 NTFS 的事务。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parameters

| 参数  |                                                                                                                                                     描述                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   提交   |                                                                                                                      将成功的隐式或显式指定事务的结束标记。                                                                                                                      |
|   <GUID>   |                                                                                                                               指定表示事务的 GUID 值。                                                                                                                               |
|  fileinfo  |                                                                                                                              显示指定的文件的事务信息。                                                                                                                               |
| <Filename> |                                                                                                                                         指定完整路径和文件名。                                                                                                                                          |
|    列表    |                                                                                                                                 显示当前正在运行的事务的列表。                                                                                                                                  |
|   查询    | 显示指定的事务的信息。<br /><br />-如果**fsutil 事务查询文件**指定，则将仅为指定的事务显示文件信息。<br />-如果**fsutil 事务查询所有**指定，将显示该事务的所有信息。 |
|  回滚  |                                                                                                                                指定的事务回滚到开始。                                                                                                                                 |

### <a name="remarks"></a>备注

-   Windows Server 2008 中引入了事务性 NTFS。

### <a name="BKMK_examples"></a>示例
若要显示文件 c:\test.txt 的事务信息，请键入：

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[事务性 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)



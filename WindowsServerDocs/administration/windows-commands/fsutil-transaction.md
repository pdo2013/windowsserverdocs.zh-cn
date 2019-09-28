---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil transaction
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 286660baad699e21abe751a9cb956b1ac7613e80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376857"
---
# <a name="fsutil-transaction"></a>Fsutil transaction
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7，Windows 2008，Windows Vista

管理 NTFS 事务。

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
|   立即   |                                                                                                                      标记成功的隐式或显式指定的事务的结束。                                                                                                                      |
|   <GUID>   |                                                                                                                               指定表示事务的 GUID 值。                                                                                                                               |
|  fileinfo  |                                                                                                                              显示指定文件的事务信息。                                                                                                                               |
| <Filename> |                                                                                                                                         指定完整路径和文件名。                                                                                                                                          |
|    list    |                                                                                                                                 显示当前正在运行的事务的列表。                                                                                                                                  |
|   query    | 显示指定事务的信息。<br /><br />-如果指定了**fsutil transaction Query Files** ，将仅为指定的事务显示文件信息。<br />-如果指定了**fsutil transaction Query all** ，则将显示该事务的所有信息。 |
|  回退  |                                                                                                                                将指定的事务回滚到开始处。                                                                                                                                 |

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



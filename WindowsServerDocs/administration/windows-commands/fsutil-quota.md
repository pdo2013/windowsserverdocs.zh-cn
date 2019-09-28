---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil 配额
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 5e1c6793ca866ecacd8b00aa7e01d632c2538405
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376842"
---
# <a name="fsutil-quota"></a>Fsutil 配额
>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10，Windows Server 2012 R2，Windows 8.1，Windows Server 2012，Windows 8，Windows Server 2008 R2，Windows 7

管理 NTFS 卷上的磁盘配额，以便更精确地控制基于网络的存储。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>Parameters

|   参数   |                                                                                    描述                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    禁用    |                                                         在指定卷上禁用配额跟踪和强制。                                                          |
|    实行    |                                                                   在指定卷上强制实施配额。                                                                   |
|    modify     |                                                              修改现有磁盘配额，或创建新配额。                                                              |
|     query     |                                                                            列出现有磁盘配额。                                                                            |
|     记录     |                                                                    跟踪指定卷上的磁盘使用情况。                                                                     |
|  多次   | 搜索系统和应用程序日志，并显示一条消息，指示已检测到配额冲突，或者用户已达到配额阈值或配额限制。 |
| \<VolumePath > |                                  必需。 指定驱动器名称后跟冒号或格式**卷 {** <em>GUID</em> **}** 中的 GUID。                                  |
| \<Threshold >  |                            设置发出警告的限制（以字节为单位）。 此参数对于**fsutil 配额修改**命令是必需的。                            |
|   \<Limit >    |                                设置允许的最大磁盘使用量（以字节为单位）。 此参数对于**fsutil 配额修改**命令是必需的。                                |
|  \<UserName >  |                                      指定域或用户名。 此参数对于**fsutil 配额修改**命令是必需的。                                       |

## <a name="remarks"></a>备注

-   磁盘配额是根据每个卷实现的，它们启用了基于每个用户的硬和软存储限制。

-   你可以使用在每次添加新用户或自动跟踪配额限制、将其编译为报表以及自动通过电子邮件将其发送给系统管理员时使用**fsutil 配额**的编写脚本设置配额限制。

### <a name="BKMK_examples"></a>示例
若要列出使用 GUID "{928842df-5a01-11de-a85c-806e6f6e6963}" 指定的磁盘卷的现有磁盘配额，请键入：

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

若要列出使用驱动器号（ **C：** ）指定的磁盘卷的现有磁盘配额，请键入：

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)



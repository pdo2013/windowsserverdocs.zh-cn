---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: fsutil 配额
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 1e844d73348ee31f309f44895831ded9a2e6365c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439059"
---
# <a name="fsutil-quota"></a>fsutil 配额
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理要提供更精确地控制基于网络的存储的 NTFS 卷上的磁盘配额。

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
|    禁用    |                                                         禁用配额跟踪和强制指定卷上。                                                          |
|    强制实施    |                                                                   强制执行指定的卷上的配额使用情况。                                                                   |
|    modify     |                                                              修改现有的磁盘配额或创建新的配额。                                                              |
|     查询     |                                                                            列出现有的磁盘配额。                                                                            |
|     跟踪     |                                                                    跟踪磁盘上的指定卷的使用情况。                                                                     |
|  冲突   | 搜索系统和应用程序日志，并显示一条消息，以指示已检测到配额冲突或用户已达到配额阈值或配额限制。 |
| \<VolumePath> |                                  必需。 指定后接一个冒号或格式的 GUID 的驱动器名称**卷 {** <em>GUID</em> **}** 。                                  |
| \<阈值 >  |                            设置将发出警告的限制 （以字节为单位）。 此参数是必需的**fsutil 配额修改**命令。                            |
|   \<限制 >    |                                设置最大允许的磁盘使用情况 （以字节为单位）。 此参数是必需的**fsutil 配额修改**命令。                                |
|  \<UserName>  |                                      指定域或用户的名称。 此参数是必需的**fsutil 配额修改**命令。                                       |

## <a name="remarks"></a>备注

-   磁盘配额按实现的每个卷，并且它们使这两个硬件和软件的存储限制，以实现基于每个用户。

-   可以使用编写脚本，使用**fsutil 配额**若要设置的配额限制，每次添加新用户或能够自动跟踪配额限制，将其编译到报表，并自动将其发送到电子邮件中的系统管理员。

### <a name="BKMK_examples"></a>示例
若要列出现有的磁盘使用的 GUID，{928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁盘卷的配额类型：

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

若要列出的驱动器号，使用指定的磁盘卷上的现有磁盘配额**c:** ，类型：

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>其他参考
[命令行语法项](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)



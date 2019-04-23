---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: fsutil 分层
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859248"
---
# <a name="fsutil-tiering"></a>fsutil 分层
>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

使您能够管理存储层功能，如设置和禁用标志和层的列表。

## <a name="syntax"></a>语法

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|clearflags|禁用卷的分层行为标记。|
|\<volume>|指定的卷。|
|/TrNH|卷与分层存储，会导致热收集被禁用。<br /><br>仅适用于 NTFS 和 ReFS。|
|queryflags|查询卷的分层行为标志。|
|regionlist|列出了分层的区域内的一个卷和其各自的存储层。|
|setflags|使卷的分层行为标志。|
|tierlist|列出了与卷关联存储 tieres。|


### <a name="examples"></a>示例

若要查询卷 C 上的标志，请键入：

```
fsutil tiering clearflags C:
```

若要在卷 C 上设置标志，请键入：

```
fsutil tiering setflags C: /TrNH
```

若要清除卷 C 上的标志，请键入：

```
fsutil tiering clearflags C: /TrNH
```

若要列出的卷 C 和其各自的存储层的区域，请键入：

```
fsutil tiering regionlist C:
```

若要列出的卷 C 层，请键入：

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


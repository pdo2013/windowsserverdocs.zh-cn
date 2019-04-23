---
title: bitsadmin getaclflags
description: Windows 命令主题**bitsadmin getaclflags** -检索访问控制列表传播标志。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861448"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

检索访问控制列表 (ACL) 传播标志。

## <a name="syntax"></a>语法

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

将显示一个或多个以下的标志值︰
-   O:与文件复制所有者信息。
-   G:与文件复制组信息。
-   D:与文件复制 DACL 信息。
-   S:与文件复制 SACL 信息。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的访问控制列表传播标志 *myDownloadJob*。
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
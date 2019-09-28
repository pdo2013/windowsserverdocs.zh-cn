---
title: bitsadmin getaclflags
description: 适用于**bitsadmin getaclflags**的 Windows 命令主题-检索访问控制列表传播标志。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381786"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

检索访问控制列表（ACL）传播标志。

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
-   I/O将所有者信息复制到文件。
-   G用文件复制组信息。
-   D:复制 DACL 信息和文件。
-   些将 SACL 信息复制到文件。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的访问控制列表传播标志 *myDownloadJob*。
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
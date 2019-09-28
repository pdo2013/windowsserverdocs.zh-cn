---
title: bitsadmin getproxybypasslist
description: 适用于**bitsadmin getproxybypasslist**的 Windows 命令主题-检索指定作业的代理跳过列表。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381419"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

检索指定作业的代理跳过列表。

## <a name="syntax"></a>语法

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

旁路列表包含不是通过代理路由的主机名或 IP 地址。 此列表可以包含 "\<local >" 以引用同一 LAN 上的所有服务器。 列表可以用分号或空格分隔。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的代理跳过列表。
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
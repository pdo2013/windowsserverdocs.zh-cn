---
title: bitsadmin getproxylist-检索指定作业的代理列表。
description: 适用于**bitsadmin getproxylist**的 Windows 命令主题-检索指定作业的代理列表。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381311"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

检索指定作业的代理列表。

## <a name="syntax"></a>语法

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

代理列表是要使用的代理服务器的列表。 此列表以逗号分隔。

## <a name="BKMK_examples"></a>示例

下面的示例将检索名为*myDownloadJob*的作业的代理列表。
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
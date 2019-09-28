---
title: bitsadmin removeclientcertificate
description: 适用于**bitsadmin removeclientcertificate**的 Windows 命令主题-从作业中删除客户端证书。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c664ba9b26f3511dedf35477a1cd393db709337e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381040"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate



从作业中删除客户端证书。

## <a name="syntax"></a>语法

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例从名为*myJob*的作业中删除客户端证书。
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
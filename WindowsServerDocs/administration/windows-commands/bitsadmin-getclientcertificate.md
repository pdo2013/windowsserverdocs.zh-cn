---
title: bitsadmin getclientcertificate
description: Windows 命令主题**bitsadmin getclientcertificate** -从作业检索客户端证书。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 113d733d1deb9fbb1c89231495cb7af668a444d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869038"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate



从作业中检索客户端证书。

## <a name="syntax"></a>语法

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的客户端证书*myDownloadJob*。
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
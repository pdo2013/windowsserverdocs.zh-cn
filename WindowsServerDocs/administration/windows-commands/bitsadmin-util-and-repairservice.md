---
title: bitsadmin util 和 repairservice
description: Windows 命令主题，适用于**bitsadmin util 和 repairservice**命令，用于修复各种版本的 BITS 服务的已知问题。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380276"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

如果 BITS 无法启动，请使用此开关修复不同版本的 BITS 的已知问题。

**BITSAdmin 1.5 及更早版本：** 支持  Not。

## <a name="syntax"></a>语法

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Force|可选-删除并重新创建该服务。|

## <a name="remarks"></a>备注

此开关解决了与不正确的服务配置和 Windows 服务（如 LANManworkstation）和网络目录的依赖项相关的错误。 此开关生成一个输出，用于指示是否已解决问题。

> [!NOTE]
> 如果 BITS 重新创建服务，则可以在本地化系统中将服务说明字符串设置为英语。

> [!IMPORTANT]
> Windows Vista 不支持此命令。

## <a name="BKMK_examples"></a>示例

以下示例修复 BITS 服务配置。
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
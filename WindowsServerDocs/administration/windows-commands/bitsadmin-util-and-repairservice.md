---
title: bitsadmin util 和 repairservice
description: Windows 命令主题**bitsadmin util 和 repairservice** -使用命令来解决各种版本的 BITS 服务的已知的问题。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852088"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util 和 repairservice

如果 BITS 启动失败，则使用此开关以修复各种版本的位的已知的问题。

**BITSAdmin 1.5 和更早版本：** 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Force|可选-删除并重新创建该服务。|

## <a name="remarks"></a>备注

此开关解决了错误与错误相关的服务配置和 Windows 服务 （例如 LANManworkstation) 上的依赖项以及网络目录。 此开关将生成输出，指示如果问题，已解决。

> [!NOTE]
> 如果 BITS 重新创建该服务，可能会在本地化系统中为英语设置服务说明字符串。

> [!IMPORTANT]
> 在 Windows Vista 上不支持此命令。

## <a name="BKMK_examples"></a>示例

下面的示例，以修复 BITS 服务配置。
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
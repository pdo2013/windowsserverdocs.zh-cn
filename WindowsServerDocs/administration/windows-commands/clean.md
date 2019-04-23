---
title: clean
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e7d7613784f3e599005b25259f70466db60e626
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877428"
---
# <a name="clean"></a>clean

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Diskpart clean 命令中删除所有分区或卷格式从选中的磁盘。
## <a name="syntax"></a>语法
```
clean [all]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|全部|指定在磁盘上的每个扇区设置为零，完全删除磁盘上包含的所有数据。|
## <a name="remarks"></a>备注
-   在主启动记录 (MBR) 磁盘，覆盖 MBR 分区信息和隐藏的扇区信息。
-   在 GUID 分区表 (gpt) 磁盘上，会覆盖 gpt 分区信息，包括保护性 MBR。 没有任何隐藏扇区信息。
-   成功执行此操作，必须选择一个磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。
## <a name="BKMK_examples"></a>示例
若要删除所选磁盘中的所有格式，请键入：
```
clean
```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)

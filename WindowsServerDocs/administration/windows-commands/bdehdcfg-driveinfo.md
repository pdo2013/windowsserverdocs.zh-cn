---
title: bdehdcfg driveinfo
description: 'Windows 命令主题 * * bdehdcfg: driveinfo * *-显示的驱动器号、 总大小、 最大的可用空间，以及分区特征。'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4aa041c27b1797e7d00476212887a7dc6dbc1880
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889058"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: driveinfo

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示的驱动器号、 总大小、 最大可用空间和分区特征。 列出了有效的分区。 如果已存在四个主要或扩展分区，未列出的未分配的空间。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<DriveLetter>|指定后接一个冒号的驱动器号。|
## <a name="remarks"></a>备注
该命令只是提供信息并不会修改该驱动器。
## <a name="BKMK_Examples"></a>示例
下面的示例将显示为驱动器 c。 驱动器信息
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)

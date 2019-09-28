---
title: bdehdcfg system.io.driveinfo
description: 适用于 * * bdehdcfg： system.io.driveinfo * * 的 Windows 命令主题显示驱动器号、总大小、最大可用空间和分区特征。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b0f4541bfd71fb7639d18e6e548559ed02918815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382269"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg： system.io.driveinfo

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示驱动器号、总大小、最大可用空间和分区特征。 仅列出了有效的分区。 如果已存在四个主分区或扩展分区，则不会列出未分配的空间。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parameters

|   参数   |                  描述                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | 指定后跟冒号的驱动器号。 |

## <a name="remarks"></a>备注
命令仅提供信息，不对驱动器进行任何修改。
## <a name="BKMK_Examples"></a>实例
下面的示例将显示驱动器 C 的驱动器信息。
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)

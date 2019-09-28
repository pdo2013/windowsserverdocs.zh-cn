---
title: 详细命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c7ecc2bb3578b578060694c95833fd32674db10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369912"
---
# <a name="the-verbose-command"></a>详细命令



显示指定命令的详细输出。 可以将 **/verbose**与运行的任何其他 WDSUTIL 命令一起使用。 请注意，必须在**WDSUTIL**之后直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>语法

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>示例

若要从自动添加数据库中删除已批准的计算机并显示详细输出，请键入：
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```
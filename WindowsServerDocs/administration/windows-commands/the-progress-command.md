---
title: 进度命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fff31c91b4d267011f2d738b4fc3acb3f0f2377
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832698"
---
# <a name="the-progress-command"></a>进度命令



正在执行命令时将显示进度。 可以使用 **/progress**与您运行的任何其他 WDSUTIL 命令。 请注意，必须指定 **/verbose**并 **/progress**后直接**WDSUTIL**。

## <a name="syntax"></a>语法

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>示例

若要初始化该服务器，并显示进度，请键入：
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```
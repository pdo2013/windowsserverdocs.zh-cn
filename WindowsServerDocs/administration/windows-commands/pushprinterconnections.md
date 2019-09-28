---
title: pushprinterconnections
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe25a29af34f78ebe161dc0d07c5edf64257f5c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371965"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



从组策略读取已部署的打印机连接设置，并根据需要部署/删除打印机连接。

## <a name="syntax"></a>语法

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|< 日志 >|将每个用户的调试日志文件写入% temp，或将每个计算机的调试日志写入%windir%\temp|
|<-？ >|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

此实用程序在计算机启动或用户登录脚本中使用，不能从命令行运行。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [使用组策略部署打印机](https://go.microsoft.com/fwlink/?LinkId=230627)
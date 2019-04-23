---
title: pushprinterconnections
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873718"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



读取从组策略部署打印机连接设置并部署/删除打印机连接根据需要。

## <a name="syntax"></a>语法

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|<-log>|将写入每个用户调试日志文件 %temp 或写入到每个对 windir%\temp 机调试日志。|
|<-?>|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

此实用程序是在计算机启动或用户登录脚本中使用和不能从命令行运行。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [使用组策略部署打印机](https://go.microsoft.com/fwlink/?LinkId=230627)
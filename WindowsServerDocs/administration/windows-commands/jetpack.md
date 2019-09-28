---
title: jetpack
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b011658c6a745d62707cf88404379b17b0e05eef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375321"
---
# <a name="jetpack"></a>jetpack

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

压缩 Windows Internet 名称服务（WINS）或动态主机配置协议（DHCP）数据库。 Microsoft 建议您在 WINS 数据库接近 30 MB 时将其压缩。 

## <a name="syntax"></a>语法
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<database name>|指定原始数据库文件。|
|<temp database name>|指定临时数据库文件。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_Examples"></a>示例
压缩 WINS 数据库：
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
压缩 DHCP 数据库：
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
在上述示例中， **Tmp**是 jetpack 使用的临时数据库。 **Wins-a**是 wins 数据库。 **Dhcp**数据库。
jetpack 通过执行以下操作来压缩 WINS 或 DHCP 数据库：
1.  将数据库信息复制到名为**Tmp**的临时数据库文件。
2.  删除原始数据库文件、 **wins-a**或**Dhcp**。
3.  将临时数据库文件重命名为原始文件名。

> [!NOTE]
> 在压缩过程中，jetpack 将使用*temp database name*参数指定的名称创建一个临时文件。 压缩过程完成后，将删除临时文件。 请确保 WINS 或 DHCP 文件夹中没有与*temp database name*参数中指定的文件同名的文件。

## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)

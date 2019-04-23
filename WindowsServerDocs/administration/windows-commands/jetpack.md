---
title: jetpack
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a3bffc29519df139921bdb1de53e67acd558b306
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858008"
---
# <a name="jetpack"></a>jetpack

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将压缩的 Windows Internet 名称服务 (WINS) 或动态主机配置协议 (DHCP) 数据库。 Microsoft 建议每当接近 30MB 压缩 WINS 数据库。 

## <a name="syntax"></a>语法
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<database name>|指定初始数据库文件。|
|<temp database name>|指定的临时数据库文件。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_Examples"></a>示例
若要压缩 WINS 数据库：
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
将 DHCP 数据库压缩：
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
在上面的示例**Tmp.mdb**是由 jetpack.exe 的临时数据库。 **Wins.mdb**是 WINS 数据库。 **Dhcp.mdb**是 DHCP 数据库。
jetpack.exe 压缩 WINS 或 DHCP 数据库执行以下操作：
1.  副本数据库到名为的临时数据库文件的信息**Tmp.mdb**。
2.  删除原始数据库文件， **Wins.mdb**或**Dhcp.mdb**。
3.  将临时数据库文件重命名为原始文件名。

> [!NOTE]
> 在压缩过程中，jetpack.exe 创建临时文件的名称，由指定*临时数据库名称*参数。 Compact 过程完成后，会删除临时文件。 请确保您没有在 WINS 或 DHCP 中已存在的文件中指定与同名的文件夹*临时数据库名称*参数。

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

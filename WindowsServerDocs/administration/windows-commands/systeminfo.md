---
title: systeminfo
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841498"
---
# <a name="systeminfo"></a>systeminfo



显示详细配置信息在计算机和其操作系统，包括操作系统配置、 安全信息、 产品 ID 和硬件属性 （如 RAM、 磁盘空间和网卡）。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u\<域 >\<用户名 >|使用指定的用户帐户的帐户权限运行该命令。 如果 **/u**未指定，则此命令使用当前登录到发出命令的计算机的用户的权限。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/fo\<格式 >|使用以下值之一指定输出格式：</br>表：显示输出表中。</br>列表：显示输出列表中。</br>CSV:以逗号分隔值格式显示输出。|
|/nh|禁止显示在输出中的列标题。 时有效 **/fo**参数设置为表或 CSV。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要查看名为 Srvmain 的计算机的配置信息，请键入：

**systeminfo /s srvmain**

若要远程查看名为 Srvmain2 位于 Maindom 域的计算机的配置信息，请键入：

**systeminfo /s srvmain2 /u maindom\hiropln**

若要远程查看名为 Srvmain2 位于 Maindom 域的计算机的配置信息 （以列表格式），请键入：

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
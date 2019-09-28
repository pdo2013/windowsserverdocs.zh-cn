---
title: systeminfo
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32a84a33c5339e9949648a4e40d71daf25c055d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370690"
---
# <a name="systeminfo"></a>systeminfo



显示有关计算机及其操作系统的详细配置信息，包括操作系统配置、安全信息、产品 ID 和硬件属性（如 RAM、磁盘空间和网卡）。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/s \<Computer >|指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。|
|/u \<Domain > \<UserName >|用指定用户帐户的帐户权限运行命令。 如果未指定 **/u** ，则此命令将使用当前登录到发出命令的计算机的用户的权限。|
|/p \<密码 >|指定在 **/u**参数中指定的用户帐户的密码。|
|/fo \<Format >|用以下值之一指定输出格式：</br>数据表显示表中的输出。</br>成员列表在列表中显示输出。</br>.CSV以逗号分隔值格式显示输出。|
|/nh|取消输出中的列标题。 当 **/fo**参数设置为表或 CSV 时有效。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要查看名为 Srvmain 的计算机的配置信息，请键入：

**systeminfo.exe/s srvmain**

若要远程查看位于 Maindom 域上名为 Srvmain2 的计算机的配置信息，请键入：

**systeminfo.exe/s srvmain2/u maindom\hiropln**

若要远程查看位于 Maindom 域上名为 Srvmain2 的计算机的配置信息（列表格式），请键入：

**systeminfo.exe/s srvmain2/u maindom\hiropln/p p@ssW23/fo list**

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
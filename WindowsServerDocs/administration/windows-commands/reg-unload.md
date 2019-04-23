---
title: Reg 卸载
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834978"
---
# <a name="reg-unload"></a>Reg 卸载



删除使用加载的注册表部分**reg 负载**操作。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg unload <KeyName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<KeyName>|指定要卸载的子项的完整路径。 用于指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 本地计算机的有效的根键是 HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键是 HKLM 和 hku 开头。|
|/?|显示的帮助**reg 卸载**在命令提示符处。|

## <a name="remarks"></a>备注

下表列出的返回值**reg 卸载**选项。

|ReplTest1|Description|
|-----|-----------|
|0|成功|
|1|失败|

## <a name="BKMK_examples"></a>示例

若要卸载配置单元 TempHive 文件 HKLM 中，键入：
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> 不要编辑注册表直接除非别无选择。 注册表编辑器避开了标准安全措施，从而使得这些可能会降低性能、 会损坏您的系统，或甚至需要您重新安装 Windows 设置。 通过使用 Microsoft 管理控制台 (MMC) 或控制面板中的程序，可以安全地更改大多数注册表设置。 如果必须直接编辑注册表，先进行备份。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
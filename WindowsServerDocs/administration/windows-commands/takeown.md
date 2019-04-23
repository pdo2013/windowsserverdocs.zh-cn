---
title: takeown
description: 了解如何通过成为文件的所有者获取对文件的访问。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854358"
---
# <a name="takeown"></a>takeown

使管理员作为文件的所有者，恢复对之前被拒文件的访问权限。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。 此参数适用于的所有文件和命令中指定的文件夹。|
|/u [\<域 >\]<User name>|指定的用户帐户的权限来运行脚本。 默认值为系统权限。|
|/p [\<Password>]|指定在指定的用户帐户的密码 **/u**参数。|
|/f\<文件名称 >|指定的文件名或目录名称模式。 可以使用通配符 * 指定模式时。 此外可以使用语法*ShareName*\*文件名 *。|
|/a|对而不是当前用户的管理员组授予所有权。|
|/r|中指定的目录和子目录执行递归操作上的所有文件。|
|/d {Y \| N}|取消当前用户指定的目录上没有"列出文件夹"权限，而是使用指定的默认值时，将显示确认提示。 有效值 **/d**选项如下所示：</br>-Y:获取目录的所有权。</br>-N:跳过该目录。</br>请注意，必须结合使用此选项 **/r**选项。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   在批处理文件中通常使用此命令。
-   如果 **/a**参数未指定，则文件所有权授予当前登录到计算机的用户。
-   使用混合的模式 (**？** 并 **&#42;**) 不受**takeown**命令。
-   删除与该锁后**takeown**，你可能必须使用 Windows 资源管理器或**cacls**命令来指定您自己的完全权限对文件和目录，才能删除它们。 有关详细信息**cacls**，请参阅本主题末尾的"其他参考"。

## <a name="BKMK_examples"></a>示例

若要获取名为 Lostfile 的文件的所有权，请键入：
```
takeown /f lostfile
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
---
title: takeown
description: 了解如何通过成为文件所有者来获取对文件的访问权限。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 08804db36357c3d1d1efa7243b338bd85d5c48e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383759"
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
|/s \<Computer >|指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。 此参数适用于命令中指定的所有文件和文件夹。|
|/u [@no__t > \] @ no__t-2|用指定用户帐户的权限运行脚本。 默认值为 "系统权限"。|
|/p [\<Password >]|指定在 **/u**参数中指定的用户帐户的密码。|
|/f \<File name >|指定文件名或目录名称模式。 指定模式时，可以使用通配符 *。 还可以使用语法*共享名*\*FileName *。|
|/a|向管理员组而不是当前用户提供所有权。|
|/r|对指定目录和子目录中的所有文件执行递归操作。|
|/d {Y \| N}|取消当当前用户对指定目录没有 "列出文件夹" 权限时显示的确认提示，而是使用指定的默认值。 **/D**选项的有效值如下所示：</br>误差取得目录的所有权。</br>北跳过目录。</br>请注意，必须将此选项与 **/r**选项一起使用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   通常在批处理文件中使用此命令。
-   如果未指定 **/a**参数，则会为当前登录到计算机的用户提供文件所有权。
-   混合模式使用（ **？** takeown **&#42;** 命令不支持和） 。
-   删除**takeown**的锁定后，你可能需要使用 Windows 资源管理器或**cacls**命令向自己授予对文件和目录的完全权限，然后才能删除它们。 有关**cacls**的详细信息，请参阅本主题末尾的 "其他参考"。

## <a name="BKMK_examples"></a>示例

若要获取名为 Lostfile 的文件的所有权，请键入：
```
takeown /f lostfile
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
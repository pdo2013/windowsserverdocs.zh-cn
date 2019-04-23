---
title: prnjobs
description: 了解如何从命令行管理打印作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 03a7f0cf36539272140ea39903bab585b4bc5c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889578"
---
# <a name="prnjobs"></a>prnjobs

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

暂停、 恢复、 取消，并列出了打印作业。

## <a name="syntax"></a>语法
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-z|暂停打印作业使用指定 **-j**参数。|
|-m|恢复具有指定的打印作业 **-j**参数。|
|-x|取消打印作业使用指定 **-j**参数。|
|-l|列出了打印队列中的所有打印作业。|
|-s\<服务器名 >|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|
|-p \<printerName>|指定你想要管理的打印机的名称。 必需。|
|-j \<JobID>|（通过 ID 号） 中指定你想要取消的打印作业。|
|-u \<UserName> -w <Password>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Prnjobs**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 prnjobs 文件或将目录更改为适当的文件夹的完整路径。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs
    ```
-   如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。

## <a name="BKMK_examples"></a>示例
若要暂停作业 id 为 27 发送到远程计算机名 HRServer 为用于名为 colorprinter 打印机上打印的打印作业，请键入：
```
cscript prnjobs -z -s HRServer -p colorprinter -j 27
```
若要列出名为 colorprinter_2 的本地打印机队列中的所有当前打印作业，请键入：
```
cscript prnjobs -l -p colorprinter_2
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)

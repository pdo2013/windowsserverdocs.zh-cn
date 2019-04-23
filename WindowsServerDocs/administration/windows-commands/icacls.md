---
title: icacls
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 20b2150b1135467cce43ae23bfdc275a5da22141
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852638"
---
# <a name="icacls"></a>icacls



显示或修改指定文件上的随机访问控制列表 (DACL)，并将存储的 DACL 应用于指定目录中的文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<FileName>|指定要为其显示 Dacl 的文件。|
|\<目录 >|指定要为其显示 Dacl 的目录。|
|/t|当前目录及其子目录中执行所有指定的文件的操作。|
|/c|将继续操作而不考虑任何文件错误。 仍将显示错误消息。|
|/l|执行上而不是其目标的符号链接的操作。|
|/q|禁止显示成功消息。|
|[/ 保存\<ACLfile > [/t] [/c] [/l] [/q]]|存储的所有匹配文件到 Dacl *ACLfile*以更高版本用于 **/还原**。|
|[/ 出现了 setowner\<用户名 > [/t] [/c] [/l] [/q]]|更改为指定的用户的所有匹配文件的所有者。|
|[/findSID \<Sid> [/t] [/c] [/l] [/q]]|查找包含 DACL 显式一提的指定的安全标识符 (SID) 的所有匹配文件。|
|[/verify [/t] [/c] [/l] [/q]]|找到的所有文件的 Acl 不规范或可以选择此选项与 ACE （访问控制项） 计数不一致。|
|[/reset [/t] [/c] [/l] [/q]]|默认值的替换 Acl 继承的所有匹配文件的 Acl。|
|[授予 / [: r] \<Sid >:<Perm>[...]]|授予指定用户访问权限。 权限将替换以前授予显式权限。</br>无需 **: r**，权限添加到任何以前授予的显式权限。|
|[/ 拒绝\<Sid >:<Perm>[...]]|显式拒绝指定的用户访问权限。 显式拒绝 ACE 中新增的规定权限并删除任何显式授予在相同的权限。|
|[/remove [: g\|: d]] \<Sid > [...]][/t][/c][/l][/q]|从 DACL 中移除指定的 SID 的所有匹配项。</br>**: g**移除到指定的 SID 被授予权限的所有匹配项。</br>**: d**移除对指定的 SID 的拒绝权限的所有匹配项。|
|[/ setintegritylevel [(CI)(OI)]\<级别 >:<Policy>[...]]|显式将完整性 ACE 添加到所有匹配的文件。 *级别*指定为：</br>-   **L**[ow]</br>-   **M**[edium]</br>-   **H**[igh]</br>完整性 ACE 的继承选项之前可能出现在级别，并仅应用于目录。|
|[/substitute \<SidOld> <SidNew> [...]]|替换现有的 SID (*SidOld*) 与一个新的 SID (*SidNew*)。 需要*Directory*参数。|
|/ 还原\<ACLfile > [/c] [/l] [/q]|将应用从存储的 Dacl *ACLfile*到指定目录中的文件。 需要*Directory*参数。|
|/inheritancelevel:[e\|d\|r]|设置的继承级别： <br>  **e** -使 enheritance <br>**d** -禁用继承，并将复制 Ace <br>**r** -移除所有继承 Ace

## <a name="remarks"></a>备注

-   Sid 可能在任一数字或友好名称格式。 如果使用数字形式，词缀通配符 **&#42;** SID 的开头。
-   **icacls**保留 ACE 条目的规范顺序：  
    -   显式拒绝
    -   显式授予
    -   继承被拒绝
    -   继承的授予
-   *为永久*是可以在以下形式之一中指定一个权限掩码：  
    -   一系列简单的权限：

        **F** （完全访问权限）

        **M** （修改访问权限）

        **RX** （读取和执行访问权限）

        **R** （只读访问）

        **W** （只写访问权限）
    -   以逗号分隔的列表，用括号括起来的特定权限：

        **D** （删除）

        **RC** （读取控件）

        **WDAC** （编写 DAC）

        **WO** （写入所有者）

        **S** （同步）

        **AS** （访问系统的安全性）

        **MA** （最多允许）

        **GR** （一般读取）

        **GW** （一般性写）

        **GE** （泛型执行）

        **GA** （所有通用）

        **RD** （读取/列出数据目录）

        **WD** （写入/添加数据的文件）

        **AD** （追加数据/添加子目录）

        **REA** （读取扩展的属性）

        **WEA** （写入扩展的属性）

        **X** （执行/遍历）

        **DC** （删除子）

        **远程协助**（读取属性）

        **WA** （写入属性）
-   继承权限可能位于任一*Perm*窗体中，并且它们仅应用于目录：

    **(OI)**： 对象继承

    **(CI)**： 容器继承

    **(IO)**： 仅继承

    **(NP)**： 将不会传播继承

## <a name="BKMK_examples"></a>示例

若要保存的所有文件的 Dacl C:\Windows 目录及其子目录 ACLFile 文件中，请键入：
```
icacls c:\windows\* /save aclfile /t
```
若要还原的每个文件中存在 C:\Windows 目录及其子目录中的 ACLFile Dacl，请键入：
```
icacls c:\windows\ /restore aclfile
```
若要向用户授予 User1 删除和写入 DAC 权限到名为"Test1"，键入：
```
icacls test1 /grant User1:(d,wdac)
```
若要授予用户定义的 SID 为 S-1-1-0 删除和写入 DAC 权限到文件中，名为"Test2"，键入：
```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

---
title: icacls
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 494c87073cfd78c7f5e17c72d4c65bec33a49b98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375486"
---
# <a name="icacls"></a>icacls

显示或修改指定文件上的随机访问控制列表 (DACL)，并将存储的 DACL 应用于指定目录中的文件。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<文件名 >|指定要为其显示 Dacl 的文件。|
|\<Directory >|指定要为其显示 Dacl 的目录。|
|/t|对当前目录及其子目录中的所有指定文件执行操作。|
|/c|即使存在任何文件错误，也会继续操作。 仍会显示错误消息。|
|/l|对符号链接与目标执行操作。|
|/q|禁止显示成功消息。|
|[/save \<ACLfile > [/t] [/c] [/l] [/q]]|将所有匹配文件的 Dacl 存储到*ACLfile*中，以便以后用于 **/restore**。|
|[/setowner \<Username > [/t] [/c] [/l] [/q]]|将所有匹配文件的所有者更改为指定用户。|
|[/findSID \<Sid > [/t] [/c] [/l] [/q]]|查找所有匹配文件，其中包含显式提及指定安全标识符（SID）的 DACL。|
|[/verify [/t] [/c] [/l] [/q]]|查找其 Acl 不规范或长度与 ACE （访问控制项）计数不一致的所有文件。|
|[/reset [/t] [/c] [/l] [/q]]|将 Acl 替换为所有匹配文件的默认继承 Acl。|
|[/grant [： r] \<Sid >： <Perm> [...]]|授予指定的用户访问权限。 权限替换之前授予的显式权限。</br>如果没有 **： r**，则权限将添加到之前授予的任何显式权限。|
|[/deny \<Sid >： <Perm> [...]]|显式拒绝指定的用户访问权限。 将为所述权限添加显式拒绝 ACE，并删除任何显式授权中的相同权限。|
|[/remove [： g @ no__t-0： d]] \<Sid > [...]]/t/c/l/q|从 DACL 中移除指定 SID 的所有匹配项。</br>**： g**删除授予指定 SID 的所有权限。</br>**:d**删除对指定 SID 的所有拒绝的权限。|
|[/setintegritylevel [（CI）（OI）] \<Level >： <Policy> [...]]|将完整性 ACE 显式添加到所有匹配的文件。 *级别*指定为：</br>-   **L**[o]</br>-   **M**[edium]</br>-   **H**[igh]</br>完整性 ACE 的继承选项可能在级别之前，只适用于目录。|
|[/substitute \<SidOld > <SidNew> [...]]|使用新的 SID （*SidNew*）替换现有 Sid （*SidOld*）。 需要*Directory*参数。|
|/restore \<ACLfile > [/c] [/l] [/q]|将*ACLfile*中存储的 dacl 应用于指定目录中的文件。 需要*Directory*参数。|
|/inheritancelevel： [e @ no__t-0d @ no__t-1r]|设置继承级别： <br>  **e** -启用 enheritance <br>**d** -禁用继承并复制 ace <br>**r** -删除所有继承的 ace

## <a name="remarks"></a>备注

-   Sid 可以是数字或友好名称格式。 如果使用数字形式，请将通配符字符 **&#42;** 的开头。
-   **icacls**保留 ACE 条目的规范顺序：  
    -   显式拒绝
    -   显式授予
    -   继承的拒绝
    -   继承的授权
-   *永久状态*是可以使用以下形式之一指定的权限掩码：  
    -   一系列简单权限：

        **F** （完全访问权限）

        **M** （修改访问权限）

        **RX** （读取和执行访问）

        **R** （只读访问）

        **W** （只写访问）
    -   以逗号分隔的特定权限的列表（以逗号分隔）：

        **D** （删除）

        **RC** （读取控制）

        **WDAC** （写入 DAC）

        **WO** （写入所有者）

        **S** （同步）

        **AS** （访问系统安全性）

        **MA** （允许的最大值）

        **GR** （常规读取）

        **GW** （一般写入）

        **GE** （通用执行）

        **GA** （通用）

        **RD** （读取数据/列表目录）

        **WD** （写入数据/添加文件）

        **AD** （追加数据/添加子目录）

        **REA** （读取扩展属性）

        **WEA** （写入扩展属性）

        **X** （执行/遍历）

        **DC** （删除子项）

        **RA** （读取属性）

        **WA** （编写属性）
-   继承权限可能在*永久状态*形式之前，只适用于目录：

    **（OI）** ：对象继承

    **（CI）** ：容器继承

    **（IO）** ：仅继承

    **（NP）** ：不传播继承

## <a name="examples"></a>示例

若要将 C：\Windows 目录及其子目录中所有文件的 Dacl 保存到 ACLFile 文件，请键入：

```
icacls c:\windows\* /save aclfile /t
```

要还原 ACLFile 中存在的每个文件的 Dacl 及其子目录，请键入：

```
icacls c:\windows\ /restore aclfile
```

若要授予用户 User1 删除和写入名为 "Test1" 的文件的 DAC 权限，请键入：

```
icacls test1 /grant User1:(d,wdac)
```

若要向用户授予 SID S-1-1-0 删除和写入 DAC 权限的用户，请在名为 "Test2" 的文件中键入：

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

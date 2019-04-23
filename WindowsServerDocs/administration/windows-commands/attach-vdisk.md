---
title: 附加的虚拟磁盘
description: Windows 命令主题**附加的虚拟磁盘**-将 （有时称为的装载或应用层协议） 虚拟硬盘 (VHD) 以使其显示本地硬盘驱动器上的主机计算机上。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e715b36f8d9d8b416311567c2455920243dfc7a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885608"
---
# <a name="attach-vdisk"></a>附加的虚拟磁盘

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将附加 （有时称为的装载或应用层协议） 虚拟硬盘 (VHD) 以使其显示本地硬盘驱动器上的主机计算机上。 如果 VHD 已具有磁盘分区和文件系统卷在附加时，在 VHD 内的卷分配驱动器号。
> [!NOTE]
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>语法
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|只读|将附加 VHD 作为只读的。 任何写入操作将返回错误。|
|sd=<SDDL string>|在 VHD 上设置的用户筛选器。 筛选器字符串必须采用安全描述符定义语言 (SDDL) 格式。 默认情况下用户筛选器允许像访问物理磁盘上。<br /><br />SDDL 字符串可能很复杂，但在最简单的形式来保护访问的安全描述符被称为随机访问控制列表 (DACL)。 它是窗体：D:<dacl_flags><string_ace1><string_ace2>... <string_acen><br /><br />常见的 DACL 标志包括：<br /><br />-   **一个**允许访问<br />-   **D**拒绝访问<br /><br />常见权限是：<br /><br />-   **GA**的所有访问<br />-   **GR**读取访问权限<br />-   **GW**写访问权限<br /><br />常见的用户帐户是：<br /><br />-   **BA**内置的管理员<br />-   **澳大利亚**身份验证的用户<br />-   **CO**创建者所有者<br />-   **WD** -每个人<br /><br />示例：<br /><br />**D:P:(A;;GR;;澳大利亚**所有经过身份验证的用户提供读取访问权限<br /><br />**D:P:(A;;GA;;WD**给予每个人的完全访问权限|
|usefilesd|指定应在 VHD 上使用的.vhd 文件上的安全描述符。 如果**Usefilesd**未指定参数，VHD 将没有显式的安全描述符，除非使用指定**Sd**参数。|
|noerr|用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|
## <a name="remarks"></a>备注
-   必须选择并为此操作才能成功分离 VHD。 使用**选择的虚拟磁盘**命令选择 VHD，并将焦点移到它。
## <a name="BKMK_Examples"></a>示例
若要附加为只读的所选的 VHD，请键入：
```
attach vdisk readonly
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
-   [压缩的虚拟磁盘](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [分离的虚拟磁盘](detach-vdisk.md)
-   [展开的虚拟磁盘](expand-vdisk.md)
-   [合并的虚拟磁盘](merge-vdisk.md)
-   [选择的虚拟磁盘](select-vdisk.md)
-   [list_1](list_1.md)

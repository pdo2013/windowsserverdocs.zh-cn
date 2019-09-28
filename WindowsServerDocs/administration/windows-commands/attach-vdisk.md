---
title: 附加 vdisk
description: 用于**附加 vdisk**的 Windows 命令主题（有时称为装载或曲面）是虚拟硬盘（VHD），以使其作为本地硬盘驱动器出现在主计算机上。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382638"
---
# <a name="attach-vdisk"></a>附加 vdisk

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

附加（有时称为装载或曲面）虚拟硬盘（VHD），以使其作为本地硬盘驱动器出现在主计算机上。 如果 VHD 在附加时已有磁盘分区和文件系统卷，则会为 VHD 中的卷分配一个驱动器号。
> [!NOTE]
> 此命令仅适用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>语法
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Parameters

|    参数     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     只读     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             将 VHD 附加为只读。 任何写入操作都将返回错误。                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sd = <SDDL string> | 设置 VHD 上的用户筛选器。 筛选器字符串的格式必须为安全描述符定义语言（SDDL）。 默认情况下，用户筛选器允许访问，就像在物理磁盘上。<br /><br />SDDL 字符串可能比较复杂，但最简单的形式是，保护访问的安全描述符称为自由访问控制列表（DACL）。 其形式如下：D： < dacl_flags > < string_ace1 > < string_ace2 > .。。< string_acen ><br /><br />常见的 DACL 标志包括：<br /><br />-   **A**允许访问<br />@no__t**D**拒绝访问<br /><br />常见权限包括：<br /><br />-   **GA**所有访问权限<br />-   **GR**读取权限<br />-   **GW**写入访问权限<br /><br />常见用户帐户包括：<br /><br />-   **BA**内置管理员<br />-   **AU**验证的用户<br />-   **CO**创建者所有者<br />-   **WD** -Everyone<br /><br />例如：<br /><br />**D:P：（A;;GR;;;AU**为所有经过身份验证的用户提供读取访问权限<br /><br />**D:P：（A;;GA;;;WD**为每个人提供完全的 |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          指定应在 VHD 上使用 .vhd 文件上的安全描述符。 如果未指定**Usefilesd**参数，则 VHD 将不具有明确的安全描述符，除非使用**Sd**参数指定它。                                                                                                                                                                                                                                                                                                                                                                                          |
|      noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>备注
- 必须选择并分离 VHD，此操作才能成功。 使用 "**选择 vdisk** " 命令选择 VHD 并将焦点移动到该 VHD。
  ## <a name="BKMK_Examples"></a>示例
  若要将所选 VHD 附加为只读，请键入：
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)
- [compact vdisk](compact-vdisk.md)

- [详细信息 vdisk](detail-vdisk.md)
- [分离 vdisk](detach-vdisk.md)
- [展开 vdisk](expand-vdisk.md)
- [Merge vdisk](merge-vdisk.md)
- [选择 vdisk](select-vdisk.md)
- [list_1](list_1.md)

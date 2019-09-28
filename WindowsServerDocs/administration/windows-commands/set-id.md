---
title: 设置 id
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5793d7ad-827e-4285-b2c6-ae60eeb0e886
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b48cc701716412c4a79cedddb4458c57ba25ad5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384074"
---
# <a name="set-id"></a>设置 id

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

Diskpart Set ID 命令将更改具有焦点的分区的 "分区类型" 字段。  
  
> [!IMPORTANT]  
> 此命令仅供原始设备制造商 \(OEMs @ no__t。 更改具有此参数的分区类型字段可能导致计算机出现故障或无法启动。 除非你是使用 gpt 磁盘的 OEM 或经验丰富的磁盘，否则不应使用此参数更改 gpt 磁盘上的分区类型字段。 相反，请始终使用[create partition efi](create-partition-efi.md)命令创建 efi 系统分区，使用[create partition Msr](create-partition-msr.md)命令创建 Microsoft 保留分区，并创建不带 ID 参数的[create partition primary](create-partition-primary.md)命令在 gpt 磁盘上创建主分区。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数 |                                                                                                                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       对于主启动记录 \(MBR @ no__t-1 磁盘，以十六进制格式为分区指定 "类型" 字段的新值。 除了指定 LDM 分区的类型0x42 以外，可以使用此参数指定任何分区类型字节。 请注意，在指定十六进制分区类型时，将省略前导0x。                                                                                                                                                                                                       |
|  <GUID>   | 对于 GUID 分区表 \(gpt @ no__t 磁盘，为分区的类型字段指定新的 GUID 值。 可识别的 Guid 包括：<br /><br />-EFI 系统分区： c12a7328 @ no__t-0f81f @ no__t-111d2 @ no__t-2ba4b @ no__t-300a0c93ec93b<br />-Basic data partition： ebd0a0a2 @ no__t-0b9e5 @ no__t-14433 @ no__t-287c0 @ no__t-368b6b72699c7<br /><br />任何分区类型 GUID 都可与此参数一起指定，如下所示：<br /><br />-Microsoft 保留分区： e3c9e316 @ no__t-00b5c @ no__t-14db8 @ no__t-2817d @ no__t-3f92df00215ae<br />-动态磁盘上的 LDM 元数据分区： 5808c8aa @ no__t-07e8f @ no__t-142e0 @ no__t-285d2 @ no__t-3e1e90434cfb3<br />-动态磁盘上的 LDM 数据分区： af9b60a0 @ no__t-01431 @ no__t-14f62 @ no__t-2bc68 @ no__t-33311714a69ad<br />-Cluster metadata partition： db97dba9 @ no__t-00840 @ no__t-14bae @ no__t-297f0 @ no__t-3ffb9a327c7e1 |
| 忽略  |                                                                强制在更改分区类型前卸除卷上的文件系统。 当你运行 "**设置 id** " 命令时，DiskPart 尝试锁定并卸除卷上的文件系统。 如果未指定**override** ，并且调用锁定文件系统失败 \(for 示例，因为有一个打开的句柄 @ no__t-2，则操作将失败。 指定**替代**后，即使对锁定文件系统的调用失败，也会强制卸载，并且卷的任何打开的句柄都将变为无效。<br /><br />此命令仅适用于 Windows 7 和 Windows Server 2008 R2。                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>备注  
  
-   除了前面提到的限制之外，DiskPart 不会检查指定 \(except 的值的有效性，以确保它是十六进制格式的字节或 GUID @ no__t-1。  
  
-   此命令不适用于动态磁盘或 Microsoft 保留分区。  
  
## <a name="BKMK_examples"></a>示例  
若要将 "类型" 字段设置为0x07 并强制卸除文件系统，请键入：  
  
```  
set id=0x07 override  
```  
  
若要将 "类型" 字段设置为基本数据分区，请键入：  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


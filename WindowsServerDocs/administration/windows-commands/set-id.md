---
title: 集 id
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da870c4a9676a08070e22f5391164af0bffd4df0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441338"
---
# <a name="set-id"></a>集 id

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Diskpart 设置 ID 命令将更改具有焦点的分区的分区类型字段。  
  
> [!IMPORTANT]  
> 此命令旨在用于由原始设备制造商\(Oem\)仅。 更改分区类型字段，使用此参数可能会导致计算机故障或无法启动。 除非您是 OEM 或具有丰富 gpt 磁盘经验，不应使用此参数更改 gpt 磁盘上的分区类型字段。 请始终使用[创建分区 efi](create-partition-efi.md)命令创建 EFI 系统分区[创建分区 msr](create-partition-msr.md)命令以创建 Microsoft 保留分区和[创建主分区](create-partition-primary.md)命令而无需在 gpt 磁盘上创建主分区的 ID 参数。  
  
  
  
## <a name="syntax"></a>语法  
  
```  
set id={ <byte> | <GUID> } [override] [noerr]  
```  
  
## <a name="parameters"></a>Parameters  
  
| 参数 |                                                                                                                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                                                                                                                   |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <byte>   |                                                                                                                                                                                                       对于主启动记录\(MBR\)磁盘，指定以十六进制格式，为分区的类型字段的新值。 可以使用除指定 LDM 分区的类型 0x42，此参数指定任何分区类型字节。 请注意指定十六进制分区类型时，省略前导 0x。                                                                                                                                                                                                       |
|  <GUID>   | 对于 GUID 分区表\(gpt\)磁盘，则指定分区类型字段的新 GUID 值。 已识别的 Guid 包括：<br /><br />-   EFI system partition: c12a7328\-f81f\-11d2\-ba4b\-00a0c93ec93b<br />-基本数据分区： ebd0a0a2\-b9e5\-4433\-87 c 0\-68b6b72699c7<br /><br />可以使用除以下此参数指定任何分区类型 GUID:<br /><br />-   Microsoft Reserved partition: e3c9e316\-0b5c\-4db8\-817d\-f92df00215ae<br />的动态磁盘： 5808c8aa 上 LDM 元数据分区\-7e8f\-42e0\-85 d 2\-e1e90434cfb3<br />的动态磁盘上 LDM 数据分区： af9b60a0\-1431年\-4f62\-bc68\-3311714a69ad<br />-群集元数据分区： db97dba9\-0840年\-4bae\-97f0\-ffb9a327c7e1 |
| 重写  |                                                                强制该卷卸载然后再更改分区类型上的文件系统。 在运行时**集 id**命令时，DiskPart 尝试锁定和卸除卷上的文件系统。 如果**重写**未指定，以及锁定文件系统调用失败\(例如，因为没有打开的句柄\)，则操作将失败。 当**重写**指定，则 DiskPart 强制卸除，即使对锁定的文件系统的调用会失败，并且任何打开的句柄到卷将变为无效。<br /><br />此命令才适用于 Windows 7 和 Windows Server 2008 R2。                                                                 |
|   noerr   |                                                                                                                                                                                                                                                                    用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。                                                                                                                                                                                                                                                                    |
  
## <a name="remarks"></a>备注  
  
-   如前面所述的限制，以外 DiskPart 不会检查你指定的值的有效性\(除若要确保它是一个字节，采用 GUID 或十六进制形式\)。  
  
-   在动态磁盘上或 Microsoft 保留分区，此命令不会不起作用。  
  
## <a name="BKMK_examples"></a>示例  
若要将类型字段设置为 0x07 并强制卸除的文件系统，请键入：  
  
```  
set id=0x07 override  
```  
  
若要设置将基本数据分区的类型字段，请键入：  
  
```  
set id=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
  

  


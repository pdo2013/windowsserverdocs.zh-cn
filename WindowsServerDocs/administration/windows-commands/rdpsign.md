---
title: rdpsign
description: 了解如何对 RDP 文件进行数字签名。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: aa1f8f8f31abd85a1ad106a3c4764fc4ccf74258
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384766"
---
# <a name="rdpsign"></a>rdpsign

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使您能够对远程桌面协议（.rdp）文件进行数字签名。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>语法
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/sha1 \<hash >|指定指纹，该指纹是证书存储中包含的签名证书的安全哈希算法1（SHA1）哈希。|
|/q|安静模式。 如果命令成功，则没有输出，如果该命令失败，则输出最小。|
|/v|详细模式。 显示所有警告、消息和状态。|
|/l|测试签名和输出结果，而不实际替换任何输入文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   SHA1 证书指纹应表示受信任的 .rdp 文件发布者。 若要获取证书指纹，请打开 "证书" 管理单元，双击要使用的证书（无论是在本地计算机的证书存储区中还是在个人证书存储区中），单击 "**详细信息**" 选项卡，然后在**字段**列表中，单击 "**指纹**"。

    > [!NOTE]
    > 复制指纹以用于 rdpsign 工具时，必须删除任何空格。

-   必须使用完整的文件名指定要签名的 .rdp 文件。 不接受通配符。
-   已签名的输出文件将覆盖输入文件。
-   如果无法读取或写入任何 .rdp 文件，则该工具将继续到下一个文件（如果指定了多个文件）。

## <a name="BKMK_examples"></a>示例
- 若要对名为 File1 的 .rdp 文件进行签名，请导航到保存 .rdp 文件的文件夹，然后键入以下内容：
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > *哈希*值表示 SHA1 证书指纹，不含任何空格。
- 若要测试 .rdp 文件的数字签名是否会成功，而无需实际对文件进行签名，请键入以下内容：
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- 若要对多个 .rdp 文件进行签名，请使用空格分隔文件名。 例如，若要对名为 File1、File2 和 File3 的多个 .rdp 文件进行签名，请键入以下内容：
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>请参阅
  [命令行语法解答](command-line-syntax-key.md)
  [远程桌面服务&#40;终端服务和&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)

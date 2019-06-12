---
title: rdpsign
description: 了解如何进行数字签名的 RDP 文件。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9e35d3a3e85ed046fb658bbf5a97ab5fc5eec6d3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442009"
---
# <a name="rdpsign"></a>rdpsign

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使您能够远程桌面协议 (.rdp) 文件进行数字签名。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。

## <a name="syntax"></a>语法
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/sha1 \<hash>|指定的指纹，这是包含在证书存储区中的签名证书的安全哈希算法 1 (SHA1) 哈希。|
|/q|安静模式。 没有输出时，命令会成功，并最小输出，如果该命令将失败。|
|/v|详细模式。 显示所有警告、 消息和状态。|
|/l|测试签名和输出结果，而无需实际替换的任何输入文件。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   SHA1 证书指纹应代表可信的.rdp 文件发布服务器。 若要获取证书指纹，请打开证书管理单元中，双击你想要使用 （本地计算机的证书存储区中或在您的个人证书存储区），请单击该证书**详细信息**选项卡上，然后在**字段**列表中，单击**指纹**。

    > [!NOTE]
    > 当复制与 rdpsign.exe 工具一起使用的指纹时，则必须删除任何空格。

-   必须指定.rdp 文件 （或文件） 进行签名通过使用文件的完整名称。 不接受通配符字符。
-   有符号的输出文件将覆盖输入的文件。
-   如果无法读取或写入到的任何.rdp 文件，该工具将继续到下一个文件中，如果指定了多个文件。

## <a name="BKMK_examples"></a>示例
- 登录名为 File1.rdp 的.rdp 文件中，导航到保存.rdp 文件的文件夹和然后键入以下内容：
  ```
  rdpsign /sha1 hash file1.rdp
  ```
  > [!NOTE]
  > *哈希*值表示 SHA1 证书指纹，不包含任何空格。
- 若要测试是否数字签名将会成功的.rdp 文件而无需实际对文件进行签名，请键入以下命令：
  ```
  rdpsign /sha1 hash /l file1.rdp
  ```
- 为多个.rdp 文件签名，请使用空格分隔的文件名。 例如，若要登录多个分别名为 File1.rdp、 File2.rdp 和 File3.rdp 的.rdp 文件，键入以下命令：
  ```
  rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
  ```
  ## <a name="see-also"></a>请参阅
  [命令行语法解答](command-line-syntax-key.md)
  [远程桌面服务 & #40;终端服务和 #41;命令参考](remote-desktop-services-terminal-services-command-reference.md)

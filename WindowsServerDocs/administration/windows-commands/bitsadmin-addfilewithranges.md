---
title: bitsadmin addfilewithranges
description: 适用于**bitsadmin addfilewithranges**的 Windows 命令主题-将文件添加到指定的作业。 BITS 从远程文件下载指定的范围。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382019"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

将文件添加到指定的作业。 BITS 从远程文件下载指定的范围。 此开关仅对下载作业有效。

## <a name="syntax"></a>语法

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|RemoteURL|*RemoteURL*是服务器上的文件的 URL。|
|LocalName|*LocalName*是本地计算机上的文件的名称。 *LocalName*必须包含文件的绝对路径。|
|RangeList|*RangeList*是以逗号分隔的偏移量：长度对的列表。 使用冒号将 offset 值与 length 值分隔开。 例如，如果值为 `0:100,2000:100,5000:eof`，则表示从偏移量0、100字节从偏移2000传输100字节，并将剩余字节从偏移量5000传输到文件末尾。|

## <a name="more-information"></a>详细信息

-   令牌**eof**是在 *\<RangeList >* 的偏移量和长度对中的有效长度值。 它指示服务读取到指定文件的末尾。
-   请注意，当指定长度为零的范围并同时指定一个具有相同偏移量的范围时，AddFileWithRanges 将失败，错误代码为0x8020002c，如：C:\bits > bitsadmin/addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100：0100：5

    错误消息：无法将文件添加到作业-0x8020002c。 字节范围的列表包含一些不受支持的重叠范围。

    解决方法：不要首先指定长度为零的范围。 例如： bitsadmin/addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100：5100：0。

## <a name="examples"></a>示例

下面的示例告知 BITS 要将100字节从偏移量为0、100字节从偏移为2000、从偏移量5000传输到文件末尾。

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
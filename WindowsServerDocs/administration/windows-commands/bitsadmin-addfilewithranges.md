---
title: bitsadmin addfilewithranges
description: Windows 命令主题**bitsadmin addfilewithranges** -将文件添加到指定的作业。 BITS 下载远程文件从指定的范围。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d5e1e4f8af9117928f9ab044d29e65f57aa5a119
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811278"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

将文件添加到指定的作业。 BITS 下载远程文件从指定的范围。 此开关将仅对下载作业有效。

## <a name="syntax"></a>语法

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|RemoteURL|*RemoteURL*是服务器上的文件的 URL。|
|LocalName|*LocalName*是本地计算机上的文件的名称。 *LocalName*必须包含到该文件的绝对路径。|
|RangeList|*RangeList*是以逗号分隔的偏移量： 长度对的列表。 使用冒号分隔的偏移量的值的长度值。 例如，值为`0:100,2000:100,5000:eof`介绍 BITS，用于传输偏移量为 0，100 个字节的偏移量 2000 中，从 100 个字节，并从剩余的字节偏移量 5000 到文件末尾。|

## <a name="more-information"></a>详细信息

-   令牌**eof**是一个有效的长度值中的偏移量和长度对内 *\<RangeList >* 。 它指示服务读取到指定的文件的末尾。
-   请注意 AddFileWithRanges 将失败，错误代码为 0x8020002c 时如以及相同偏移量的另一个范围指定了长度为零的范围：C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0、 100:5

    错误消息：无法将文件添加到作业-0x8020002c。 字节范围的列表包含一些重叠的范围，这不受支持。

    解决方法： 不要先指定长度为零的范围。 例如： bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5、 100:0。

## <a name="examples"></a>示例

下面的示例说明了 BITS，用于传输 100 个字节的偏移量 0，100 个字节从偏移量 2000，以及从剩余的字节偏移量 5000 到文件末尾。

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
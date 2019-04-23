---
title: bitsadmin Transfer
description: Windows 命令主题**bitsadmin 传输**-用于将一个或多个文件。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852458"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

用于将一个或多个文件。 若要传输多个文件，指定多个\<RemoteFileName\>-\<LocalFileName\>对。 对用空格分隔。

## <a name="syntax"></a>语法

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|名称|作业的名称。 与大多数命令不同**名称**只能是一个名称并不是 GUID。|
|在任务栏的搜索框中键入|可选-指定作业的类型。 使用 **/下载**（默认值） 为下载作业或 **/上传**上载作业。|
|Priority|可选-job_priority 设置为以下值之一：</br>的前景色</br>-高</br>-正常</br>-中低|
|ACLFlags|可选-指示你想要保持与所下载的文件的所有者和 ACL 的信息。 例如，若要保持与该文件的所有者和组，设置标志为`OG`。 指定一个或多个下列标志：</br>-O:与文件复制所有者信息。</br>-G:与文件复制组信息。</br>-D:与文件复制 DACL 信息。</br>-S:与文件复制 SACL 信息。|
|\/DYNAMIC|配置具有的作业[ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id)，这会减轻服务器端的要求。|
|RemoteFileName|文件传输到服务器时的名称。|
|LocalFileName|位于本地文件的名称。|

## <a name="remarks"></a>备注

默认情况下，BITSAdmin 服务会创建一个在运行的下载作业**正常**优先级并使用进度信息更新命令窗口中，直到传输完成或发生严重错误。 如果已成功将所有文件，而取消的作业，如果发生严重错误，该服务完成作业。 如果无法将文件添加到作业或您指定的值无效，该服务不会创建作业*类型*或*Job_Priority*。 若要传输多个文件，指定多个*RemoteFileName*-*LocalFileName*对。 对用空格分隔。

> [!NOTE]
> BITSAdmin 命令会继续运行，如果发生暂时性错误。 若要结束该命令，请按 CTRL + C。

## <a name="BKMK_examples"></a>示例

传输作业在以下示例启动名为*myDownloadJob*。
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
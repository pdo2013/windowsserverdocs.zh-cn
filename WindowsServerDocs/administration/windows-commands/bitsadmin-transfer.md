---
title: bitsadmin Transfer
description: '**Bitsadmin 传输**的 Windows 命令主题-传输一个或多个文件。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2a12e6e2023c979d5b0c095c1eddd77eb5155d1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380350"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

传输一个或多个文件。 若要传输多个文件，请指定多个 \<RemoteFileName @ no__t @ no__t-2 @ no__t-3LocalFileName @ no__t-4 对。 对以空格分隔。

## <a name="syntax"></a>语法

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|名称|作业的名称。 与大多数命令不同，**名称**只能是名称而不是 GUID。|
|type|可选-指定作业的类型。 将 **/DOWNLOAD** （默认值）用于下载作业或 **/UPLOAD**用于上载作业。|
|Priority|可选-将 job_priority 设置为以下值之一：</br>-前景</br>-高</br>-正常</br>-低|
|ACLFlags|可选-指示你希望在要下载的文件中维护所有者和 ACL 信息。 例如，若要维护文件的所有者和组，请将标志设置为 `OG`。 指定以下一个或多个标志：</br>I/O将所有者信息复制到文件。</br>G用文件复制组信息。</br>2-D复制 DACL 信息和文件。</br>些将 SACL 信息复制到文件。|
|\/DYNAMIC|将作业配置为[**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id)，这会放宽服务器端要求。|
|RemoteFileName|传输到服务器时文件的名称。|
|LocalFileName|位于本地的文件的名称。|

## <a name="remarks"></a>备注

默认情况下，BITSAdmin 服务创建一个按**正常**优先级运行的下载作业，并在传输完成之前或发生严重错误之前，用进度信息更新命令窗口。 如果作业成功传输所有文件并在发生严重错误时取消作业，则该服务将完成该作业。 如果该服务无法将文件添加到作业中，或者如果为*Type*或*Job_Priority*指定了无效的值，则该服务不会创建作业。 若要传输多个文件，请指定多个*RemoteFileName*-*LocalFileName*对。 对以空格分隔。

> [!NOTE]
> 如果发生暂时性错误，BITSAdmin 命令将继续运行。 若要结束命令，请按 CTRL + C。

## <a name="BKMK_examples"></a>示例

下面的示例启动一个名为*myDownloadJob*的传输作业。
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
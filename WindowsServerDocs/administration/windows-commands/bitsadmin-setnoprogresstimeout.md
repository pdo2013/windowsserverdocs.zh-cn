---
title: bitsadmin setnoprogresstimeout
description: Windows 命令主题**bitsadmin setnoprogresstimeout** -设置的时间，以秒为单位，该服务尝试将文件传输后发生暂时性错误。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873768"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

以秒为单位，BITS 会尝试将文件传输的第一个暂时性错误发生后设置时间的长度。

## <a name="syntax"></a>语法

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|TimeOutvalue|一个以秒为单位表示的数字。|

## <a name="remarks"></a>备注

-   作业遇到暂时性错误时，没有进度超时时间间隔开始计时。
-   超时间隔将停止或重置时成功传输字节的数据。
-   如果没有进度超时间隔超过*TimeOutvalue*，则该作业处于错误状态。

## <a name="BKMK_examples"></a>示例

下面的示例设置名为的作业不将进度超时值*myDownloadJob*为 20 秒
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
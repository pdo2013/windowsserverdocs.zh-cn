---
title: bitsadmin setnoprogresstimeout
description: 适用于**bitsadmin setnoprogresstimeout**的 Windows 命令主题-设置服务在发生暂时性错误后尝试传输文件的时间长度（以秒为单位）。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380497"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

设置在第一次发生暂时性错误后 BITS 尝试传输文件的时间长度（以秒为单位）。

## <a name="syntax"></a>语法

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|TimeOutvalue|以秒表示的数字。|

## <a name="remarks"></a>备注

-   如果作业遇到暂时性错误，则不会开始任何进度超时时间间隔。
-   成功传输字节的数据后，超时间隔将停止或重置。
-   如果没有进度超时间隔超过*TimeOutvalue*，则作业将置于严重错误状态。

## <a name="BKMK_examples"></a>示例

下面的示例将名为*myDownloadJob*的作业的无进度超时值设置为20秒
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
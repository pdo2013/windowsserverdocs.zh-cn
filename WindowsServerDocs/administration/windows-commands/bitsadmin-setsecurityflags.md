---
title: bitsadmin setsecurityflags
description: Windows 命令主题**bitsadmin setsecurityflags** -集标记为确定 BITS 应检查证书吊销列表中，将忽略某些证书错误，以及定义策略时要使用的 HTTP 服务器HTTP 请求重定向。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858618"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags



确定 BITS 应检查证书吊销列表，将忽略某些证书错误，以及定义策略后，服务器将重定向 HTTP 请求时要使用的 http 设置标志。 值是一个无符号的整数。

## <a name="syntax"></a>语法

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ReplTest1|请参阅备注|

## <a name="remarks"></a>备注

**值**参数可以包含一个或多个以下的通知标志。

|操作|二进制表示形式|
|------|---------------------|
|启用 CRL 检查|设置最低有效位|
|忽略服务器证书中的公用名无效|从右侧设置第 2 个字节|
|忽略服务器证书中的日期无效|将第三位设置从右侧|
|忽略无效的证书颁发机构中服务器证书|设置从右侧的第四位|
|忽略无效的证书的使用情况|设置从右侧的第五位|
|重定向策略|控制由从右侧的第 9 个到第 11 位</br>0,0,0-将自动允许重定向。</br>0,0,1-如果发生重定向，将更新 IBackgroundCopyFile 界面中的远程名称。</br>0,1,0-如果发生重定向，BITS 将失败作业。|
|允许从 HTTPS 到 HTTP 的重定向|设置从右侧的第 12 位|

## <a name="BKMK_examples"></a>示例

下面的示例设置以启用名为的作业的 CRL 检查的安全标志*myJob*。
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
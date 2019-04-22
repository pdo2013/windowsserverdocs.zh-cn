---
title: bitsadmin create
description: Windows 命令主题**bitsadmin 创建**-传输作业创建具有给定的显示名称。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817188"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建给定的显示名的传输作业。 从服务器的作业传输数据下载到本地文件中。 上传到服务器的本地文件中的作业传输数据。 上载-答复作业将数据从本地文件传输到服务器，并从服务器接收答复文件。

使用[bitsadmin 恢复](bitsadmin-resume.md)开关来激活的传输队列中的作业。

## <a name="syntax"></a>语法

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|type|-   **/ 下载**将数据从服务器传输到本地文件。<br />-   **/ 上传**将数据从本地文件传输到服务器。<br />-   **/ 上载-答复**将数据从本地文件传输到服务器并从服务器接收答复文件。<br />-此参数默认为 **/下载** 时未指定命令行上。|
|DisplayName|分配给新创建的作业的显示名称。|

**1.2 及更早版本的 BITS**: /Upload 和 /Upload-Reply 类型不可用。

## <a name="BKMK_examples"></a>示例

创建一个名为下载作业 *myDownloadJob*。

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

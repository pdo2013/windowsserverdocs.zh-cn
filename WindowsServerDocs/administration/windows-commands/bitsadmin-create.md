---
title: bitsadmin create
description: '**Bitsadmin create**的 Windows 命令主题-创建具有给定显示名称的传输作业。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381807"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

创建给定的显示名的传输作业。 下载作业：将数据从服务器传输到本地文件。 上载作业：将数据从本地文件传输到服务器。 上载-答复作业：将数据从本地文件传输到服务器，并从服务器接收回复文件。

使用[bitsadmin resume](bitsadmin-resume.md)开关激活传输队列中的作业。

## <a name="syntax"></a>语法

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|type|-    **/Download**将数据从服务器传输到本地文件。<br />-    **/Upload**将数据从本地文件传输到服务器。<br />-    **/Upload-Reply**将数据从本地文件传输到服务器，并从服务器接收回复文件。<br />-此参数默认为 **/下载** 时未指定命令行上。|
|DisplayName|分配给新创建的作业的显示名称。|

**BITS 1.2 及更早版本**： /Upload 和/Upload-Reply 类型不可用。

## <a name="BKMK_examples"></a>示例

创建一个名为下载作业 *myDownloadJob*。

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

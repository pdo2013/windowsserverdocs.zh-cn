---
title: bitsadmin 对等缓存和 getconfigurationflags
description: Windows 命令主题**bitsadmin 对等缓存和 getconfigurationflags** -获取确定计算机是否为对等端提供服务内容的配置标志并可以从对等方下载内容。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826348"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin 对等缓存和 getconfigurationflags



获取确定计算机提供到对等节点的内容以及可以从对等方下载内容的配置标志。

## <a name="syntax"></a>语法

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例获取名为的作业的配置标志*myJob*。
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
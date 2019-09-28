---
title: bitsadmin 对等缓存和 getconfigurationflags
description: 用于 bitsadmin 对等互连**和 getconfigurationflags**的 Windows 命令主题-获取确定计算机是否向对等机提供内容以及能否从对等方下载内容的配置标志。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 94c7eb1a115fe9152b149b8cf65765b179080cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381087"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin 对等缓存和 getconfigurationflags



获取用于确定计算机是否向对等方提供内容以及能否从对等方下载内容的配置标志。

## <a name="syntax"></a>语法

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例获取名为*myJob*的作业的配置标志。
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
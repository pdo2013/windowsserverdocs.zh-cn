---
title: secedit：验证
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece0a0324b77eb4226b679bc29f7bd599f15a120
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371104"
---
# <a name="seceditvalidate"></a>secedit：验证



验证存储在安全模板（.inf 文件）中的安全设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|配置文件名称|必需。</br>指定将验证的安全模板的路径和文件名。|

## <a name="remarks"></a>备注

如果安全模板已损坏或设置不当，则验证这些模板可以帮助你。

将不会应用无效的安全模板。

日志文件将不会更新。

在 Windows Server 2008 中`Secedit /refreshpolicy` ，已`gpupdate`替换为。 有关如何刷新安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

对安全模板执行回滚后，你想要验证回退 inf 文件 secRBKcontoso 是否有效。
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>其他参考

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [命令行语法项](command-line-syntax-key.md)
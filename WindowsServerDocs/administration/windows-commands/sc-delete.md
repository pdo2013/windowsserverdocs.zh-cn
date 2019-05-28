---
title: Sc delete
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe94fb3-e4d1-47b5-b999-39995ecbb644
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68af5f118b2cc9d7941abddccd2a1bc7fde4c6d0
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222928"
---
# <a name="sc-delete"></a>Sc delete



从注册表中删除一个服务的子项。 如果该服务正在运行或另一个进程有一个服务的打开句柄，该服务被标记为删除。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
sc [<ServerName>] delete [<ServiceName>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ServerName>|指定的服务所在的远程服务器的名称。 该名称必须使用通用命名约定 (UNC) 格式 (例如， \\ \\myserver)。 若要本地运行 SC.exe，忽略此参数。|
|\<ServiceName>|指定返回的服务名称**getkeyname**操作。|
|?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

使用**添加或删除程序**上**控制面板**删除 DHCP、 DNS 或任何其他内置操作系统服务。 请注意，**添加或删除程序**只不会删除该服务的注册表子项，但它还将卸载该服务并删除任何快捷方式。

## <a name="examples"></a>示例

若要删除服务注册表子项**NewServ**从本地计算机上注册表中，键入：
```
sc delete newserv
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
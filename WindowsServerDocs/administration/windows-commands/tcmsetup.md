---
title: tcmsetup
description: 了解如何设置和禁用该 TAPI 客户端。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861998"
---
# <a name="tcmsetup"></a>tcmsetup



设置或禁用该 TAPI 客户端。

## <a name="syntax"></a>语法

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/q|禁止显示消息框。|
|/x|指定的面向连接的回调将用于高访问流量网络数据包丢失是高。 如果省略此参数，则将使用连接的回叫。|
|/c|必需。 指定客户端安装程序。|
|\<Server1>|必需。 指定了客户端将使用的 TAPI 服务提供程序的远程服务器的名称。 客户端将使用服务提供商线路和电话。 客户端必须与服务器位于同一域中或在具有与包含该服务器的域的双向信任关系的域中。|
|\<Server2>…|指定此客户端可用的任何其他服务器。 如果指定的服务器列表，请使用空格分隔服务器名称。|
|/d|清除远程服务器的列表。 通过防止使用位于远程服务器的 TAPI 服务提供程序来禁用该 TAPI 客户端。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要执行该过程，你必须是本地计算机上Administrators 组的成员，或你必须已被委派适当的权限。 如果计算机已加入某个域，则 Domain Admins 组的成员可能会执行该过程。 作为安全方面的最佳做法，请考虑使用“运行方式”来执行该过程。
-   为了使 TAPI 才能正常工作，必须运行**tcmsetup**指定 TAPI 客户端将使用的远程服务器。
-   客户端用户可以使用 TAPI 服务器上的电话或线路之前，电话服务器管理员必须将用户分配到电话或线路。
-   此命令创建电话服务器的列表将替换任何现有的客户端可用的电话服务器列表。 不能使用此命令将添加到现有列表。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[命令行解释器概述](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[客户端计算机上指定电话服务器](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[将电话服务用户分配给线路或电话](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)


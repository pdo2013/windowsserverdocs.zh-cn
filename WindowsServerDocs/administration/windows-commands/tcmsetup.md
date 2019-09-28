---
title: tcmsetup
description: 了解如何设置和禁用 TAPI 客户端。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0c646acef51f06c57f16ec7e5310e3319a11383f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370678"
---
# <a name="tcmsetup"></a>tcmsetup



设置或禁用 TAPI 客户端。

## <a name="syntax"></a>语法

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/q|阻止显示消息框。|
|/x|指定面向连接的回调将用于大流量网络，其中数据包丢失很高。 如果省略此参数，则将使用无连接回拨。|
|/c|必需。 指定客户端设置。|
|\<Server1 >|必需。 指定包含客户端将使用的 TAPI 服务提供程序的远程服务器的名称。 客户端将使用服务提供商的线路和电话。 客户端必须与服务器位于同一域中，或位于与包含该服务器的域具有双向信任关系的域中。|
|\<Server2 > 。|指定将可用于此客户端的任何其他服务器。 如果指定服务器的列表，请使用空格分隔服务器名称。|
|/d|清除远程服务器的列表。 通过阻止 TAPI 客户端使用远程服务器上的 TAPI 服务提供程序来禁用该客户端。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   若要执行该过程，你必须是本地计算机上Administrators 组的成员，或你必须已被委派适当的权限。 如果计算机已加入某个域，则 Domain Admins 组的成员可能会执行该过程。 作为安全方面的最佳做法，请考虑使用“运行方式”来执行该过程。
-   为了使 TAPI 正常运行，必须运行**tcmsetup**来指定 tapi 客户端将使用的远程服务器。
-   在客户端用户可以使用 TAPI 服务器上的电话或线路之前，电话服务器管理员必须将该用户分配到电话号码或线路。
-   此命令创建的电话服务器列表将替换任何现有的电话服务器列表，这些服务器可供客户端使用。 不能使用此命令添加到现有列表。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[命令外壳概述](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[指定客户端计算机上的电话服务器](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[将电话服务用户分配给线路或电话](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)


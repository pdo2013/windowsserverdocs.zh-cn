---
title: ksetup:mapuser
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc68fe9e8f4cbb9869cb74e4eb20a3400eb56ad
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437959"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



映射到帐户的 Kerberos 主体名称。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parameters

|  参数   |                                                   描述                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<主体 > |              任何主体; 的完全限定的域名例如， mike@corp.CONTOSO.COM。              |
|  \<Account>  | 此计算机，如来宾、 域用户或管理员存在任何帐户或安全组名称。 |

## <a name="remarks"></a>备注

一个帐户可以专门标识，如域来宾。 或者，可以使用通配符 （*） 以包括所有帐户。

如果省略了帐户名，则映射会删除指定的主体。

如果它们存在有效的 Kerberos 票证，计算机将仅身份验证的主体的给定领域。

使用**ksetup**而无需任何参数或参数，以查看当前映射设置和默认领域。

每当对外部密钥分发中心 (KDC) 和领域配置进行了更改，更改了设置的重新启动是计算机的必需的。

## <a name="BKMK_Examples"></a>示例

在 Kerberos 领域 CONTOSO Mike Danseglio 的帐户映射到此计算机上，他授予内置的来宾帐户的成员的所有权限而无需对此计算机进行身份验证的来宾帐户：
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
删除以防止与他联系到他的凭据与此计算机将从 CONTOSO 的进行身份验证此计算机上的来宾帐户到 Mike Danseglio 的帐户的映射：
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
将 CONTOSO Kerberos 领域内的 Mike Danseglio 的帐户映射到此计算机上的任何现有帐户。 （如果只有标准用户和来宾帐户处于活动状态在此计算机上，Mike 的权限将设置为那些）：
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
将 CONTOSO Kerberos 领域中的所有帐户都映射到此计算机上具有相同名称的任何现有帐户：
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
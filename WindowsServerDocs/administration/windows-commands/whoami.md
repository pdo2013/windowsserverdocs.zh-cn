---
title: whoami
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840148"
---
# <a name="whoami"></a>whoami



显示当前登录到本地系统的用户的用户、 组和权限信息。 如果使用不带参数， **whoami**显示当前的域和用户名称。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/upn|用户主体名称 (UPN) 格式显示的用户名。|
|/fqdn|显示完全限定的域名 (fqdn) 格式的用户名。|
|/logonid|显示当前用户的登录 ID。|
|/user|显示当前的域和用户名称和安全标识符 (SID)。|
|/groups|显示当前用户所属的用户组。|
|/priv|显示当前用户的安全特权。|
|/fo\<格式 >|指定的输出格式。 有效值包括：</br>**表**表中显示输出。 这是默认值。</br>**列表**以列表形式显示输出。</br>**csv**以逗号分隔值 (CSV) 格式显示输出。|
|/all|显示当前访问令牌，包括当前用户名称、 安全标识符 (SID)、 权限和当前用户所属的组中的所有信息。|
|/nh|指定列标题不应显示在输出中。 此值仅适用于表和 CSV 格式有效。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要显示当前已登录到此计算机的人员的域和用户名称，请键入：
```
whoami
```
将显示类似于以下输出：
```
DOMAIN1\administrator
```
若要显示当前的访问令牌中的所有信息，请键入：
```
whoami /all
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
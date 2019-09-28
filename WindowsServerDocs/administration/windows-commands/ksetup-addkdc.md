---
title: ksetup： addkdc
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66efe4e56007aff39b83c92dfea2afaadcfc0210
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375209"
---
# <a name="ksetupaddkdc"></a>ksetup： addkdc



添加给定 Kerberos 领域的密钥发行中心（KDC）地址。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<RealmName >|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM，在**ksetup**运行时，它将作为默认领域列出。 这是你尝试添加其他 KDC 的领域。|
|\<KDCName >|KDC 名称被声明为不区分大小写的完全限定域名，如 mitkdc.microsoft.com。 如果省略了 KDC 名称，DNS 将定位 Kdc。|

## <a name="remarks"></a>备注

这些映射存储在**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**下的注册表中。 若要将 Kerberos 领域配置数据部署到多台计算机，请使用安全配置模板管理单元和策略分发，而不是在单独的计算机上显式使用**ksetup** 。

必须重新启动计算机，然后才能使用新领域设置。

若要验证计算机的默认领域名称，或者要验证此命令是否按预期运行，请在命令提示符下运行**ksetup** ，并验证所添加 KDC 的输出。

## <a name="BKMK_Examples"></a>示例

配置非 Windows KDC 服务器和工作站应使用的领域：
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
在与前一个命令相同的计算机的命令行上运行 Ksetup 工具，将本地计算机帐户密码设置为 "p@sswrd1%"。 然后重新启动计算机。
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>其他参考

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [命令行语法项](command-line-syntax-key.md)
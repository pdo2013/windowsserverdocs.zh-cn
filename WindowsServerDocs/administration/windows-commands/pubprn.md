---
title: pubprn
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372012"
---
# <a name="pubprn"></a>pubprn

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将打印机发布到 active directory 域服务。

## <a name="syntax"></a>语法
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|\<ServerName >|指定承载要发布的打印机的 Windows 服务器的名称。 如果未指定计算机，则使用本地计算机。|
|\<UNCprinterpath >|要发布的共享打印机的通用命名约定（UNC）路径。|
|"LDAP://CN = <Container>，DC = <Container>"|指定要在其中发布打印机的 active directory 域服务中的容器的路径。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Pubprn**命令是位于%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t 目录中的 Visual Basic 脚本。 若要使用此命令，请在命令提示符下键入**cscript** ，后跟 pubprn 文件的完整路径，或将目录更改为相应的文件夹。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   如果提供的信息包含空格，请使用引号将文本括起来（例如 `"computer Name"`）。

## <a name="BKMK_examples"></a>示例
若要将 \\ \ Server1 计算机上的所有打印机发布到 MyDomain.company.Com 域中的 MyContainer 容器，请键入：
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
若要将 \\ \ Server1 服务器上的 Laserprinter1 打印机发布到 MyDomain.company.Com 域中的 MyContainer 容器，请键入：
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)

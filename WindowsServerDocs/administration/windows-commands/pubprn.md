---
title: pubprn
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831698"
---
# <a name="pubprn"></a>pubprn

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将打印机发布到 active directory 域服务。

## <a name="syntax"></a>语法
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|\<ServerName>|指定承载要发布的打印机的 Windows 服务器的名称。 如果不指定计算机，则使用本地计算机。|
|\<UNCprinterpath>|到想要发布的共享打印机的通用命名约定 (UNC) 路径。|
|"LDAP://CN=<Container>,DC=<Container>"|指定你想要将打印机发布的 active directory 域服务中的容器的路径。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Pubprn**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 pubprn 文件或将目录更改为适当的文件夹的完整路径。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。

## <a name="BKMK_examples"></a>示例
若要发布的所有打印机在\\\Server1 计算机添加到的 MyContainer 容器中 MyDomain.company.Com 域中，键入：
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
若要发布在 Laserprinter1 打印机\\\Server1 服务器到 MyContainer 容器 MyDomain.company.Com 域类型中：
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)

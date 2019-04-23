---
title: cacls
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 289015ff580d7e2fba5ff911878028f5302cab8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838078"
---
# <a name="cacls"></a>cacls

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示或修改指定文件的自由访问控制列表 (DACL)。  
## <a name="syntax"></a>语法  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|\<filename\>|必需。 显示指定文件的 Acl。|  
|/t|更改当前目录及所有子目录中的指定文件的 Acl。|  
|/m|更改 Acl 的卷装载到目录中。|  
|/l|适用于符号链接本身而不是目标。|  
|/s:sddl|使用中的 SDDL 字符串指定替换 Acl (不有效，且 **/e**， **/g**， **/r**， **/p**，或 **/d**).|  
|/e|编辑 ACL 而不是替换它。|  
|/c|拒绝访问错误时继续。|  
|/g 用户：\<为永久\>|授予指定用户访问权限。<br /><br />有效权限的值：<br /><br />-n-无<br />-r-读取<br />-w-写入<br />-c-更改 （写入）<br />-f-完全控制|  
|/r 用户 [...]|撤消指定的用户的访问权限 (仅有效，且 **/e**)。|  
|[/p user:\<perm\> [...]|将为指定的用户的访问权限。<br /><br />有效权限的值：<br /><br />-n-无<br />-r-读取<br />-w-写入<br />-c-更改 （写入）<br />-f-完全控制|  
|[/d 用户 [...]|拒绝指定的用户访问。|  
|/?|在命令提示符下显示帮助。|  
## <a name="remarks"></a>备注  
-   此命令已被弃用。 请使用[icacls](icacls.md)相反。  
-   使用下表解释结果：  

    |输出|访问控制项 (ACE) 适用于|  
    |-----|----------------------|  
    |OI|对象继承。 此文件夹和文件。|  
    |CI|容器继承。 此文件夹和子文件夹。|  
    |IO|仅继承。 ACE 不适用于当前的文件/目录。|  
    |没有输出消息|仅此文件夹中。|  
    |(OI)(CI)|此文件夹、 子文件夹和文件。|  
    |(OI)(CI)(IO)|子文件夹和文件。|  
    |(CI)(IO)|仅限子文件夹。|  
    |(OI)(IO)|仅限文件。|  

-   你可以使用通配符 (**？** 并**\***) 若要指定多个文件。  
-   您可以指定多个用户。  

#### <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)   
-   [icacls](icacls.md)  

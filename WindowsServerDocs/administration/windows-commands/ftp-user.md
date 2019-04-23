---
title: ftp 用户
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef3b943491a90078dab453aaf3a037bd4ccf1825
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887488"
---
# <a name="ftp-user"></a>ftp： 用户

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

指定到远程计算机的用户。   
## <a name="syntax"></a>语法  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<UserName>|指定用来登录到远程计算机上的用户名称。|  
|[<Password>]|指定的密码*用户名*。 如果未指定密码，但是是必需的**ftp**提示输入密码。|  
|[<Account>]|指定用来登录到远程计算机上的帐户。 如果*帐户*必需的但未指定**ftp**提示输入该帐户。|  
## <a name="BKMK_Examples"></a>示例  
指定使用密码 Password1 User1。  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

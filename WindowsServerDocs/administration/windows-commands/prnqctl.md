---
title: prnqctl
description: 打印测试页、 暂停或继续打印机。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ba58970e76497f6e91c53c73a429eb65a275b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442102"
---
# <a name="prnqctl"></a>prnqctl

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

打印测试页、 暂停或继续打印，并清除打印机队列。  

## <a name="syntax"></a>语法  
```  
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <ServerName>]   
[-p <printerName>] [-u <UserName>] [-w <Password>]  
```  
## <a name="parameters"></a>Parameters  

|参数|描述|  
|-------|--------|  
|-z|暂停上使用指定的打印机的打印 **-p**参数。|  
|-m|恢复具有指定的打印机上打印 **-p**参数。|  
|-e|使用指定打印机上打印测试页 **-p**参数。|  
|-x|取消具有指定的打印机上的所有打印作业 **-p**参数。|  
|-s\<服务器名 >|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|  
|-p \<printerName>|指定你想要管理的打印机的名称。 必需。|  
|-u \<UserName> -w \<Password>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
- **Prnqctl**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 prnqctl 文件或将目录更改为适当的文件夹的完整路径。 例如：  
  ```  
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl  
  ```  
- 如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。  

## <a name="BKMK_examples"></a>示例  
若要共享的 Laserprinter1 打印机上打印测试页\\\Server1 计算机中，键入：  
```  
cscript Prnqctl -e -s Server1 -p Laserprinter1  
```  
若要暂停本地计算机上的 Laserprinter1 打印机上打印，请键入：  
```  
cscript Prnqctl -z -p Laserprinter1  
```  
若要取消 Laserprinter1 打印机上的本地计算机上的所有打印作业，请键入：  
```  
cscript Prnqctl -x -p Laserprinter1  
```  

#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
[打印命令参考](print-command-reference.md)  

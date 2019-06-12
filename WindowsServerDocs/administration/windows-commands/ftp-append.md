---
title: ftp 追加
description: 'Ftp 的 Windows 命令主题追加 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9580d725120bb32a9b915d37cdbc173bfb17b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438843"
---
# <a name="ftp-append"></a>ftp： 追加

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将本地文件追加到用当前的文件类型设置的远程计算机上的文件。   
## <a name="syntax"></a>语法  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parameters  

|  参数   |                               描述                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     指定要添加的本地文件。                     |
| [remoteFile] | 指定到的远程计算机上的文件<LocalFile>添加。 |

## <a name="remarks"></a>备注  
如果*remoteFile*省略，则*LocalFile*名称用来远程文件名代替。  
## <a name="BKMK_Examples"></a>示例  
追加到远程计算机上的 file2.txt file1.txt。  
```  
append file1.txt file2.txt  
```  
将本地 file1.txt 追加到名为 file1.txt 远程计算机上的文件。  
```  
append file1.txt  
```  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

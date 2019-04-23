---
title: mqbkup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842898"
---
# <a name="mqbkup"></a>mqbkup

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

备份 MSMQ 消息文件和注册表设置到的存储设备并将还原以前存储的消息和设置。   
备份和还原操作将停止本地 MSMQ 服务。 如果已事先启动 MSMQ 服务，该实用工具将尝试重新启动 MSMQ 服务备份或还原操作的末尾。 如果服务已经停止运行该实用程序之前，由不会尝试重新启动服务。  
在使用 MSMQ 消息备份/还原实用程序之前必须关闭所有正在使用 MSMQ 的本地应用程序。  
## <a name="syntax"></a>语法  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/b|指定备份操作|  
|/r|指定还原操作|  
|<folder path_to_storage\_device>|指定 MSMQ 消息文件和注册表设置的存储位置的路径|  
|/?|在命令提示符下显示帮助。|  
## <a name="BKMK_Examples"></a>示例  
若要备份所有 MSMQ 消息文件和注册表设置并将存储在*Msmqbkup* c： 驱动器上的文件夹。  
```  
mqbkup /b c:\msmqbkup  
```  
如果指定的文件夹不存在，该实用程序将自动创建一个。 如果您选择指定现有文件夹，此文件夹必须为空。 如果指定非空文件夹，该实用工具将删除每个文件和它所包含的子文件夹。 在这种情况下，系统会提示要授予权限，删除现有文件和子文件夹。 可以使用 **/y**参数来指示，即表示你同意提前到的所有现有文件和子文件夹指定文件夹中的删除。  
若要删除所有文件和子文件夹中的*Oldbkup*文件夹 c： 驱动器以及存储 MSMQ 消息文件和注册表设置，此文件夹中的。  
```  
mqbkup /b /y c:\oldbkup  
```  
若要还原的 MSMQ 消息和注册表设置：  
```  
mqbkup /r c:\msmqbkup  
```  
用于存储 MSMQ 消息文件文件夹的位置存储在注册表中。 因此，该实用工具将还原 MSMQ 消息文件到注册表中指定的文件夹而不执行还原操作之前使用的存储文件夹。 如果在注册表中指定的文件夹不存在，还原操作将自动创建它们。 如果文件夹目录存在，并且不为空，该实用程序将提示您输入删除这些文件夹的当前内容的权限。  
## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  

---
title: mqbkup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66783e0bbfe5c82971e14fd05e913d485485dc6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373508"
---
# <a name="mqbkup"></a>mqbkup

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将 MSMQ 消息文件和注册表设置备份到存储设备，并还原以前存储的消息和设置。   
备份和还原操作都将停止本地 MSMQ 服务。 如果已预先启动 MSMQ 服务，则实用工具将在备份或还原操作结束时尝试重新启动 MSMQ 服务。 如果服务在运行此实用工具之前已停止，则不会尝试重新启动该服务。  
使用 MSMQ 消息备份/还原实用程序之前，必须先关闭所有使用 MSMQ 的本地应用程序。  
## <a name="syntax"></a>语法  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/b|指定备份操作|  
|/r|指定还原操作|  
|< 文件夹 path_to_storage @ no__t-0device >|指定 MSMQ 消息文件和注册表设置的存储路径|  
|/?|在命令提示符下显示帮助。|  
## <a name="BKMK_Examples"></a>示例  
若要备份所有 MSMQ 消息文件和注册表设置，并将它们存储在 C：驱动器上的*Msmqbkup*文件夹中。  
```  
mqbkup /b c:\msmqbkup  
```  
如果指定的文件夹不存在，实用工具将自动创建一个。 如果选择指定现有文件夹，则此文件夹必须为空。 如果指定非空文件夹，实用工具将删除其中包含的每个文件和子文件夹。 在这种情况下，系统将提示你授予删除现有文件和子文件夹的权限。 您可以使用 **/y**参数指示您事先同意删除指定文件夹中的所有现有文件和子文件夹。  
若要删除 C：驱动器上*Oldbkup*文件夹中的所有文件和子文件夹，并将 MSMQ 消息文件和注册表设置存储在此文件夹中。  
```  
mqbkup /b /y c:\oldbkup  
```  
还原 MSMQ 消息和注册表设置：  
```  
mqbkup /r c:\msmqbkup  
```  
用于存储 MSMQ 消息文件的文件夹的位置存储在注册表中。 因此，实用工具会将 MSMQ 消息文件还原到注册表中指定的文件夹，而不是还原操作之前使用的存储文件夹。 如果在注册表中指定的文件夹不存在，则还原操作将自动创建它们。 如果文件夹目录存在且不为空，则实用工具将提示你是否有权删除这些文件夹的当前内容。  
## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  

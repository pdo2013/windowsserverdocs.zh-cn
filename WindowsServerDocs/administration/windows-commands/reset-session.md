---
title: reset session
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5a0991c76ba890bb94b0dcf258df6207ed228e72
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441792"
---
# <a name="reset-session"></a>reset session

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使您能够重置远程桌面会话主机 (rd 会话主机) 服务器上的会话 （删除）。  
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。  

## <a name="syntax"></a>语法  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Parameters  

|参数|描述|  
|-------|--------|  
|\<SessionName>|指定你想要重置的会话的名称。 若要确定会话的名称，请使用**查询会话**命令。|  
|\<SessionID>|指定要重置的会话 ID。|  
|/server:\<服务器名 >|指定包含你想要重置的会话的终端服务器。 否则，使用当前的 rd 会话主机服务器。|  
|/v|显示有关正在执行的操作的信息。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   始终可以重置您自己的会话，但必须具有完全控制访问权限，才能重置其他用户的会话。  
-   请注意，不警告用户重置用户的会话可导致该会话中的数据丢失。  
-   应重置会话仅它运行不正常或似乎已停止响应。  
-   **/Server**参数是必需的仅当你使用**重置会话**从远程服务器。  

## <a name="BKMK_examples"></a>示例  
- 若要重置指定 rdp-tcp #6 的会话，请键入：  
  ```  
  reset session rdp-tcp#6  
  ```  
- 若要重置使用会话 ID 为 3 的会话，请键入：  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
[远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)  

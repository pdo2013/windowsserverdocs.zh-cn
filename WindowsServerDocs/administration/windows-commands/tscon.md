---
title: tscon
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8cac59b2f5524df5a82e9c83424fd781f0ef7c8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440936"
---
# <a name="tscon"></a>tscon

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

连接到远程桌面会话主机 (rd 会话主机) 服务器上的另一个会话。  
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解什么是最新版本中的新增功能，请参阅[的新增功能新增 Windows Server 2012 中的远程桌面服务中](https://technet.microsoft.com/library/hh831527)Windows Server TechNet 库中。  

## <a name="syntax"></a>语法  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parameters  

|参数|描述|  
|-------|--------|  
|\<SessionID>|指定你想要连接的会话的 ID。 如果使用可选 **/dest:** <*SessionName*> 参数，这是你想要连接的会话的 ID。|  
|\<SessionName>|指定你想要连接的会话的名称。|  
|/dest:\<SessionName>|指定当前会话的名称。 连接到新的会话时，此会话将断开连接。|  
|/password:\<pw >|指定拥有你想要连接的会话的用户的密码。 连接的用户不拥有会话时，此密码是必需的。|  
|/password:*|拥有你想要连接的会话的用户的密码的提示。|  
|/v|显示有关正在执行的操作的信息。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   必须将完全控制访问权限，或连接到另一个会话连接的特殊访问权限。  
-   **/Dest:** <*SessionName*> 参数，可将其他用户的会话连接到另一个会话。  
-   如果未指定密码中的 <*密码*> 参数，并且目标会话属于用户而不是当前**tscon**失败。  
-   无法连接到控制台会话。  

## <a name="BKMK_examples"></a>示例  
- 若要连接到当前的 rd 会话主机服务器上的会话 12 并断开当前会话的连接，请键入：  
  ```  
  tscon 12  
  ```  
- 若要使用密码 mypas 当前 rd 会话主机服务器上的会话 23 连接和断开当前会话的连接，请键入：  
  ```  
  tscon 23 /password:mypass  
  ```  
- 若要连接到名为 TERM05 会话名为 TERM03 的会话，然后断开会话 TERM05，如果它已连接，请键入：  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>其他参考  
  [命令行语法项](command-line-syntax-key.md)  
  [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)  

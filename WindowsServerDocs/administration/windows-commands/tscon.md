---
title: tscon
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6e53ea2888b66b9e4fbf026f752acf9803270fc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392367"
---
# <a name="tscon"></a>tscon

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

连接到远程桌面会话主机（rd 会话主机）服务器上的另一个会话。  
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。  

> [!NOTE]  
> 在 Windows Server 2008 R2 中，终端服务被重命名为远程桌面服务。 若要了解最新版本中的新增功能，请参阅 Windows server TechNet 库中的[Windows server 2012 远程桌面服务中的新增功能](https://technet.microsoft.com/library/hh831527)。  

## <a name="syntax"></a>语法  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Parameters  

|参数|描述|  
|-------|--------|  
|\<SessionID >|指定要连接到的会话的 ID。 如果使用可选的 **/dest：** <*SessionName*> 参数，则这是要连接到的会话的 ID。|  
|\<SessionName >|指定要连接到的会话的名称。|  
|/dest： \<SessionName >|指定当前会话的名称。 当你连接到新的会话时，此会话将断开连接。|  
|/password： \<pw >|指定拥有要连接到的会话的用户的密码。 当连接用户不拥有会话时，此密码是必需的。|  
|/password： *|提示输入拥有要连接到的会话的用户的密码。|  
|/v|显示要执行的操作的相关信息。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
-   您必须具有 "完全控制" 访问权限或 "连接" 特殊访问权限才能连接到另一个会话。  
-   **/Dest：** <*SessionName*> 参数使你可以将另一个用户的会话连接到不同的会话。  
-   如果未在 <*password*> 参数中指定密码，并且目标会话所属的用户不是当前用户，则**tscon**会失败。  
-   你无法连接到控制台会话。  

## <a name="BKMK_examples"></a>示例  
- 若要连接到当前 rd 会话主机服务器上的会话12并断开当前会话的连接，请键入：  
  ```  
  tscon 12  
  ```  
- 若要通过使用密码 mypass 连接到当前 rd 会话主机服务器上的会话23并断开当前会话的连接，请键入：  
  ```  
  tscon 23 /password:mypass  
  ```  
- 若要将名为 TERM03 的会话连接到名为 TERM05 的会话，然后断开连接会话 TERM05 （如果已连接），请键入：  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>其他参考  
  [命令行语法项](command-line-syntax-key.md)  
  [远程桌面服务&#40;终端服务&#41;命令参考](remote-desktop-services-terminal-services-command-reference.md)  

---
title: cmdkey
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2b12cb53eef930d05c1e291de5574a8ba94306
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379309"
---
# <a name="cmdkey"></a>cmdkey

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

创建、列出并删除存储的用户名和密码或凭据。

## <a name="syntax"></a>语法
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parameters

|             Parameters             |                                                                                    描述                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /add： <TargetName>          | 向列表添加用户名和密码。<br /><br />需要 <TargetName> 的参数，该参数标识此项将与之关联的计算机或域名。 |
|       /常规： <TargetName>        |   向列表中添加一般凭据。<br /><br />需要 <TargetName> 的参数，该参数标识此项将与之关联的计算机或域名。    |
|             /smartcard             |                                                                    从智能卡中检索凭据。                                                                     |
|          /user： <UserName>          |                                 指定要与此条目一起存储的用户或帐户名称。 如果未提供*用户名*，则会请求它。                                  |
|          /pass： <Password>          |                                       指定要与此项一起存储的密码。 如果未提供密码，则会请求*密码*。                                        |
| /delete{： <TargetName> &#124; /ras} |  从列表中删除用户名和密码。 如果指定了*TargetName* ，则该项将被删除。 如果指定了/ras，则将删除存储的远程访问条目。   |
|         /list： <TargetName>         |                  显示存储的用户名和凭据的列表。 如果未指定*TargetName* ，则会列出所有存储的用户名和凭据。                   |
|                 /?                 |                                                                        在命令提示符下显示帮助。                                                                        |

## <a name="remarks"></a>备注
- 当使用/smartcard 命令行选项时，如果在系统上找到了多个智能卡，则**cmdkey**将显示所有可用智能卡的相关信息，然后提示用户指定要使用的智能卡。
- 存储密码后，不会显示密码。
  ## <a name="BKMK_examples"></a>示例
  若要显示所有已存储用户名和凭据的列表，请键入：
  ```
  cmdkey /list
  ```
  若要为用户 Mikedan 添加用户名和密码，以便使用 password Kleo 访问计算机 Server01，请键入：
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  若要为用户 Mikedan 添加用户名和密码以访问计算机 Server01，并在访问 Server01 时提示输入密码，请键入：
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  若要删除远程访问已存储的凭据，请键入：
  ```
  cmdkey /delete /ras
  ```
  若要删除为 Server01 存储的凭据，请键入：
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

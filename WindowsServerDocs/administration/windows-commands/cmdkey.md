---
title: cmdkey
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5c06a04fa6473bc30c3b354f049a55775d2308a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434310"
---
# <a name="cmdkey"></a>cmdkey

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建、列出并删除存储的用户名和密码或凭据。

## <a name="syntax"></a>语法
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parameters

|             Parameters             |                                                                                    描述                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         / 添加：<TargetName>          | 向列表添加用户名和密码。<br /><br />需要的参数<TargetName>用于标识此项将与之关联的计算机或域的名称。 |
|       / 常规：<TargetName>        |   将泛型凭据添加到列表。<br /><br />需要的参数<TargetName>用于标识此项将与之关联的计算机或域的名称。    |
|             /smartcard             |                                                                    从智能卡来检索凭据。                                                                     |
|          /user:<UserName>          |                                 指定要与此项将存储的用户或帐户名称。 如果*用户名*是未提供，它将请求。                                  |
|          /pass:<Password>          |                                       指定要与此项将存储的密码。 如果*密码*是未提供，它将请求。                                        |
| /delete{:<TargetName> &#124; /ras} |  从列表中删除的用户名和密码。 如果*TargetName*指定，则将删除条目。 如果指定 /ras，则将删除存储的远程访问条目。   |
|         /list:<TargetName>         |                  显示存储的用户名和凭据的列表。 如果*TargetName*不指定，则所有存储的用户名，将列出凭据。                   |
|                 /?                 |                                                                        在命令提示符下显示帮助。                                                                        |

## <a name="remarks"></a>备注
- 如果使用 /smartcard 命令行选项时，在系统中找到多个智能卡**cmdkey**将显示有关所有可用的智能卡的信息，然后提示用户指定应使用哪一个。
- 它们存储后，将不显示密码。
  ## <a name="BKMK_examples"></a>示例
  若要显示的所有用户名称和存储的凭据列表，请键入：
  ```
  cmdkey /list
  ```
  若要将用户名和用户 Mikedan 密码添加到具有密码 Kleo 访问计算机 Server01，键入：
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  若要访问 Server01 时添加的用户名和用户访问计算机 Server01 Mikedan 密码和密码提示，请键入：
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  若要删除的远程访问已存储的凭据，请键入：
  ```
  cmdkey /delete /ras
  ```
  若要删除 Server01 为存储的凭据，请键入：
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

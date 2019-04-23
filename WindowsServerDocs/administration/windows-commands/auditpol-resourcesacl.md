---
title: auditpol resourceSACL
description: Windows 命令主题**uditpol resourceSACL** -配置全局资源系统访问控制列表 (Sacl)。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837488"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL



配置全局资源系统访问控制列表 (Sacl)。

> [!NOTE]
> 仅适用于 Windows 7 和 Windows Server 2008 R2。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/set|添加到新的条目或更新资源的资源 SACL 中的现有条目指定的类型。|
|/remove|全局对象访问审核列表中移除给定的用户的所有项。|
|/ 清除|从全局对象访问审核列表中移除所有项。|
|/view|列出资源 SACL 中的全局对象访问审核项。 用户和资源类型是可选的。|
|/?|在命令提示符下显示帮助。|

## <a name="arguments"></a>参数

|参数|描述|
|--------|-----------|
|/type|对象访问审核正在配置的资源。 受支持的参数值为文件 （对于目录和文件） 和密钥 （用于注册表项）。</br>注意：文件和密钥值是区分大小写。|
|/success|指定成功审核。|
|/failure|指定失败审核。|
|/user|指定用户的以下形式之一：</br>-DomainName\Account （如 DOM\Administrators)</br>-StandaloneServer\Group 帐户 (请参阅[LookupAccountName 函数](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} （x 表示十进制、 和整个 SID 必须括在大括号中）;例如: {S-1-5-21-5624481-130208933-164394174-1001}</br>    注意：   如果使用 SID 窗体，则不进行检查是为了验证存在此帐户。|
|/access|指定的权限掩码，可以指定在两种形式之一：</br>-一系列简单的权限：</br>    -通用访问权限：</br>        -   GA - GENERIC ALL</br>        -GR-一般读取</br>        -   GW - GENERIC WRITE</br>        -GX-泛型执行</br>    文件的访问权限：</br>        -FA 的文件的所有访问</br>        泛型-FR-文件读取</br>        -FW-文件一般写入</br>        -FX-文件泛型执行</br>    注册表项的访问权限：</br>        -KA-密钥的所有访问</br>        读取-韩国-密钥</br>        -KW-密钥写入</br>        -KX-密钥执行</br>    例如: / 访问权限： FRFW 将启用审核事件进行读取和写入操作</br>-一个十六进制值，表示 （如 0x1200a9) 的访问掩码</br>    使用特定于资源的位掩码，不是安全描述符定义语言 (SDDL) 标准的一部分时，这非常有用。 如果省略，则使用完全访问权限。|

## <a name="remarks"></a>备注

对于 resourceSACL 操作，必须在安全描述符中设置该对象具有写入或完全控制权限。 此外可以通过拥有执行 resourceSACL operations**管理审核和安全日志**(SeSecurityPrivilege) 用户权限。 但是，此权限允许其他不是执行删除操作所需的访问。

## <a name="BKMK_Examples"></a>示例

要设置的全局资源 SACL 以审核成功的访问尝试通过注册表项上的用户：
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
要设置的全局资源 SACL 以审核成功和失败尝试次数由用户执行一般读取和写入文件或文件夹上的函数：
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
若要删除的文件或文件夹的所有全局资源 SACL 条目：
```
auditpol /resourceSACL /type:File /clear
```
若要从文件或文件夹中删除特定用户的所有全局资源 SACL 条目：
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
若要列出的全局对象访问文件或文件夹上设置的审核条目：
```
auditpol /resourceSACL /type:File /view
```
若要列出的全局对象访问特定用户的文件或文件夹设置的审核条目：
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
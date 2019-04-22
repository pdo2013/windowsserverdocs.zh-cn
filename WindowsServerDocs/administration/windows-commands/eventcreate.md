---
title: eventcreate
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818708"
---
# <a name="eventcreate"></a>eventcreate



使管理员能够在指定的事件日志中创建自定义事件。 有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u \<Domain\User>|运行该命令使用指定的用户帐户权限\<用户 > 或 < 域 \ 用户 >。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/l {APPLICATION\|SYSTEM}|指定在其中创建该事件的事件日志的名称。 有效的日志名称为应用程序和系统。|
|/so \<SrcName>|指定要使用的事件的源。 有效的源可以是任意字符串，并应表示应用程序或生成事件的组件。|
|/t {错误\|警告\|信息\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|指定要创建的事件的类型。 有效的类型为错误、 警告、 信息、 SUCCESSAUDIT 和 FAILUREAUDIT。|
|/id \<EventID >|指定事件的事件 ID。 有效的 ID 是从 1 到 1000年的任意数字。|
|/d\<说明 >|指定要用于新创建的事件的描述。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   不能将自定义事件写入安全日志。

## <a name="BKMK_examples"></a>示例

下面的示例演示如何使用 eventcreate 命令：
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

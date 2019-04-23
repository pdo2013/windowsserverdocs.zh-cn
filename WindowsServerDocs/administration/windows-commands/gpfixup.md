---
title: gpfixup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbe9873cc15866037c4688aaac89095e4a85dec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889418"
---
# <a name="gpfixup"></a>gpfixup



域重命名操作之后，域名称依赖项修复中的组策略对象和组策略链接。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/v|显示详细的状态消息。</br>如果不使用此参数，仅错误消息或的摘要状态消息**成功**或**失败**出现。|
|/olddns:\<OLDDNSNAME >|指定已重命名的域的旧的 DNS 名称 *\<OLDDNSNAME >* 域重命名操作时更改某个域的 DNS 名称。 您可以使用此参数仅在也使用 **/newdns**参数来指定新的域 DNS 名称。|
|/newdns:\<NEWDNSNAME >|指定作为重命名的域的新 DNS 名称 *\<NEWDNSNAME >* 域重命名操作时更改某个域的 DNS 名称。 您可以使用此参数仅在也使用 **/olddns**参数来指定旧域 DNS 名称。|
|/oldnb:\<OLDFLATNAME >|指定作为重命名的域的旧 NetBIOS 名称 *\<OLDFLATNAME >* 域重命名操作时更改某个域的 NetBIOS 名称。 可以使用此参数，仅当你使用 **/newnb**参数来指定新的域 NetBIOS 名称。|
|/newnb:\<NEWFLATNAME >|指定已重命名的域的新的 NetBIOS 名称 *\<NEWFLATNAME >* 域重命名操作时更改某个域的 NetBIOS 名称。 可以使用此参数，仅当你使用 **/oldnb**参数来指定旧域 NetBIOS 名称。|
|/dc:\<DCNAME>|连接到名为的域控制器 *\<DCNAME >* （DNS 名称或 NetBIOS 名称）。 *\<DCNAME >* 必须承载域目录分区的可写副本，如以下所示：</br>-DNS 名称 *\<NEWDNSNAME >* 使用 **/newdns**</br>NetBIOS 名称 *\<NEWFLATNAME >* 使用 **/newnb**</br>如果不使用此参数，连接到所指示的重命名的域中的任何域控制器 *\<NEWDNSNAME >* 或 *\<NEWFLATNAME >*。|
|/sionly|执行到受管理的软件安装 （组策略软件安装扩展） 相关联的组策略修复。 跳过在 Gpo 中解决组策略链接和 SYSVOL 路径的操作。|
|/user:\<用户名 >|用户的安全上下文中运行此命令*\<用户名 >*，其中*\<用户名 >* 处于域 \ 用户格式。</br>如果不使用此参数，以登录用户身份运行此命令。|
|/pwd: {\<密码 >|*}|指定密码，以便通过使用指示的其他安全上下文 **/user**。 如果 **&#42;** 指定而不是密码，系统会提示输入密码。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Gpfixup**命令现已推出 Windows Server 2008 R2 和 Windows Server 2008 中，在服务器核心安装除外。
-   尽管组策略管理控制台 (GPMC) 随 Windows Server 2008 R2 和 Windows Server 2008 一起分发，你必须作为一项功能通过服务器管理器安装组策略管理。

## <a name="BKMK_Examples"></a>示例

此示例假定您已经执行域重命名操作，在其中更改中的 DNS 名称**MyOldDnsName**到**MyNewDnsName**，和中的 NetBIOS 名称**MyOldNetBIOSName**到**MyNewNetBIOSName**。 在此示例中，使用**gpfixup**命令连接到名为的域控制器**MyDcDnsName**和修复 Gpo 和组策略中的 Gpo 和链接嵌入链接通过更新旧的域名。 状态和错误输出保存到名为的文件**gpfixup.log**。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
此示例与前一个相同，不同之处在于它假定域期间未更改的域的 NetBIOS 名称重命名操作。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>其他参考

-   [管理 Active Directory 域重命名](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法解答](command-line-syntax-key.md)
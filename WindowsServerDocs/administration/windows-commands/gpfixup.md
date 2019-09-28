---
title: gpfixup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e32427369f1664476c81a81353ae8869ec0c2ff3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375667"
---
# <a name="gpfixup"></a>gpfixup



解决域重命名操作后组策略对象和组策略链接中的域名依赖关系。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Parameters

|       参数       |                                                                                                                                                                                                                               描述                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      显示详细的状态消息。</br>如果未使用此参数，则只会显示错误消息或摘要状态消息 "**成功**" 或 "**失败**"。                                                                                                                                                       |
| /olddns： \<OLDDNSNAME > |                                                                                                           当域重命名操作更改域的 DNS 名称时，将重命名域的旧 DNS 名称指定为 *\<OLDDNSNAME >* 。 仅当你还使用 **/newdns**参数来指定新的域 DNS 名称时，才能使用此参数。                                                                                                            |
| /newdns： \<NEWDNSNAME > |                                                                                                          当域重命名操作更改域的 DNS 名称时，将重命名的域的新 DNS 名称指定为 *\<NEWDNSNAME >* 。 仅当你还使用 **/olddns**参数来指定旧的域 DNS 名称时，才能使用此参数。                                                                                                           |
| /oldnb： \<OLDFLATNAME > |                                                                                                        当域重命名操作更改域的 NetBIOS 名称时，将重命名域的旧 NetBIOS 名称指定为 *\<OLDFLATNAME >* 。 仅当使用 **/newnb**参数指定新的域 NetBIOS 名称时，才能使用此参数。                                                                                                        |
| /newnb： \<NEWFLATNAME > |                                                                                                       当域重命名操作更改域的 NetBIOS 名称时，将重命名的域的新 NetBIOS 名称指定为 *\<NEWFLATNAME >* 。 仅当使用 **/oldnb**参数指定旧的域 NetBIOS 名称时，才能使用此参数。                                                                                                       |
|     /dc： \<DCNAME >     | 连接到名为 *\<DCNAME >* 的域控制器（DNS 名称或 NetBIOS 名称）。 *\<DCNAME >* 必须承载域目录分区的可写副本，如下所示：</br>-使用 **/newdns** *\<NEWDNSNAME >* DNS 名称</br>-使用 **/newnb** *\<NEWFLATNAME >* 的 NetBIOS 名称</br>如果未使用此参数，则连接到 *\<NEWDNSNAME >* 或 *\<NEWFLATNAME >* 所指示的重命名域中的任何域控制器。 |
|        /sionly        |                                                                                                                           仅执行与托管软件安装相关的组策略修补程序（组策略的软件安装扩展）。 跳过修复组策略链接和 Gpo 中 SYSVOL 路径的操作。                                                                                                                           |
|   /user： \<USERNAME >   |                                                                                                                                   在用户 *\<USERNAME >* 的安全上下文中运行此命令，其中 *\<USERNAME >* 的格式为 domain\user</br>如果未使用此参数，则以登录用户的身份运行此命令。                                                                                                                                    |
|   /pwd： {\<PASSWORD >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  在命令提示符下显示帮助。                                                                                                                                                                                                                   |

## <a name="remarks"></a>备注

-   除了服务器核心安装， **gpfixup**命令在 windows Server 2008 R2 和 windows server 2008 中可用。
-   尽管组策略管理控制台（GPMC）随 Windows Server 2008 R2 和 Windows Server 2008 一起分发，但必须通过服务器管理器将组策略管理作为一项功能安装。

## <a name="BKMK_Examples"></a>示例

此示例假设你已执行域重命名操作，其中你将 DNS 名称从**MyOldDnsName**更改为**MyNewDnsName**，并将 NetBIOS 名称从**MyOldNetBIOSName**更改为**MyNewNetBIOSName**。 在此示例中，使用**gpfixup**命令连接到名为**MyDcDnsName**的域控制器，并通过更新在 gpo 和链接中嵌入的旧域名来修复 gpo 和组策略链接。 状态和错误输出保存到名为**gpfixup**的文件中。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
此示例与上一个示例相同，不同之处在于它假定域的 NetBIOS 名称在域重命名操作过程中未更改。
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>其他参考

-   [管理 Active Directory 域重命名](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令行语法项](command-line-syntax-key.md)
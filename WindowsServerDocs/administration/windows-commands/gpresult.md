---
title: gpresult
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb61911450ea8c0c68af0cf1a35c2f571810504b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375665"
---
# <a name="gpresult"></a>gpresult

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示远程用户和计算机的策略的结果集（RSoP）信息。
若要通过防火墙将 RSoP 报表用于远程目标计算机，您必须具有在端口上启用入站网络流量的防火墙规则。

## <a name="syntax"></a>语法

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

## <a name="parameters"></a>Parameters

> [!NOTE]
> 除了使用 **/？** 时，您必须包含一个输出选项，即 **/r**、 **/v**、 **/z**、 **/x**或 **/h**。

|                参数                 |                                                                                                     描述                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /s \<system @ no__t-1               |                                                  指定远程计算机的名称或 IP 地址。 不要使用反斜杠。 默认值为本地计算机。                                                   |
|             /u \<USERNAME @ no__t-1              |                                使用指定用户的凭据运行该命令。 默认用户是登录到发出命令的计算机的用户。                                 |
|            /p [\<PASSWOrd @ no__t]             |            指定在 **/u**参数中提供的用户帐户的密码。 如果省略 **/p** ，则**gpresult**会提示输入密码。 **/p**不能与 **/x**或 **/h**一起使用。            |
| /user [\<TARGETDOMAIN\>\\]\<TARGETUSER\> |                                                                            指定要显示其 RSoP 数据的远程用户。                                                                             |
|      /scope {用户&#124;计算机}       |                                显示用户或计算机的 RSoP 数据。 如果省略 **/scope** ，则**gpresult**显示用户和计算机的 RSoP 数据。                                 |
|        [/x &#124; /h] <FILENAME>         | 以 XML （ **/x**）或 HTML （ **/h**）格式将报表保存在位置，并使用*FILENAME*参数指定的文件名保存。 不能与 **/u**、 **/p**、 **/r**、 **/v**或 **/z**一起使用。 |
|                    /f                    |                                                           强制**gpresult**覆盖 **/x**或 **/h**选项中指定的文件名。                                                           |
|                    /r                    |                                                                                             显示 RSoP 摘要数据。                                                                                              |
|                    /v                    |                                                    显示详细的策略信息。 这包括应用优先级为1的详细设置。                                                    |
|                    /z                    |                                     显示有关组策略的所有可用信息。 这包括应用优先级为1和更高的详细设置。                                      |
|                    /?                    |                                                                                         在命令提示符下显示帮助。                                                                                         |

## <a name="remarks"></a>备注
- 组策略是为组织中的用户和计算机定义和控制程序、网络资源和操作系统运行方式的主要管理工具。 在 active directory 环境中，组策略基于其在站点、域或组织单位中的成员身份应用于用户或计算机。
- 由于可以将重叠的策略设置应用于任何计算机或用户，因此在用户登录时，组策略功能将生成一组生成的策略设置。 **gpresult**显示用户登录时为指定用户在计算机上强制执行的策略设置的结果集。
- 由于 **/v**和 **/z**产生了大量信息，因此将输出重定向到文本文件（例如， **gpresult/z > policy .txt**）很有用。
- **Gpresult**命令在 windows server 2012、windows Server 2008 R2、windows Server 2008、windows 8、windows 7 和 windows Vista 中可用。
  ## <a name="examples"></a>示例
  以下示例检索计算机**srvmain**的远程用户**targetusername**的 rsop 数据，并仅显示有关用户的 rsop 数据。 命令将以用户**maindom\hiropln**的凭据运行，并输入<strong>p@ssW23</strong>作为该用户的密码。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
下面的示例将计算机**srvmain**的远程用户**targetusername**的所有可用组策略信息保存到名为的**文件中。** 不会包含有关计算机的任何数据。 命令将以用户**maindom\hiropln**的凭据运行，并输入<strong>p@ssW23</strong>作为该用户的密码。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
以下示例显示了计算机**srvmain**和登录用户的 RSoP 数据。 同时包含用户和计算机的相关数据。 命令将以用户**maindom\hiropln**的凭据运行，并输入<strong>p@ssW23</strong>作为该用户的密码。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>其他参考
- [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)

- [命令行语法项](command-line-syntax-key.md)

---
title: gpresult
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e0acd4563225bc1c413b2096387ad8c14f6009a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835968"
---
# <a name="gpresult"></a>gpresult

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示远程用户和计算机的策略的结果集 (RSoP) 信息。
若要使用 RSoP 报告远程目标计算机通过防火墙，必须启用入站的网络流量的端口上的防火墙规则。
## <a name="syntax"></a>语法
```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```
## <a name="parameters"></a>Parameters
> [!NOTE]
> 当你使用除 **/？**，你必须既包括输出选项， **/r**， **/v**， **/z**， **/x**，或 **/h**。

|参数|描述|
|-------|--------|
|/s \<system\>|指定的名称或远程计算机的 IP 地址。 不要使用反斜杠。 默认值为本地计算机。|
|/u \<USERNAME\>|使用指定的用户的凭据来运行该命令。 默认用户是登录到发出该命令的计算机的用户。|
|/p [\<PASSWOrd\>]|指定中提供的用户帐户的密码 **/u**参数。 如果 **/p**省略，则**gpresult**提示输入密码。 **/ p**不能用于 **/x**或 **/h**。|
|/user [\<TARGETDOMAIN\>\\]\<TARGETUSER\>|指定远程用户的 RSoP 数据将显示。|
|/ scope {用户&#124;计算机}|显示用户或计算机的 RSoP 数据。 如果 **/范围**省略，则**gpresult**显示用户和计算机的 RSoP 数据。|
|[/x &#124; /h] <FILENAME>|将报表保存在 XML 中 (**/x**) 或 HTML (**/h**) 的格式中的位置并具有指定的文件名称*文件名*参数。 不能用于 **/u**， **/p**， **/r**， **/v**，或 **/z**。|
|/f|强制**gpresult**覆盖中指定的文件名 **/x**或 **/h**选项。|
|/r|显示 RSoP 摘要数据。|
|/v|显示详细的策略信息。 这包括与 1 的设置已应用的详细的设置。|
|/z|显示有关组策略的所有可用信息。 这包括 1 和更高的优先级已应用的详细的设置。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
-   组策略是用于定义和控制组织中的用户和计算机如何运行程序、 网络资源和操作系统的主要管理工具。 在 active directory 环境中，组策略应用到用户或计算机基于站点、 域或组织单位中的成员身份。
-   因为您可以对任何计算机或用户应用策略设置重叠，组策略功能将生成生成一组策略设置，当用户登录时。 **gpresult**用户登录时指定的用户的计算机上显示生成的执行策略设置集。
-   因为 **/v**并 **/z**生成大量的信息，最好先将输出重定向到文本文件 (例如， **gpresult/z > policy.txt 时**)。
-   **Gpresult**命令现已推出 Windows Server 2012、 Windows Server 2008 R2、 Windows server 2008、 Windows 8、 Windows 7 和 Windows Vista。
## <a name="BKMK_Examples"></a>示例
下面的示例检索远程用户的 RSoP 数据**targetusername**的计算机**srvmain**，并显示有关仅对用户的 RSoP 数据。 使用用户的凭据运行命令**maindom\hiropln**，并**p@ssW23**作为密码输入为该用户。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```
以下示例将保存有关组策略为远程用户的所有可用的信息**targetusername**的计算机**srvmain**的文件的名为**policy.txt 时**. 不包含有关计算机的数据。 使用用户的凭据运行命令**maindom\hiropln**，并**p@ssW23**作为密码输入为该用户。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```
下面的示例显示在计算机的 RSoP 数据**srvmain**和登录的用户。 包含有关用户和计算机数据。 使用用户的凭据运行命令**maindom\hiropln**，并**p@ssW23**作为密码输入为该用户。
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```
## <a name="additional-references"></a>其他参考
-   [组策略技术中心](https://go.microsoft.com/fwlink/?LinkID=145531)

-   [命令行语法解答](command-line-syntax-key.md)

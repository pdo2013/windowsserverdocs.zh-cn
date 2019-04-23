---
title: Scwcmd 分析
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0259271b-be5b-48d7-a51d-8b9b6786efb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce3428b281ede582ed781afbdee9dea495b52ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873908"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> 适用于：Windows Server 2012 R2, Windows Server 2012

确定计算机是否符合策略。 在.xml 文件中返回结果。 也接受一系列计算机名称作为输入。 若要在浏览器中查看结果，请使用**scwcmd 视图**并指定 **%windir%\security\msscw\TransformFiles\scwanalysis.xsl**为.xsl 转换。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/m:\<计算机名 >|指定 NetBIOS 名称、 DNS 名称或要分析的计算机的 IP 地址。 如果 **/m**指定参数，则 **/p**还必须指定参数。|
|/ou:\<OuName >|指定 Active Directory 域服务中的组织单位 (OU) 的完全限定的域名 (FQDN)。 如果 **/ou**指定参数，则 **/p**还必须指定参数。 根据给定的策略，将对 OU 中的所有计算机进行都分析。|
|/p:\<策略 >|指定要用于执行分析的.xml 策略文件的路径和文件。|
|/i:\<ComputerList >|指定包含一系列计算机及其需要的策略文件的.xml 文件的路径和文件。 将对其相应的策略文件进行的.xml 文件中的所有计算机。 示例.xml 文件是 %windir%\security\samplemachinelist.xml。|
|/o:\<ResultDir >|指定的路径和保存分析结果文件的目录。 默认值为当前目录。|
|带 /u:\<用户名 >|指定一个备用用户凭据在远程计算机上执行分析时使用。 默认值是已登录用户。|
|/pw:\<密码 >|指定一个备用用户凭据在远程计算机上执行分析时使用。 默认值是已登录用户的密码。|
|/t:\<线程 >|指定应在分析过程中维护的同时未完成的分析操作的数目 (默认值 = 40，MinValue = 1，MaxValue = 1000年)。|
|/l|会导致分析过程要记入日志。 将为所分析的每台计算机生成一个日志文件。 日志文件将存储在与结果文件相同的目录。 使用 **/o**选项指定结果文件的目录。|
|/e|一个事件记录到应用程序事件日志中，如果找到不匹配。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要分析针对文件 webpolicy.xml 安全策略，请键入：
```
scwcmd analyze /p:webpolicy.xml

```
若要分析使用 webadmin 帐户的凭据来命名文件 webpolicy.xml 针对 web 服务器的计算机上的安全策略，请键入：
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
若要分析针对文件 webpolicy.xml，最多 100 个线程的安全策略并将结果输出到名为 resultserver 共享中的结果的文件，请键入：
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
若要使用 DomainAdmin 凭据分析信息安全策略以获得对文件 webpolicy.xml WebServers OU，请键入：
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
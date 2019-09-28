---
title: Scwcmd 分析
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 14426ae33144ae9bdd8f8154b4be74a3f088606b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371261"
---
# <a name="scwcmd-analyze"></a>Scwcmd: analyze

> 适用于：Windows Server 2012 R2、Windows Server 2012

确定计算机是否符合策略。 结果将返回到 .xml 文件中。 还接受计算机名称的列表作为输入。 若要在浏览器中查看结果，请使用**scwcmd 视图**并将 **%windir%\security\msscw\TransformFiles\scwanalysis.xsl**指定为 .xsl 转换。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
scwcmd analyze [[[/m:<ComputerName> | /ou:<Ou>] /p:<Policy>] | /i:<ComputerList>] [/o:
<ResultDir>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>] [/l] [/e]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/m： \<ComputerName >|指定要分析的计算机的 NetBIOS 名称、DNS 名称或 IP 地址。 如果指定了 **/m**参数，则还必须指定 **/p**参数。|
|/ou： \<OuName >|指定 Active Directory 域服务中组织单位（OU）的完全限定的域名（FQDN）。 如果指定 **/ou**参数，则还必须指定 **/p**参数。 OU 中的所有计算机都将针对给定策略进行分析。|
|/p： \<Policy >|指定要用于执行分析的 .xml 策略文件的路径和文件名。|
|/i： \<ComputerList >|指定 .xml 文件的路径和文件名，该文件包含计算机的列表及其预期的策略文件。 .Xml 文件中的所有计算机都将针对其相应的策略文件进行分析。 示例 .xml 文件为%windir%\security\SampleMachineList.xml。|
|/o： \<ResultDir >|指定应在其中保存分析结果文件的路径和目录。 默认值为当前目录。|
|/u： \<UserName >|指定在远程计算机上执行分析时要使用的备用用户凭据。 默认值为已登录的用户。|
|/pw： \<Password >|指定在远程计算机上执行分析时要使用的备用用户凭据。 默认值为登录用户的密码。|
|/t： \<Threads >|指定在分析过程中应保持的同时未完成的分析操作的数量（DefaultValue = 40，MinValue = 1，同类型 = 1000）。|
|/l|导致记录分析进程。 将为要分析的每台计算机生成一个日志文件。 日志文件将与结果文件存储在同一目录中。 使用 **/o**选项指定结果文件的目录。|
|/e|如果发现不匹配，请将事件记录到应用程序事件日志中。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd 仅适用于运行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的计算机。

## <a name="BKMK_Examples"></a>示例

若要针对文件 webpolicy 分析安全策略，请键入：
```
scwcmd analyze /p:webpolicy.xml

```
若要使用 webadmin 帐户的凭据针对文件 webpolicy 分析名为 web 服务器上的安全策略，请键入：
```
scwcmd analyze /m:webserver /p:webpolicy.xml /u:webadmin

```
若要针对文件 webpolicy 分析安全策略，最多可包含100个线程，并将结果输出到 resultserver 共享中名为 "结果" 的文件，请键入：
```
scwcmd analyze /i:webpolicy.xml /t:100 /o:\\resultserver\results

```
若要使用 DomainAdmin 凭据针对文件 webpolicy 分析 WebServers OU 的安全策略，请键入：
```
scwcmd analyze /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
---
title: Scwcmd 配置
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6528b9dc-3d82-4228-b734-ed717458d74c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31838ac7299cc30a7b7dde3beb47669df772487c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857498"
---
# <a name="scwcmd-configure"></a>Scwcmd: configure

> 适用于：Windows Server 2012 R2, Windows Server 2012

适用于一台计算机的安全配置向导 SCW 生成安全策略。 此命令行工具也接受一系列计算机名称作为输入。

## <a name="syntax"></a>语法

```
scwcmd configure [[[/m:<ComputerName> | /ou:<OuName>] /p:<Policy>] | /i:<ComputerList>] [/u:<UserName>] [/pw:<Password>] [/t:<Threads>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/m:\<计算机名 >|指定 NetBIOS 名称、 DNS 名称或要配置的计算机的 IP 地址。 如果 **/m**指定参数，则 **/p**还必须指定参数。|
|/ou:\<OuName >|指定 Active Directory 域服务中的组织单位 (OU) 的完全限定的域名 (FQDN)。 如果 **/ou**指定参数，则 **/p**还必须指定参数。 根据给定的策略，将对 OU 中的所有计算机进行都分析。|
|/p:\<策略 >|指定要用来执行配置的.xml 策略文件的路径和文件。|
|/i:\<ComputerList >|指定包含一系列计算机及其需要的策略文件的.xml 文件的路径和文件。 将根据其相应的策略文件配置的.xml 文件中的所有计算机。 示例.xml 文件是 %windir%\security\samplemachinelist.xml。|
|带 /u:\<用户名 >|指定配置的远程计算机时要使用的备用用户凭据。 默认值是已登录用户。|
|/pw:\<密码 >|指定配置的远程计算机时要使用的备用用户凭据。 默认值是已登录用户的密码。|
|/t:\<线程 >|指定应在配置过程中维护的同步未完成配置操作的数目 (默认值 = 40，MinValue = 1，MaxValue = 1000年)。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要配置针对文件 webpolicy.xml 安全策略，请键入：
```
scwcmd configure /p:webpolicy.xml
```
若要使用 webadmin 帐户凭据时对文件 webpolicy.xml 172.16.0.0 配置计算机的安全策略，请键入：
```
scwcmd configure /m:172.16.0.0 /p:webpolicy.xml /u:webadmin
```
若要在上一个最多包含 100 个线程列表 campusmachines.xml 的所有计算机上配置安全策略，请键入：
```
scwcmd configure /i:campusmachines.xml /t:100
```
若要通过使用 DomainAdmin 帐户的凭据针对文件 webpolicy.xml WebServers OU 中的所有计算机上配置安全策略，请键入：
```
scwcmd configure /ou:OU=WebServers,DC=Marketing,DC=ABCCompany,DC=com /p:webpolicy.xml /u:DomainAdmin
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
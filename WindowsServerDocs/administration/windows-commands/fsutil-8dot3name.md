---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873188"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查询或更改的设置短名称 （8.3 文件名名称） 的行为，其中包括：

-   查询的短名称行为的当前设置

-   扫描可能会受到影响，如果已从指定的目录路径中去除的短名称的注册表项的指定的目录路径

-   更改控制的短名称行为的设置。 此设置可以应用到指定的卷或默认卷设置。

-   删除目录中的所有文件的短名称

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|query [<VolumePath>]|8.3 文件名短名称创建行为的状态在文件系统中查询。<br /><br />如果*VolumePath*不指定作为参数，则会显示所有卷的默认 8dot3name 创建行为设置。|
|扫描 <DirectoryPath>|扫描所在的文件中指定*DirectoryPath*可能会受到影响，如果已从文件名称中去除 8.3 短名称的注册表项。|
|set { <DefaultValue> &#124; <VolumePath>}|更改在以下情况下 8dot3 名称创建的文件系统行为：<br /><br /><ul><li>当*DefaultValue*指定，则注册表项， **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**，设置为*DefaultValue*。<br /><br />    *DefaultValue*可以具有以下值：<br /><br /><ul><li>**0**:启用 8dot3 名称创建的系统上的所有卷。</li><li>**1**:禁用 8.3 文件名为在系统上的所有卷的名称创建。</li><li>**2**:针对每个卷设置 8dot3 名称创建。</li><li>**3**:禁用 8.3 文件名为除系统卷的所有卷的名称创建。</li></ul></li><li>当*VolumePath*磁盘标志 8dot3name 属性上的指定的卷设置为启用 8dot3 指定卷的名称创建的指定 (**0**) 或一组要在禁用 8.3 文件名名称创建指定卷 (**1**)。<br /><br />    必须设置为值的 8dot3 名称创建的默认文件系统行为**2**才能启用或禁用 8.3 文件名指定卷的名称创建。</li></ul>|
|条带 <DirectoryPath>|删除 8dot3 文件名称所在的所有文件中指定*DirectoryPath*。 8.3 文件名文件名称不删除任何文件位置*DirectoryPath*组合文件名称包含超过 260 个字符。<br /><br />此命令列出了，但不会修改指向有 8.3 文件名称中永久删除的文件的注册表项。<br /><br />从文件中永久删除 8.3 文件名称的效果的详细信息，请参阅[备注](Fsutil-8dot3name.md#BKMK_remarks)。|
|<VolumePath>|指定后接一个冒号或格式的 GUID 的驱动器名称**卷 {***GUID***}**。|
|/f|指定的所有文件的所在中指定*DirectoryPath*已 8.3 文件名称删除，即使有指向使用 8dot3 文件名称的文件的注册表项。 在这种情况下，该操作删除 8.3 文件名称，但不修改任何指向使用 8.3 文件名称的文件的注册表项。 **警告：** 建议您备份你的目录或卷之前使用 **/f**参数因为它可能会导致意外的应用程序故障，包括无法卸载程序。|
|/l [<log file>]|指定写入信息的日志文件。<br /><br />如果 **/l**未指定参数，所有信息都写入到默认的日志文件：<br /><br />**%temp%\8dot3_removal_log@(***GMT YYYY-MM-DD HH-MM-SS***).log**|
|/s|指定该操作应应用到指定的子目录*DirectoryPath*。|
|/t|指定删除 8dot3 文件的名称，应在测试模式下运行。 除实际删除 8.3 文件名称的所有操作都执行。 测试模式可用于发现的注册表项指向使用 8.3 文件名称的文件。|
|/v|指定写入到日志文件的所有信息也会都显示在命令行。|

## <a name="BKMK_remarks"></a>备注

-   永久删除 8dot3 文件的名称并不修改指向 8.3 文件名称的注册表项可能会导致意外的应用程序故障，包括无法卸载应用程序。 建议您首先备份目录或卷然后再尝试删除 8dot3 文件的名称。

## <a name="BKMK_examples"></a>示例
若要使用的 GUID，{928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁盘卷禁用 8.3 文件名名称行为查询键入：

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

此外可以通过使用查询 8dot3 名称行为**行为**子命令。

若要删除 8.3 文件名中的文件名称*D:\MyData*目录及所有子目录，同时将信息写入日志文件指定为*mylogfile.log*，类型：

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[fsutil 行为](Fsutil-behavior.md)



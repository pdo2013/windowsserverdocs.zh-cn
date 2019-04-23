---
title: msinfo32
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843458"
---
# <a name="msinfo32"></a>msinfo32

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

打开系统信息工具以在本地计算机上显示的硬件、 系统组件和软件环境的全面视图。 
## <a name="syntax"></a>语法
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<path>|指定的文件格式打开*C:\Folder1\File1.XXX*，其中*C*是驱动器号， *Folder1*是文件夹， *File1*是文件名称，并*XXX*是文件扩展名。<br /><br />此文件可以是 **.nfo**， **.xml**， **.txt**，或者 **.cab**文件。|
|<computerName>|指定的目标或本地计算机的名称。 这可以是 UNC 名称、 IP 地址或完整计算机名称。|
|<CategoryID>|指定的类别项的 ID。 你可以通过使用获取的类别 ID **/showcategories**。|
|/pch|显示系统历史记录视图中的系统信息工具。|
|/nfo|保存导出的文件作为 **.nfo**文件。 中指定的文件的名称，如果*路径*不是以结尾 **.nfo**扩展 **.nfo**扩展自动追加到的文件的名称。|
|/report|将保存在文件*路径*为文本文件。 保存的文件名称中显示的样子*路径*。 除非指定路径中时，.txt 扩展名不被追加到文件。|
|/computer|启动指定的远程计算机的系统信息工具。 必须具有适当的权限即可访问远程计算机。|
|/showcategories|系统信息工具启动了所有可用类别 Id 显示，而不是显示友好或本地化名称。 例如，软件环境类别显示为**SWEnv**类别。|
|/category|系统信息开始指定所选的类别。 使用 **/showcategories**以显示可用的类别 Id 的列表。|
|/categories|启动指定的类别或类别显示系统信息。 它还将输出限制为所选的类别或类别。 使用 **/showcategories**以显示可用的类别 Id 的列表。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
某些系统信息类别包含大量的数据。 可以使用**start /wait**命令可优化这些类别的报告性能。 有关详细信息，请参阅[系统信息](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx)。
## <a name="BKMK_Examples"></a>示例
若要列出可用的类别 Id，请键入：
```
msinfo32 /showcategories
```
若要开始的所有可用信息的系统信息工具显示，除已加载模块，类型：
```
msinfo32 /categories +all -loadedmodules
```
若要显示仅系统摘要信息，并创建一个名包含系统摘要类别中的信息的 syssum.nfo.nfo 文件，请键入：
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
若要显示资源的冲突信息并创建名为包含有关资源冲突的信息的 conflicts.nfo.nfo 文件，请键入：
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)


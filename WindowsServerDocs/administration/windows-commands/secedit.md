---
title: secedit
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c70211cc029cec7e6bb0290877089ecb9a86f22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441458"
---
# <a name="secedit"></a>secedit



配置和分析系统安全，通过比较当前配置与指定的安全模板。

## <a name="syntax"></a>语法

```
secedit 
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|可以分析针对存储在数据库的基准设置的当前系统设置。  分析结果存储在数据库的一个独立区域中，可以在安全配置和分析管理单元中查看。|
|[Secedit:configure](secedit-configure.md)|可以使用存储在数据库中的安全设置来配置系统。|
|[Secedit:export](secedit-export.md)|可以导出存储在数据库中的安全设置。|
|[Secedit:generaterollback](secedit-generaterollback.md)|可以生成关于配置模板的回滚模板。|
|[Secedit:import](secedit-import.md)|可以导入安全模板数据库，以便在模板中指定的设置可以应用到系统或对系统进行分析。|
|[Secedit:validate](secedit-validate.md)|可验证的安全模板的语法。|

## <a name="remarks"></a>备注

对于所有文件名使用当前目录，如果未指定路径。

在使用安全模板管理单元中和安全配置，会创建一个安全模板并分析管理单元中运行，会创建以下文件：


|           文件           |                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv.log        |                                                                                                                             **位置**: %windir%\security\logs</br>**通过创建**： 操作系统</br>**文件类型**： 文本</br>**刷新频率**:覆盖时 secedit / 分析 / 配置/导出或运行 /import。</br>**内容**:包含按策略类型分组的分析结果。                                                                                                                             |
| *用户选择了名称*.sdb |                                                                                    **位置**: %windir%\*用户帐户<em>\Documents\Security\Database</br></em>*创建由*<em>： 安全配置和分析管理单元中运行</br></em>*文件类型*<em>： 专有</br></em>*刷新速率*<em>:一旦创建了一个新的安全模板更新。</br></em>*内容*\*:本地安全策略和用户创建的安全模板。                                                                                    |
| *用户选择了名称*.log | **位置**：用户定义，但是默认为 %windir%\*用户帐户<em>\Documents\Security\Logs</br></em>*创建由*<em>:运行 / 分析和 / 配置子命令 （或使用安全配置和分析管理单元）</br></em>*文件类型*<em>： 文本</br></em>*刷新速率*<em>:运行 / 分析和 / 配置子命令 （或使用安全配置和分析管理单元）;被覆盖。</br></em>*内容*\*:</br>1.日志文件名称</br>2.日期和时间</br>3.分析或调查的结果。 |
| *用户选择了名称*.inf |                                                                                     **位置**: %windir%\*用户帐户<em>\Documents\Security\Templates</br></em>*创建由*<em>： 安全模板管理单元中运行</br></em>*文件类型*<em>： 文本</br></em>*刷新速率*<em>： 每次更新的安全模板</br></em>*内容*\*:包含有关使用管理单元中选择每个策略模板的信息设置。                                                                                     |

> [!NOTE]
> Microsoft 管理控制台 (MMC) 和安全配置和分析管理单元中不可用在 Server Core 上。

#### <a name="additional-references"></a>其他参考

有关如何使用此命令的示例，请参阅示例部分中的任何子命令文件。
-   [命令行语法项](command-line-syntax-key.md)
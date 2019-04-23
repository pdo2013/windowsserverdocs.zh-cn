---
title: bcdedit
description: Windows 命令主题**bcdedit** -创建新的存储、 修改现有的存储，并添加引导菜单参数。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872438"
---
# <a name="bcdedit"></a>bcdedit



启动配置数据 (BCD) 文件提供用于描述启动应用程序和启动应用程序设置的存储。 对象和存储区中的元素有效地替换 Boot.ini。

BCDEdit 是用于管理 BCD 存储的命令行工具。 它可以用于多种用途，包括创建新的存储，修改现有的存储，添加启动菜单参数，依此类推。 BCDEdit 是实质上是相同的目的 Bootcfg.exe 在早期版本的 Windows，但有两个主要改进：
-   公开比 Bootcfg.exe 更广泛的引导参数。
-   已改进了脚本的支持。

> [!NOTE]
> 若要使用 BCDEdit 修改 BCD，所需管理权限。

BCDEdit 是编辑的启动配置的 Windows Vista 和更高版本的 Windows 的主要工具。 将包括与 Windows Vista 分发 %WINDIR%\System32 文件夹中。

BCDEdit 仅限于标准数据类型和主要用于执行到 BCD 单个常见的更改。 对于更复杂的操作或使用了非标准数据类型，请考虑使用 BCD Windows Management Instrumentation (WMI) 应用程序编程接口 (API) 来创建更功能强大且灵活的自定义工具。

## <a name="syntax"></a>语法

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parameters

### <a name="general-bcdedit-command-line-option"></a>常规 BCDEdit 命令行选项

|Option|描述|
|------|-----------|
|/?|显示 BCDEdit 命令的列表。 运行不带参数的此命令显示可用命令的摘要。 若要显示有关特定命令的详细的帮助，请运行**bcdedit /？** \<命令 >，其中<command>是要搜索有关的详细信息的命令的名称。 例如， **bcdedit /？ createstore**显示 Createstore 命令的详细的帮助。|

### <a name="parameters-that-operate-on-a-store"></a>对存储执行操作的参数

|Option|描述|
|------|-----------|
|/createstore|创建新的空引导配置数据存储。 创建的存储区不是系统存储区。|
|/export|导出到文件存储系统的内容。 可以稍后使用此文件，以还原系统存储的状态。 此命令将仅对系统存储有效。|
|/import|通过使用先前生成的备份数据文件还原的系统存储状态 **/export**选项。 导入之前，此命令将删除系统存储区中的所有现有条目。 此命令将仅对系统存储有效。|
|/store|此选项可以使用大多数 BCDedit 命令，用于指定要使用的存储区。 如果未指定此选项，BCDEdit 运行系统存储区。 运行**bcdedit /store**本身的命令是等效于运行**bcdedit /enum active**命令。|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>对存储中的条目的参数

|参数|描述|
|---------|-----------|
|/copy|使指定的启动项的副本中相同的系统存储。|
|/create|在引导配置数据存储中创建一个新条目。 如果指定的已知的标识符，则 **/应用程序**， **/ 继承**，并 **/device**不能指定参数。 如果标识符未指定或不非常有名 **/应用程序**， **/ 继承**，或 **/device**必须指定选项。|
|/delete|从指定项中删除一个元素。|

### <a name="parameters-that-operate-on-entry-options"></a>对输入选项的参数

|参数|描述|
|---------|-----------|
|/deletevalue|从启动项目中删除指定的元素。|
|/set|设置条目选项值。|

### <a name="parameters-that-control-output"></a>控制输出的参数

|参数|描述|
|---------|-----------|
|/enum|列出了存储区中的项。 **/Enum**选项是 BCEdit，因此，运行的默认值**bcdedit**不带参数的命令是等效于运行**bcdedit /enum active**命令。|
|/v|详细模式。 通常情况下，任何已知的入口标识符由其友好速记形式表示。 指定 **/v**如命令行选项中完整显示所有标识符。 运行**bcdedit /v**本身的命令是等效于运行**bcdedit /enum active /v**命令。|

### <a name="parameters-that-control-the-boot-manager"></a>控制启动管理器的参数

|参数|描述|
|---------|-----------|
|/bootsequence|指定要用于下一次启动一次性显示顺序。 此命令是类似于 **/displayorder**选项，但它使用仅在下次计算机启动。 此后，计算机将恢复为原始的显示顺序。|
|/default|指定当在超时到期时，启动管理器选择的默认条目。|
|/displayorder|指定向用户显示引导参数时，启动管理器使用的显示顺序。|
|/timeout|指定等待，以秒为单位，启动管理器中选择的默认条目之前的时间。|
|/toolsdisplayorder|指定启动管理器显示时要使用的显示顺序**工具**菜单。|

### <a name="parameters-that-control-emergency-management-services"></a>控制紧急管理服务的参数

|参数|描述|
|---------|-----------|
|/bootems|启用或禁用紧急管理服务 (EMS) 为指定的项。|
|/ems|启用或禁用 EMS 为指定的操作系统启动项目。|
|/emssettings|设置计算机的全局 EMS 设置。 **/emssettings**不启用或禁用的任何特定启动项的 EMS。|

### <a name="parameters-that-control-debugging"></a>控制调试参数

|参数|描述|
|---------|-----------|
|/bootdebug|启用或禁用指定的启动项启动调试器。 此命令适用于任何启动条目，尽管是仅针对启动应用程序有效。|
|/dbgsettings|指定或显示系统的全局调试器设置。 此命令不会启用或禁用内核调试程序;使用 **/debug**选项完成此目的。 若要设置单个全局调试程序设置，请使用**bcdedit /set** \<dbgsettings > <type> <value> 命令。|
|/debug|启用或禁用内核调试程序指定的启动项。|

## <a name="examples"></a>示例

有关 BCDEdit 的示例，请参阅[BCDEdit 选项参考](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference)。
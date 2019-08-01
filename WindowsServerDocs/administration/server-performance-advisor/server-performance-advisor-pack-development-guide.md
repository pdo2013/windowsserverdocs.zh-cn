---
title: Server Performance Advisor 包开发指南
description: Server Performance Advisor 包开发指南
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c27dd0602c5993fd84e6956c2f50f6e2bfec8691
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "66435470"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Server Performance Advisor 包开发指南

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Microsoft Server Performance Advisor (SPA) 的这一开发指南提供了一些指导原则, 帮助开发人员和系统管理员开发顾问包以分析服务器性能。

它假定你熟悉性能日志和警报 (PLA)、性能计数器、注册表设置、Windows Management Instrumentation (WMI)、Windows 事件跟踪 (ETW) 和 Transact-sql (T-sql)。

有关使用 SPA 的详细信息, 请参阅[Server Performance Advisor 用户指南](server-performance-advisor-users-guide.md)。

## <a name="spa-advisor-pack-overview"></a>SPA 顾问包概述


Advisor 包通常用于特定的服务器角色, 并定义以下内容:

* 要通过 PLA 收集的数据, 包括 Windows Management Instrumentation (WMI)、性能计数器、注册表设置、文件和 Windows 事件跟踪 (ETW)

* 显示警报和建议的规则

* 要显示的数据 (收集的原始数据、聚合数据或前10个列表)

* 用于查看随时间变化的值的统计信息

* 可以趋势化的统计信息值

Advisor 包包含以下元素:

* **XML 元数据**(ProvisionMetadata)

    * [性能日志和警报 (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)数据收集器集

    * 报表布局

* **SQL 脚本**

    * 主存储过程

    * SQL 对象, 如存储过程和用户定义的函数

* **ETW 架构文件**(Schema) 这是可选的

### <a name="advisor-pack-workflow"></a>Advisor 包工作流

![advisor 包工作流](../media/server-performance-advisor/spa-dev-guide-workflow.png)

在此流程图中, 绿色圆圈表示 advisor 包。 所有其他圆圈表示在 SPA 框架的进程中运行的阶段。 SPA 使用 advisor 包收集数据、将数据导入到数据库中、初始化执行环境和运行 SQL 脚本。

### <a name="collect-data"></a>收集数据

当使用 SPA 将 advisor 包排队等候特定服务器时, 数据收集模块将从 advisor 包查询数据收集器集 XML, 并从目标服务器收集数据。 原始数据存储在用户指定的文件共享中。 在超过用户指定的 SPA 运行持续时间之前, 将不会停止数据收集。

### <a name="import-data-into-the-database"></a>将数据导入数据库

数据收集完成后, 每种类型的数据将导入到 SQL Server 数据库中的相应表中。 例如, 注册表设置将导入到名\#为 registryKeys 的表中。

导入 ETW 文件需要 ETW 架构文件以对 .etl 文件进行解码。 ETW 架构文件是一个 XML 文件。 它可以通过使用 Windows 附带的 tracerpt 生成。 只有在 advisor 包需要导入 ETW 数据时, 才需要 ETW 架构文件。

### <a name="switch-to-low-user-rights"></a>切换到低用户权限

SPA 框架自动调整权限以最大程度地减少所需的安全访问级别。 由于 advisor 包可以由任何人开发或修改, 因此 advisor 包可能包含篡改的 SQL 脚本。 为了缓解安全风险, 应使用低用户权限运行 advisor 包的任何 SQL 脚本。 它只能访问有限的数据库对象, 如临时表和作为存储过程公开的 SPA Api。 Advisor pack 中的 SQL 脚本可以调用这些存储过程以与 SPA 框架交互。

### <a name="initialize-execution-environment"></a>初始化执行环境

Advisor 包可以生成不同类型的输出, 例如通知、建议、事实数据表、统计信息和用于统计信息的图表。 SQL 脚本对收集的数据执行某些计算。 生成的结果通过 SPA 公共 Api 存储在临时表中。 在初始化阶段, 需要预配这些临时表和其他系统资源。

### <a name="run-sql-scripts"></a>运行 SQL 脚本

有一个主存储过程, 由 advisor pack 开发人员命名。 SPA 框架调用此存储过程以启动计算。 存储过程使用所收集的数据, 并将最终结果传达给 SPA 框架。

### <a name="switch-to-administrative-rights"></a>切换到管理权限

生成报表需要管理员权限。 报表生成由 SPA 完全控制。 不太可能被篡改。

### <a name="generate-a-report"></a>生成报告

在完成对 advisor 包的主存储过程之前, 将不会保留所有计算所得的结果, 如通知和统计信息。 在此阶段, SPA 框架会将临时表中的最终结果传输到特定格式的表中。 完成此阶段后, 可以使用 SPA 控制台查看报表。

## <a name="authoring-an-advisor-pack"></a>创作 advisor 包


### <a name="quick-guidelines"></a>快速指导原则

以下流程图介绍了用于开发完全功能顾问包的步骤。 本部分还提供了一些分步示例, 用于更好地说明每个步骤。

![advisor 包开发过程](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

顾问包通常按以下方式进行结构化:

Advisor 包

ProvisionMetadata

脚本

main .sql

func .sql

Schema. 男士

每个 advisor 包都必须具有一个名为 ProvisionMetadata 的文件。 它定义基本 advisor 包信息、要收集的数据、通知和规则, 以及报表需要存储和显示的方式。 SPA 框架使用此信息来生成一个临时表, 然后将临时表中的结果传输到用户可访问的表中。

所有报表 SQL 脚本都必须保存在一个名为 "**脚本**" 的子文件夹中。 出于维护目的, 我们建议您将不同的数据库对象保存在不同的 SQL Server 文件中。 至少必须有一个存储过程作为主入口点。

> [!NOTE]
> 除非 advisor pack 收集 ETW 跟踪, 否则不需要架构文件。 此架构文件用于描述 ETW 事件的架构和解码 ETW 事件。

### <a name="defining-basic-information"></a>定义基本信息

本部分介绍构成 advisor 包的一些基本元素, 包括 ProvisionMetadata 和属性。

下面是 ProvisionMetadata 文件的示例标头:

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Advisor 包版本

属性名称:**版本**

Advisor 包开发人员可以定义 advisor 包的主要版本和次要版本:

* 主要版本通常涉及重大改进。 旧版本生成的结果可能与新版本不兼容。 我们强烈建议在 advisor 包名称中包含主要版本。

* 当只有少量更改但没有数据不兼容问题时, SPA 允许次版本升级。

有关版本控制的详细信息, 请参阅[高级主题](#bkmk-advancedtopics)。

### <a name="script-entry-point"></a>脚本入口点

属性名称: **reportScript**

SPA 框架从脚本入口点查找主存储过程名称, 并以安全的方式运行。

### <a name="other-attributes"></a>其他属性

下面是一些可用于识别 advisor 包的其他属性:

* 显示名称: **displayName**

* 描述:**说明**

* 作者:**作者**

* Framework 版本: **frameworkversion** (默认为 3.0)

* 最低操作系统版本: **minOSversion** (此为将来的可扩展性保留)

* 丢失的事件通知: **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>定义数据收集器集

数据收集器集定义 SPA 框架应从目标服务器收集的性能数据。 它支持注册表设置、WMI、性能计数器、来自目标服务器的文件和 ETW。

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

上一示例中 **&lt;dataCollectorSet/&gt;** 的**duration**属性定义数据收集的持续时间 (以秒为单位)。 **duration**是必需的属性。 此设置控制性能计数器和 ETW 使用的收集持续时间。

### <a name="collect-registry-data"></a>收集注册表数据

你可以从以下注册表配置单元收集注册表数据:

* HKEY\_类\_根

* HKEY\_当前\_配置

* HKEY\_当前\_用户

* HKEY\_本地\_计算机

* HKEY\_用户

若要收集注册表设置, 请指定值名称的完整路径:HKEY\_本地\_计算机MyKey\\MyValue\\

若要收集注册表项下的所有设置, 请指定注册表项的完整路径:HKEY\_本地\_计算机MyKey\\\\

若要收集注册表项及其子项下的所有值 (PLA 以递归方式收集注册表数据), 请使用两个反斜杠作为最后一个路径分隔符:HKEY\_本地\_计算机MyKey\\\\\\

若要从远程计算机收集注册表信息, 请在注册表路径的开头包含计算机名称:HKEY\_本地\_计算机MyKey\\MyValue\\

例如, 你可能会看到如下所示的注册表项:

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

示例 1：仅返回活动的 PowerSchemes 及其值:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

示例 2：返回此路径下的所有键值对:

> [!NOTE]
> PLA 以用户凭据运行。 某些注册表项需要管理凭据。 枚举在未能访问任何子项时停止。

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

在运行 SQL 报表脚本之前, 所有收集的数据 **\#** 将导入到名为 registryKeys 的临时表中。 下表显示了示例2的结果:

KeyName | KeytypeId | ReplTest1
------ | ----- | -------
HKEY_LOCAL_MACHINE. ..\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Power Source 优化
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

**#RegistryKeys**表的架构如下所示:

列名 | SQL 数据类型 | 描述
-------- | -------- | --------
KeyName | Nvarchar (300) NOT NULL | 注册表项的完整路径名称
KeytypeId | Smallint NOT NULL | 内部类型 Id
值 | Nvarchar (4000) NOT NULL | 所有值

**KeytypeID**列可以具有以下类型之一:

id | type
--- | ---
1 | String
2 | expandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | 链接
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | Qword

### <a name="collect-wmi"></a>收集 WMI

可以添加任何 WMI 查询。 有关编写 WMI 查询的详细信息, 请参阅[WQL (适用于 WMI 的 SQL)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx)。下面的示例查询页面文件位置:

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

以上示例中的查询返回一条记录:

标题 | 名称 | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

由于 WMI 返回具有不同列的表, 因此当收集的数据导入到数据库中时, SPA 会执行数据规范化并添加到下表中:

**\#WMIObjects 表**

SequenceID | 命名空间 | ClassName | Relativepath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage =<br>C:\\页面文件 .sys | 1

**\#WmiObjectsProperties 表**

id | 查询
--- | ---
1 | Root\Cimv2: select * FROM Win32_PageFileUsage

**\#WmiQueries 表**

id | 查询
--- | ---
1 | Root\Cimv2: select * FROM Win32_PageFileUsage

**\#WmiObjects 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceId | Int NOT NULL | 关联行及其属性
命名空间 | Nvarchar (200) NOT NULL | WMI 命名空间
ClassName | Nvarchar (200) NOT NULL | WMI 类名称
Relativepath | Nvarchar (500) NOT NULL | WMI 相对路径
WmiqueryId | Int NOT NULL | 关联密钥 #WmiQueries

**\#WmiObjectProperties 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceId | Int NOT NULL | 关联行及其属性
名称 | Nvarchar (1000) NOT NULL | 属性名
ReplTest1 | Nvarchar (4000) NULL | 当前属性的值。

**\#WmiQueries 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
Id | Int NOT NULL | > 唯一查询 ID
查询 | Nvarchar (4000) NOT NULL | 设置元数据中的原始查询字符串

### <a name="collect-performance-counters"></a>收集性能计数器

下面是有关如何收集性能计数器的示例:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

**Interval**属性是所有性能计数器的必需全局设置。 它定义了收集性能数据的时间间隔 (以秒为单位)。

在上面的示例中, \\counter PhysicalDisk\*(\\) Avg。Disk sec/Transfer 将每秒查询一次。

可能有两个实例:总计和0**C:  **\_** D:** 和输出可能如下所示:

标志 | 类别 | CounterName | _Total 的实例值 | 实例值 0 C:D:
---- | ---- | ---- | ---- | ----
13:45: 52.630 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00100008362473995 |0.00100008362473995
13:45: 53.629 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00280023414927187 | 0.00280023414927187
13:45: 54.627 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00385999853230048 | 0.00385999853230048
13:45: 55.626 | PhysicalDisk | Avg.Disk sec/Transfer | 0.000933297607934224 | 0.000933297607934224

若要将数据导入到数据库中, 数据将规范化到名 **\#为 PerformanceCounters**的表中。

CategoryDisplayName | InstanceName | CounterdisplayName | 值
---- | ---- | ---- | ----
PhysicalDisk | 经 | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | 经 | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | 经 | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | 经 | Avg.Disk sec/Transfer | 0.000933297607934224
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.000933297607934224

**注意**本地化名称 (如**CategoryDisplayName**和**CounterdisplayName**) 根据目标服务器上使用的显示语言而有所不同。 如果要创建非特定语言的顾问包, 请避免使用这些字段。

PerformanceCounters 表架构 **\#**

列名 | SQL 数据类型 | 描述
---- | ---- | ---- | ----
标志 | datetime2 (3) NOT NULL | UNC 中收集的日期时间
类别 | Nvarchar (200) NOT NULL | 类别名称
CategoryDisplayName | Nvarchar (200) NOT NULL | 本地化类别名称
InstanceName | Nvarchar (200) NULL | 实例名
CounterName | Nvarchar (200) NOT NULL | 计数器名
CounterdisplayName | Nvarchar (200) NOT NULL | 本地化计数器名称
ReplTest1 | Float NOT NULL | 收集的值

### <a name="collect-files"></a>收集文件

路径可以是绝对路径或相对路径。 文件名可以包含通配符 (\*) 和问号 (？)。 例如, 若要收集临时文件夹中的所有文件, 可以指定 c:\\temp。\\\* 通配符适用于指定文件夹中的文件。

如果还希望从指定文件夹的子文件夹收集文件, 请使用两个反斜杠作为最后一个文件夹分隔符, 例如 c:\\temp。\\\\\*

下面是查询**applicationhost.config**文件的示例:

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

可在名 **\#为 Files**的表中找到结果, 例如:

querypath | Fullpath | Parentpath | FileName | 内容
----- | ----- | ----- | ----- | -----
% windir%\.。\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | 配置 | 0x3C3F78

**\#文件表架构**

列名 | SQL 数据类型 | 描述
---- | ---- | ----
querypath | Nvarchar (300) NOT NULL | 原始查询语句
Fullpath | Nvarchar (300) NOT NULL | 绝对文件路径和文件名
Parentpath | Nvarchar (300) NOT NULL | 文件路径
FileName | Nvarchar (300) NOT NULL | 文件名
内容 | Varbinary (MAX) NULL | 二进制形式的文件内容

### <a name="defining-rules"></a>定义规则

从目标服务器使用 PLA 收集足够多的数据后, advisor 包可以使用此数据进行验证, 并向系统管理员显示快速摘要。

规则简要概述服务器的性能。 它们重点介绍了问题并提供了建议。 可以列出要为 advisor 包验证的所有规则。 例如, 如果想要开发核心操作系统顾问包, 可能的规则包括:

* CPU 电源模式是否正在节能

* 服务器是否处于虚拟环境中

* 是否存在磁盘 i/o 压力

规则包含以下元素:

* 从属阈值 (规则的可配置部分)

* 规则定义 (警报和建议)

下面是一个简单规则的示例:

``` syntax
<advisorPack>

  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>阈值

阈值是一种可配置的因素, 使系统管理员能够决定何时规则显示良好状态或状态错误。 以下示例显示了一个规则, 用于检测系统驱动器上的可用空间, 以及在可用空间小于 10 GB 时发出警告。

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

但是, 在这种情况下, 系统管理员有一个较小的硬盘驱动器。 他认为有 5 GB 的可用空间仍是一个不错的条件, 并且不希望看到警告。 他可以通过 SPA 控制台更新默认值10到 5, 而不必了解如何开发 advisor 包。

引入阈值有助于系统管理员快速更改值, 而无需修改 advisor 包。

在此示例中, 除**description**之外的所有属性都是必需的。 您可以使用任何数值作为**值**。

阈值可在规则之间共享。

### <a name="alerts-and-recommendations"></a>警报和建议

规则定义不涉及任何逻辑计算。 它定义 UI 的外观, 以及 SQL Server 报表脚本如何将结果传递给 UI。

规则有三个部分:

* 警报 (规则标题)

* 建议 (建议)

* 关联阈值 (有关依赖项的可选信息)

下面是规则的示例:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

你可以根据需要定义多个建议, 并且通常会定义建议。 建议**级别**为 "**成功**" 或 "**警告**"。

可以链接到所需数量的阈值。 甚至可以链接到与当前规则无关的阈值。 链接有助于 SPA 控制台轻松管理阈值。

规则名称和建议是密钥, 它们在其作用域内是唯一的。 两个规则不能具有相同的名称, 并且一个规则中没有两个建议可以具有相同的名称。 编写 SQL 脚本报表时, 这些名称非常重要。 可以调用\[dbo\]。\[用于\]设置规则状态的 SetNotification API。

### <a name="defining-ui-display-elements"></a>定义 UI 显示元素

定义规则后, 系统管理员可以查看报表摘要。 不过, 系统管理员通常会对聚合数据感兴趣, 并希望检查性能规则中使用的数据源。

继续前面的示例, 用户将知道系统驱动器上是否有足够的可用磁盘空间。 用户可能还会对可用空间的实际大小感兴趣。 单个值组用于存储和显示此类结果。 多个单个值可以分组并显示在 SPA 控制台的表中。 该表只有两列: 名称和值, 如下所示。

名称 | ReplTest1
---- | ----
系统驱动器上的可用磁盘大小 (GB) | 100
已安装的磁盘总大小 (GB) | 500 

如果用户想要查看服务器上安装的所有硬盘驱动器的列表及其磁盘大小, 则可以调用包含三列和多行的列表值, 如下所示。

Disk | 可用磁盘大小 (GB) | 总大小 (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

在 advisor 包中, 可能有多个表 (单值组和列表值表)。 我们可以使用部分来组织和分类这些表。

总而言之, 有三种类型的 UI 元素:

* [个](#bkmk-ui-section)

* [单个值组](#bkmk-ui-svg)

* [列表值表](#bkmk-ui-lvt)

下面举例说明了 UI 元素:

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>个

节仅适用于 UI 布局。 它不参与任何逻辑计算。 每个报表都包含一组没有父节的顶级节。 顶级部分显示为报表中的选项卡。 节可以包含个子节, 最多可以有10个级别。 顶级部分下的所有子部分都显示在可扩展区域中。 节可以包含多个子节、单个值组和列表值表。 单值组和列表值表以表的形式出现。

下面是一个顶层部分的示例。

``` syntax
<section name="CPU" caption="CPU"/>
```

节名称必须是唯一的。 它用作可通过其他节、单个值组和列表值表链接到的键。

下面的示例具有属性**parent**, 并指向 "CPU" 部分。 CPUFacts 是名为 "CPU" 的部分的子元素。 **父代**必须引用前面的部分名称;否则, 可能会产生循环。

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

下面的单值组具有属性和**节**, 它可以基于 UI 设计指向任何部分。

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>数据类型

单个值组和列表值表包含不同类型的数据, 例如 string、int 和 float。 由于这些值存储在 SQL Server 数据库中, 因此可以为每个数据属性定义 SQL 数据类型。 不过, 定义 SQL 数据类型非常复杂。 您必须指定可能很容易更改的长度或精度。

若要定义逻辑数据类型, 可以使用 **&lt;reportdefinition.xsd/&gt;** 的第一个子级, 可在其中定义 SQL 数据类型和逻辑类型的映射。

下面的示例定义了两种数据类型。 一个是**字符串**, 另一个是**companyCode**。

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

数据类型名称可以是任何有效的字符串。 下面是允许的 SQL 数据类型的列表:

* bigint

* 二进制

* 小段

* char

* date

* DATETIME

* datetime2

* datetimeoffset

* 十进制

* 浮点数

* int

* 纸币

* nchar

* 加法

* nvarchar

* 实际上

* smalldatetime

* smallint

* smallmoney

* 时间

* tinyint

* uniqueidentifier

* varbinary

* varchar

有关这些 SQL 数据类型的详细信息, 请参阅[数据类型 (transact-sql)](https://msdn.microsoft.com/library/ms187752.aspx)。

### <a href="" id="bkmk-ui-svg"></a>单个值组

单个值组将多个单个值组合到一起显示在一个表中, 如下所示。

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

在上面的示例中, 我们定义了单个值组。 它是**SystemoverviewSection**部分的子节点。 此组具有单个值, 分别为**OsName**、 **Osversion**和**OsLocation**。

单个值必须具有全局唯一名称属性。 在此示例中, 全局唯一名称属性为**Systemoverview**。 将使用唯一名称为自定义报表生成相应的视图。 每个视图都包含前缀**vw**, 例如 vwSystemoverview。

虽然您可以定义多个单个值组, 但不能有两个单个值名称相同, 即使它们在不同的组中。 SQL 脚本报表使用单个值名称来相应地设置值。

可以为每个单个值定义一个数据类型。 **类型**的允许输入在 **&lt;datatype/&gt;** 中定义。 最终报表如下所示:

**真实**

名称 | ReplTest1
--- | ---
操作系统 | &lt;_将通过报表脚本设置值_&gt;
操作系统版本 | &lt;_将通过报表脚本设置值_&gt;
OS 位置 | &lt;_将通过报表脚本设置值_&gt;

**值/&gt;的 caption 属性显示在第一列中。 &lt;** "值" 列中的值由脚本报表通过\[dbo\]设置。\[SetSingleValue\]。 **值/&gt;的 description 特性显示在工具提示中。 &lt;** 通常, 工具提示会向用户显示数据源。 有关工具提示的详细信息, 请参阅[工具提示](#bkmk-tooltips)。

### <a href="" id="bkmk-ui-lvt"></a>列表值表

定义列表值与定义表的值相同。

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

列表值名称必须是全局唯一的。 此名称将成为临时表的名称。 在上面的示例中, 将在\#执行环境初始化阶段创建名为 NetworkAdapterInformation 的表, 其中包含所述的所有列。 与单个值名称类似, 列表值名称还用作自定义视图名称 (例如 vwNetworkAdapterInformation) 的一部分。

@type列/&gt; 由&lt;数据类型定义 &lt;&gt;

最终报表的模拟 UI 如下所示:

**物理网络适配器信息**

id | 名称 | type | 速度 (Mbps) | MAC 地址
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


&lt;列/ &lt;&gt; 的&gt; caption 特性显示为列名, 列/的**description**特性显示为相应列标题的工具提示。 通常, tooltip 会向用户显示数据源。 有关详细信息, 请参阅[工具提示](#bkmk-tooltips)。

在某些情况下, 表可能有很多列并且只有几行, 因此, 交换列和行会使表的外观更好。 若要交换列和行, 可以添加以下样式特性:

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>定义图表元素

您可以选取任何统计信息键, 并查看历史图表或趋势图中的值。 有两种类型的统计信息:

* **静态统计信息**单个值, 在设计时是已知的。 例如, 系统驱动器上的可用磁盘空间为静态统计信息。

* **动态统计信息**在设计时可能是未知的。 例如, 每个核心的平均 CPU 使用率为动态统计信息, 因为在设计时不知道系统中有多少 CPU 内核。

统计信息键限制数据必须与 double 数据类型兼容。 它可以是整数、小数或可转换为 double 的字符串。

SPA 使用单个值组来支持静态统计信息, 并使用列表值表支持动态统计信息。 以下各节介绍如何定义静态统计信息和动态统计信息键。

### <a name="static-statistics"></a>静态统计信息

如前所述, 静态统计信息是单个值。 从逻辑上讲, 可以将任何单个值定义为静态统计信息。 不过, 查看不能转换为数字类型的单个值是没有意义的。 若要定义静态统计信息, 只需将属性**trendable**添加到相应的单值键, 如下所示:

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>动态统计信息

动态统计信息键在设计时未知, 因此可能值的数目未知。 但是, 由于列表值存储在多个行中, 因此可以轻松地使用 list 值表存储动态统计信息。

例如, 如果需要显示不同内核的平均 CPU 使用率的图表, 可以定义一个表, 其中包含**CpuId**和**AverageCpuUsage**列:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

另一个属性**columntype**, 可以是**键**、**值**或**信息性**。 **键**列的数据类型必须是 double 或 double 转换。 在**键**列中, 不能在表中插入相同的键。 **值**或**信息性**列没有此限制。

统计信息值存储在**值**列中。

**信息性**列类似于普通列表值表中的普通列。 如果未指定, 则**信息**是默认列类型。 此类列不会影响统计信息键的数量, 也不会参与统计相关的计算。

继续前面的示例, 如果服务器有两个 CPU 内核, 则表中的结果如下所示:

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

同时, SPA 框架还会生成两个统计信息密钥。 一种用于 CPU 0, 另一种用于 CPU 1。

下面的示例指示支持多个具有多个**键**列的**值**列。

CounterName | InstanceName | Average | Sum
--- | :---: | :---: | :---:
% Processor time | 经 | 10 | 20
% Processor time | CPU0 | 20 | 30 

在此示例中, 有两个**键**列和两个**值**列。 SPA 为平均列生成两个统计信息键, 并为 Sum 列生成另两个键。 统计信息键是:

* CounterName (处理器时间百分比)/实例名称\_(总计)/平均

* CounterName (处理器时间百分比)/InstanceName (CPU0)/Average

* CounterName (处理器时间百分比)/实例名称\_(总计)/总计

* CounterName (处理器时间百分比)/InstanceName (CPU0)/Sum

CounterName 和 InstanceName 合并为一个键。 组合键不能有任何重复项。

SPA 生成许多统计信息密钥。 其中一些问题可能对你不感兴趣, 你可能想要将它们隐藏在 UI 中。 SPA 使开发人员可以创建筛选器以仅显示有用的统计信息密钥。

对于前面的示例, 系统管理员可能仅对 InstanceName 为\_Total 或 CPU1 的密钥感兴趣。 可以按如下所示定义筛选器:

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**trendableKeyValues/可&gt;在任何键列下定义。 &lt;** 如果有多个键列配置了此类筛选器, 则将应用逻辑。

### <a name="developing-report-scripts"></a>开发报表脚本

定义预定义元数据后, 可以开始编写报表脚本, 该脚本是一个 T-sql 存储过程。

预配元数据标头中有**name**和**reportScript**属性, 如下所示:

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

主报表脚本通过组合**name**和**reportScript**属性来命名。 在下面的示例中, 它将\[为 ServerPerformanceAdvisor. CoreOS。\]\[ReportScript\]。

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

**Name**属性将用作数据库架构名称, 例如命名空间。 此规则适用于属于当前 advisor 包的所有其他数据库对象, 例如列表值和存储过程。

在数据库对象前面具有此架构名称的优点包括:

* 避免不同 advisor 包的命名冲突

* 提供更高的安全性

在 SQL Server 数据库中, 默认架构名称为**dbo**。 数据库所有者凭据通常是在**dbo**下操作数据库对象所必需的。 如果没有为每个 advisor 包创建架构, 则有可能两个 advisor 包将定义一个同名的列表值。 这应该是不相关的, 因为你可以引入架构名称来解决此问题。 此外, 取消预配 advisor 包要容易得多。 由于 advisor 包对象属于**dbo**以外的架构, 因此, SPA 允许 SPA 使用较低的用户权限来访问它们。

正常的报表脚本会执行以下操作:

* 访问原始收集的数据

* 基于原始数据执行计算

* 更改警报和建议

* 为报表视图准备数据

### <a name="access-raw-collected-data"></a>访问原始收集的数据

收集的所有数据都将导入到以下相应的表中。 有关表架构的详细信息, 请参阅[定义数据收集器集](#bkmk-definedatacollector)。

* registry

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* 性能计数器

    * \#PerformanceCounters

* 文件

    * \#附件

* ETW

    * \#事件

    * \#EventProperties

### <a name="set-rule-status"></a>设置规则状态

\[Dbo.\]\[SetNotification\] API 设置规则状态, 以便你可以在 UI 中看到**成功**或**警告**图标。

* @ruleNamenvarchar (50)

* @adviceNamenvarchar (50)

警报和建议消息存储在设置元数据 XML 文件中。 这使报表脚本更易于管理。

最初, 每个规则状态都为 "N/A"。 可以使用此 API 通过指定建议名称来设置规则状态。 建议名称级别将用作规则状态。

回忆一下, 我们先前定义了以下规则:

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

假设可用空间小于 2 GB, 则需要将规则设置为**警告**等级。 SQL 脚本将如下所示:

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>获取阈值

\[Dbo.\]\[GetThreshold\] API 获取阈值:

* @keynvarchar (50)

* @valuefloat 输出

> [!NOTE]
> 阈值为名称/值对, 可在任何规则中引用。 系统管理员可以使用 SPA 控制台调整阈值。

 继续前面的示例, 对于阈值, 定义将如下所示:

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

可以修改报表脚本, 如下所示:

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)

```

### <a name="set-or-remove-the-single-value"></a>设置或删除单个值

\[Dbo.\]\[SetSingleValue\] API 设置单个值:

* @keynvarchar (50)

* @valuesql\_变量

对于同一单个值键, 此值可执行多次。 最后一个值将被保存。

下面的示例显示了一些定义的单个值:

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

然后, 可以设置单个值, 如下所示:

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

在极少数情况下, 可能需要删除以前使用\[dbo\]设置的结果。\[removeSingleValue\] API。

* @keynvarchar (50)

您可以使用以下脚本删除以前设置的值。

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>获取数据收集信息

\[Dbo.\]\[GetDuration\] API 获取数据收集的用户指定持续时间 (以秒为单位):

* @durationint 输出

下面是一个示例报表脚本:

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\[Dbo.\]\[GetInternal\] API 获取性能计数器的间隔。 如果当前报表没有性能计数器信息, 则它可能返回 NULL。

* @intervalint 输出

下面是一个示例报表脚本:

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>设置列表值表

没有用于更新列表值表的 API。 但是, 您可以直接访问列表值表。 在初始化阶段, 将为每个列表值创建相应的临时表。

下面的示例显示了一个列表值表:

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

然后, 你可以编写一个 SQL 脚本来插入、更新或删除结果:

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (

)
```

## <a name="development-and-debugging"></a>开发和调试


### <a name="writing-logs"></a>写入日志

如果要向系统管理员传达更多的信息, 可以编写日志。 如果有特定报表的任何日志, 将在报表表头中显示黄色横幅。 下面的示例演示如何编写日志:

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

第一个参数是要在日志中显示的消息。 第二个参数是日志级别。 第二个参数的有效输入可能是**信息性**、**警告**或**错误**。

### <a name="debug"></a>调试

SPA 控制台可在两种模式下运行: "调试" 或 "发布"。 发布模式是默认设置, 它会在生成报表后清除所有收集的原始数据。 调试模式将所有原始数据都保留在文件共享和数据库中, 以便以后可以调试报表脚本。

**调试报表脚本**

1.  安装 Microsoft SQL Server Management Studio (SSMS)。

2.  在 SSMS 启动后, 连接到 localhost\\SQLExpress。 请注意, 必须使用 localhost, 而不是。 . 否则, 可能无法在 SQL Server 中启动调试器。

3.  运行以下脚本以启用调试模式:

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  启动 SPA 控制台并运行要调试的 advisor 包。

5.  等待任务完成。 如果成功生成了报告, 请切换回 SSMS 并查找最新的任务。

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    例如, 输出可能是:

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35: 24.387 | 1

6.  您可以多次运行以下脚本, 以执行 Id 为12的报表脚本:

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **注意**还可以按 F11 单步执行前面的语句和调试。



正在\[运行\]dbo。\[DebugReportScript\]返回多个结果集, 其中包括:

1.  Microsoft SQL Server 消息和顾问包日志

2.  规则的结果

3.  统计信息键和值

4.  单个值

5.  所有列表值表

## <a name="best-practices"></a>最佳实践

### <a name="naming-convention-and-styles"></a>命名约定和样式

|                                                                 Pascal 大小写                                                                 |                       Camel 大小写                        |             一致             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>ProvisionMetadata 中的名称</li><li>存储过程</li><li>Functions</li><li>视图名称</li><li>临时表名</li></ul> | <ul><li>参数名称</li><li>局部变量</li></ul> | 用于所有 SQL 保留关键字 |

### <a name="other-recommendations"></a>其他建议

* 将大部分逻辑部分移到其他存储过程和用户定义函数中。

* 使您的主要脚本尽可能简短, 以便进行维护。

* 使用 SQL 对象的完整名称。

* 将 SQL 代码视为区分大小写。

* 将**SET NOCOUNT**添加到每个存储过程的开头。

* 请考虑使用临时表传输大量的数据。

* 如果发生错误, 请考虑使用**中的 SET\_事务中止**终止进程。

* 始终在 advisor 包显示名称中包含主要版本号。

## <a href="" id="bkmk-advancedtopics"></a>高级主题

### <a name="run-multiple-advisor-packs-simultaneously"></a>同时运行多个 advisor 包

SPA 支持同时运行多个 advisor 包。 当你希望同时查看 Internet Information Services (IIS) 和核心操作系统性能时, 此方法特别有用。 IIS advisor 包所使用的许多数据收集器也可能由核心 OS advisor 包使用。 当两个或多个顾问包在同一台目标计算机上运行时, SPA 不会两次收集相同的数据。

下面的示例演示运行两个 advisor 包的工作流。

![运行多个 advisor 包](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

合并数据收集器集仅用于收集性能计数器和 ETW 数据源。 以下合并规则适用:

1. SPA 将最长的持续时间作为新的持续时间。

2. 如果存在合并冲突, 请遵循以下规则:

   1. 使用最小的时间间隔作为新的间隔。

   2. 获取性能计数器的超级集。 例如, 在**进程 (\*\\)% Processor time**和**process (\*)\\\*中,\\process (\*)\\\\** *返回更多数据, 因此将从合并的数据收集器集中删除**进程\*()\\% Processor time**和 **\*process ()\\\\** *。

### <a name="collect-dynamic-data"></a>收集动态数据

SPA 需要在设计时定义数据收集器集。 并非总是可以知道生成报表所需的数据, 因为动态数据和查询路径在其从属数据可用之前是未知的。

例如, 如果想要列出网络适配器的所有友好名称, 则必须先查询 WMI 来枚举所有网络适配器。 每个返回的 WMI 对象都有一个注册表项路径, 其中存储了友好名称。 注册表项路径在设计时是未知的。 在这种情况下, 我们需要动态数据支持。

若要枚举所有网络适配器, 可以使用 Windows PowerShell 执行以下 WMI 查询:

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

它将返回网络适配器对象的列表。 每个对象都有一个名为**PNPDeviceID**的属性, 该属性维护一个相对的注册表项路径。 下面是前一个查询的示例输出:

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

若要查找**FriendlyName**值, 请打开注册表编辑器并通过将**HKEY\_本地\_计算机\\\\系统 CurrentControlSet\\枚举\\合并到注册表设置**与上一示例中的每一行。 , 例如:**HKEY\_本地计算机\_系统\\CurrentControlSet枚举\\根IPHTTPS0000\\。\\\\\\\***

若要将上述步骤转换为 SPA 预配元数据, 请在下面的代码示例中添加脚本:

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

在此示例中, 首先在 managementpaths 下添加 WMI 查询, 并定义密钥名称**网络适配器**。 然后, 添加一个注册表项, 并使用语法 " **$ (网络适配器. PNPDeviceID)** " 来引用**网络适配器**。

下表定义了 SPA 中的数据收集器是否支持动态数据, 以及其他数据收集器是否可以引用它:

数据类型 | 支持动态数据 | 可以引用
--- | :---: | :---:
注册表项 | 是 | 是
WMI | 是 | 是
文件 | 是 | 否
性能计数器 | 否 | 否
ETW | 否 | 否

对于 WMI 数据收集器, 每个 WMI 对象都有多个附加属性。 任何类型的 WMI 对象始终具有三个属性:\_\_命名空间\_、 \_类和\_RELpath。 \_

若要定义由其他数据收集器引用的数据收集器, 请在 ProvisionMetadata 中使用唯一键分配**name**属性。 依赖数据收集器使用此密钥生成动态数据。

下面是注册表项的示例:

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

WMI 的示例如下:

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

若要定义从属数据收集器, 请使用以下语法: $ ( *{name}* 。 *{attribute}* )。

*{name}* 和 *{attribute}* 是占位符。

当 SPA 从目标服务器收集数据时, 它会将模式 $ (\*.\*) 替换为其引用数据收集器 (注册表项/WMI) 的实际收集数据, 例如:

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**注意**SPA 支持无限制的引用深度, 但如果级别过多, 请注意性能开销。 请确保不存在不受支持的循环引用或自引用。

### <a name="versioning-limitations"></a>版本控制限制

SPA 支持重置和次要版本更新。 这些进程使用相同的算法。 此过程是刷新所有数据库对象和阈值设置, 但保留现有数据。 这可以升级到更高版本, 也可以降级到更低的版本。 选择 advisor 包, 然后单击 SPA 中的 "**配置 Advisor 包**" 对话框中的 "**重置**" 以重置或应用或更新。

此功能主要用于次更新。 您无法显著更改 UI 显示元素。 如果需要进行重大更改, 则必须创建不同的 advisor 包。 应将主版本包含在 advisor 包名称中。

次要版本修改的限制是**不能**执行下列任何操作:

* 更改架构名称

* 更改任何单个值组或列表值表的列的数据类型

* 添加或删除阈值

* 添加或删除规则

* 添加或删除建议

* 添加或删除单个值

* 添加或删除列表值

* 添加或删除列表值的列

### <a href="" id="bkmk-tooltips"></a>Tooltip

几乎所有**说明**属性都将在 SPA 控制台中显示为工具提示。

对于列表值表, 可以通过添加以下属性来实现基于行的工具提示:

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

**DescriptionColumn**属性指的是列的名称。 在此示例中, "说明" 列不显示为物理列。 但是, 当鼠标悬停在第一列的每一行上时, 它会显示为工具提示。

建议工具提示向用户显示数据源。 下面是用于显示数据源的格式:

数据源 | 格式 | 示例
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;字段/&lt;&gt; | WMIWin32_OperatingSystem/Caption
性能计数器 | Perfcounter finish&lt;类别&gt;名称/InstanceName&lt;&gt; | Perfcounter finish进程/处理器时间百分比
registry | 注册表: &lt;registerKey&gt; | registryHKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
配置文件 | Read-configfile&lt;Filepath&gt;;\[Xpath&lt;Xpath&gt;\]<br>**注意**<br>Xpath 是可选的, 仅当文件是 xml 文件时才有效。 | Read-configfile: windir%\\System32\\inetsrv\config\\applicationhost.config<br>Xpath: 配置&frasl;system.webserver<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW&lt;Provider/&gt;(关键字) | ETWWindows 内核跟踪 (进程、网络)

### <a name="table-collation"></a>表排序规则

当 advisor 包变得更加复杂时, 您可以创建自己的变量表或临时表来存储报表脚本中的中间结果。

排序字符串列可能会产生问题, 因为您创建的表排序规则可能不同于 SPA 框架创建的排序规则。 如果将两个字符串列关联在不同的表中, 则可能会看到排序规则错误。 若要避免此问题, 应始终将列排序规则的字符串定义为**SQL\_latin1-general\_General\_CP1\_CI\_** , 就像在定义表时那样。

下面介绍如何定义变量表:

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>收集 ETW

下面介绍如何在 ProvisionMetadata 文件中定义 ETW:

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

以下提供程序属性可用于收集 ETW:

特性 | type | 描述
--- | --- | ---
GUID | GUID | 提供程序 GUID
会话 | string | ETW 会话名称 (可选, 仅限内核事件需要)
keywordsany | Hex | 任何关键字 (可选, 无0x 前缀)
keywordsAll | Hex | 所有关键字 (可选)
properties | Hex | 属性 (可选)
level | Hex | 级别 (可选)
bufferSize | 整型 | 缓冲区大小 (可选)
flushtime | 整型 | 刷新时间 (可选)
maxBuffer | 整型 | 最大缓冲区 (可选)
minBuffer | 整型 | 最小缓冲区 (可选)

这里有两个输出表, 如下所示。

**\#事件表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceID | Int NOT NULL | 相关序列 ID
EventtypeId | Int NOT NULL | 事件类型 ID (请参阅 [dbo]. [Eventtypes])
ProcessId | BigInt NOT NULL | 进程 ID
ThreadId | BigInt NOT NULL | 线程 ID
标志 | datetime2 NOT NULL | 标志
Kerneltime | BigInt NOT NULL | 内核时间
Usertime | BigInt NOT NULL | 用户时间

**\#EventProperties 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceID | Int NOT NULL | 相关序列 ID
名称 | Nvarchar(100) | 属性名
ReplTest1 | Nvarchar (4000) | ReplTest1

### <a name="etw-schema"></a>ETW 架构

可以通过对 .etl 文件运行 tracerpt 来生成 ETW 架构。 生成一个架构文件。 由于 .etl 文件的格式依赖于计算机, 以下脚本仅适用于以下情况:

1.  在收集相应 .etl 文件的计算机上运行该脚本。

2.  或在安装了同一操作系统和组件的计算机上运行该脚本。

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>术语表


本文档中使用了以下术语:

**Advisor 包**

Advisor pack 是用于处理从目标服务器收集的性能日志的元数据和 SQL 脚本的集合。 然后, advisor 包将从性能日志数据生成报表。 Advisor 包中的元数据定义要从目标服务器收集的数据以提高性能。 该元数据还定义规则集、阈值和报表格式。 通常情况下, 会专门针对单个服务器角色 (例如 Internet Information Services (IIS)) 编写 advisor 包。

**SPA 控制台**

SPA 控制台是指 SpaConsole, 它是服务器性能顾问的核心部分。 SPA 无需在要测试的目标服务器上运行。 SPA 控制台包含 SPA 的所有用户界面, 从设置项目到运行分析和查看报表。 按照设计, SPA 是两层应用程序。 SPA 控制台包含 UI 层和部分业务逻辑层。 SPA 控制台计划并处理性能分析请求。

**SPA 框架**

SPA 包含两个主要部分: 框架和顾问包。 SPA 框架提供了所有用户界面、性能日志处理、配置、错误处理以及数据库 Api 和管理过程。

**SPA 项目**

SPA 项目是一个数据库, 其中包含有关在 advisor 包的目标服务器上生成的目标服务器、advisor 包和性能分析报表的所有信息。 您可以在同一 SPA 项目中比较和查看历史记录和趋势图表。 用户可以创建多个项目。 SPA 项目彼此独立, 且没有跨项目共享数据。

**目标服务器**

目标服务器是运行 Windows Server 的物理计算机或虚拟机, 其中包含某些服务器角色, 如 IIS。

**数据分析会话**

数据分析会话是针对特定目标服务器的性能分析。 数据分析会话可包含多个 advisor 包。 来自这些顾问包的数据收集器集将合并到单个数据收集器集中。 单个数据分析会话的所有性能日志在同一时间段内收集。 分析在同一数据分析会话中运行的 advisor 包生成的报表有助于用户了解总体性能情况, 并确定性能问题的根本原因。

**Windows 事件跟踪**

Windows[事件跟踪](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)(ETW) 是 windows 操作系统中提供的高性能、低开销、可缩放的跟踪系统。 它提供分析和调试功能, 可用于对各种方案进行故障排除。 SPA 使用 ETW 事件作为数据源来生成性能报告。 有关 ETW 的一般信息, 请参阅[通过 Etw 改善调试和性能优化](https://msdn.microsoft.com/magazine/cc163437.aspx)。

**WMI 查询**

Windows Management Instrumentation (WMI) 是 Windows 操作系统中管理数据和操作的基础结构。 你可以编写 WMI 脚本或应用程序, 以在远程计算机上自动执行管理任务。 WMI 还向操作系统的其他部分和产品提供管理数据。 SPA 使用 WMI 类信息和数据点作为源来生成性能报告。

**性能计数器**

性能计数器用于提供有关操作系统或应用程序、服务或驱动程序的运行状况的信息。 性能计数器数据可帮助确定系统瓶颈以及微调系统和应用程序性能。 操作系统、网络和设备提供应用程序可以使用的计数器数据, 以便为用户提供系统运行状况的图形视图。 SPA 使用性能计数器信息和数据点作为源来生成性能报告。

**性能日志和警报**

性能日志和警报 (PLA) 是 Windows 操作系统中的内置服务。 它旨在收集性能日志和跟踪, 还会在满足某些触发器时引发性能警报。 PLA 可用于收集性能计数器、Windows 事件跟踪 (ETW)、WMI 查询、注册表项和配置文件。 PLA 还支持通过远程过程调用 (RPC) 进行远程数据收集。 用户定义数据收集器集, 其中包括有关要收集的数据的信息、数据收集的频率、数据收集持续时间、筛选器以及保存结果文件的位置。 SPA 使用 PLA 来收集目标服务器中的所有性能数据。

**单个报表**

单个报表是基于单个目标服务器上一个 advisor 包的一个数据分析会话生成的 SPA 报表。 它可以包含通知和各种数据部分。

**并排报表**

并行报表是一种 SPA 报表, 用于比较同一 advisor 包的两个单个报表。 可以从不同的目标服务器或从同一目标服务器上的单独性能分析运行生成这两个报告。 并行报告将创建比较两个报告的功能, 以帮助用户识别其中一个报告中的异常行为或设置。 并行报表包含通知和各种数据部分。 在每个部分中, 并排列出了这两个报表中的数据。

**趋势图**

趋势图是用于调查性能问题的重复模式的 SPA 报表。 许多重复的性能问题都是由服务器或客户端计算机上的计划服务器负载更改引起的, 这些更改可能会每天或每周进行一次。 SPA 提供24小时趋势图和7天趋势图来确定这些问题。

用户可以一次选择一个或多个数据序列, 这是单个报表中的一个数值, 例如**平均总 CPU 使用率**。 更确切地说, 数值是由单个服务器在给定的时间实例生成的标量值。 SPA 将这些值分组为24个组, 一天中每个小时一次 (7 天, 每周报告一次)。 SPA 计算每个组的平均值、最小值、最大值和标准偏差。

**历史图表**

历史图表是一种 SPA 报表, 用于显示特定服务器和顾问包对一段时间内特定数值的更改。 用户可以选择多个数据序列并将它们一起显示在历史图表中, 以了解不同的数据系列之间的相关性。

**数据序列**

数据序列是在一段时间内从同一数据源收集的数值数据。 同一源表示数据必须来自同一目标服务器, 如一台服务器上的 IIS 的平均请求队列长度。

**原则**

规则是逻辑、阈值和说明的组合。 它们表示潜在的性能问题。 每个 advisor 包都包含多个规则。 每个规则由报表生成进程触发。 规则将逻辑和阈值应用于单个报表中的数据。 如果满足条件, 则会发出警告通知。 否则, 通知将设置为 **"正常"** 状态。 如果规则不适用, 则通知设置为 "不适用 (**NA**)" 状态。

**通知**

通知是规则向用户显示的信息。 它包括规则的状态 ( **"确定**"、" **NA**" 或 "**警告**")、规则的名称, 以及解决性能问题的可能建议。

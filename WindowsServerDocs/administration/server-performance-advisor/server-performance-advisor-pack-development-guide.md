---
title: Server Performance Advisor 包开发指南
description: Server Performance Advisor 包开发指南
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c647e8a335aac924067d92dcb41ab4d17e0cceef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884858"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Server Performance Advisor 包开发指南

>适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows 8、 Windows 10

此开发指南的 Microsoft Server Performance Advisor (SPA) 提供了有助于开发人员和系统管理员开发的 advisor 包来分析服务器性能的指导原则。

它假定你熟悉性能日志和警报 (PLA)、 性能计数器、 注册表设置、 Windows Management Instrumentation (WMI)、 事件跟踪 Windows (ETW) 和 Transact SQL (T-SQL)。

使用 SPA 的详细信息，请参阅[Server Performance Advisor 用户指南](server-performance-advisor-users-guide.md)。

## <a name="spa-advisor-pack-overview"></a>SPA 顾问包概述


Advisor 包通常设计为特定服务器角色，并定义以下：

* 收集通过 PLA，包括 Windows Management Instrumentation (WMI)、 性能计数器、 注册表设置、 文件和事件跟踪 Windows (ETW) 数据

* 显示警报和建议的规则

* 数据 （收集的原始数据、 聚合的数据或前 10 列表） 显示

* 若要查看随时间变化的值的统计信息

* 可以趋于的统计信息值

Advisor 包包括以下元素：

* **XML 元数据**(ProvisionMetadata.xml)

    * [性能日志和警报 (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)数据收集器集

    * 报表布局

* **SQL 脚本**

    * 主要的存储的过程

    * SQL 对象，如存储的过程和用户定义的函数

* **ETW 架构文件**(Schema.man) 这是可选的

### <a name="advisor-pack-workflow"></a>顾问包工作流

![顾问包工作流](../media/server-performance-advisor/spa-dev-guide-workflow.png)

此流程图中的绿色圆圈表示 advisor 包。 所有其他圆圈表示 SPA 框架的过程中运行的阶段。 SPA 使用 advisor 包来收集数据、 将数据导入数据库、 初始化执行环境和运行 SQL 脚本。

### <a name="collect-data"></a>收集数据

Advisor 包使用 SPA 中排队的特定服务器上时，数据收集模块查询 advisor 包中的数据收集器集 XML，并从目标服务器中收集数据。 原始数据存储在用户指定的文件共享。 超出运行持续时间由用户指定的 SPA，不会停止数据收集。

### <a name="import-data-into-the-database"></a>数据导入数据库

数据收集完成后，每种类型的数据将导入 SQL Server 数据库中的相应表中。 例如，注册表设置导入到名为的表\#registryKeys。

导入 ETW 文件需要用于解码.etl 文件的 ETW 架构文件。 ETW 架构文件是一个 XML 文件。 它可以通过使用 tracerpt.exe，包含在 Windows 生成。 才 ETW 架构文件时需要将 ETW 数据导入 advisor 包所需。

### <a name="switch-to-low-user-rights"></a>切换到较低的用户权限

SPA 框架自动调整最大程度减少所需的安全访问级别的权限。 因为 advisor 包可以开发或修改的任何人，很可能包含被篡改的 SQL 脚本的顾问包。 若要缓解安全风险，应使用较低的用户权限运行 advisor 包任何 SQL 脚本。 它仅可以访问受限制的数据库对象，如临时表和 SPA Api 公开为存储过程。 Advisor 包中的 SQL 脚本可以调用这些存储过程与 SPA 框架进行交互。

### <a name="initialize-execution-environment"></a>初始化执行环境

Advisor 包可以生成不同类型的输出，例如通知、 建议、 事实数据表、 统计信息和统计信息的图表。 SQL 脚本来执行某些计算针对收集的数据。 已生成的结果存储在临时表通过 SPA 公共 Api。 在初始化阶段中，这些临时表和其他系统资源都需要将其预配。

### <a name="run-sql-scripts"></a>运行 SQL 脚本

没有主要的存储的过程，由 advisor 包开发人员。 SPA 框架将调用此存储的过程以启动计算。 存储的过程使用所收集的数据，并将最终结果传递给 SPA 框架。

### <a name="switch-to-administrative-rights"></a>切换到管理权限

需要管理员权限才能生成报表。 由 SPA 完全控制生成报告。 它是不太可能会被篡改。

### <a name="generate-a-report"></a>生成报告

Advisor 包程序的主要存储的过程完成之前，不会保留所有计算的结果，例如通知和统计信息。 在此阶段，SPA 框架的最终结果将从传输临时表到特定的格式中的表。 此阶段完成后，可以通过使用 SPA 控制台中查看报表。

## <a name="authoring-an-advisor-pack"></a>创作 advisor 包


### <a name="quick-guidelines"></a>快速指南

以下流程图描述了，以便用于开发完全正常运行 advisor 包的步骤。 本部分还包括分步示例，以更好地解释每个步骤。

![advisor 包开发过程](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Advisor 包通常结构如下所示：

Advisor 包

ProvisionMetadata.xml

脚本

main.sql

func.sql

Schema.man

每个顾问包必须具有一个名为 ProvisionMetadata.xml 文件。 它定义基本顾问包信息，若要收集，通知和规则，以及如何报表需要存储并显示哪些数据。 SPA 框架使用此信息来生成一个临时表，然后将临时表中的结果传输到的用户可以访问的表。

所有报告的 SQL 脚本必须将保存在名为的子文件夹**脚本**。 出于维护目的，我们建议你在不同的 SQL Server 文件中保存不同的数据库对象。 作为主入口点必须有至少一个存储的过程。

> [!NOTE]
> 除非 advisor 包收集 ETW 跟踪，则不需要 schema.man 文件。 此架构文件用于描述 ETW 事件的架构并进行解码 ETW 事件。

### <a name="defining-basic-information"></a>定义的基本信息

本部分介绍的一些基本元素组成的顾问包，包括 ProvisionMetadata.xml 和属性。

下面是一个示例标头 ProvisionMetadata.xml 文件：

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

### <a name="advisor-pack-version"></a>顾问包版本

属性名称：**版本**

Advisor 包开发人员可以定义为 advisor 包的主要和次要版本：

* 主要版本通常涉及到重大改进。 由旧版本生成的结果可能与新兼容。 我们强烈建议在顾问包名称中包括的主版本。

* SPA 允许次要版本升级时有次要更改与没有数据不兼容性问题。

有关版本控制的详细信息，请参阅[高级主题](#bkmk-advancedtopics)。

### <a name="script-entry-point"></a>脚本入口点

属性名称： **reportScript**

SPA 框架从脚本入口点的主存储的过程名称查找并运行安全的方式。

### <a name="other-attributes"></a>其他属性

下面是可以用于标识 advisor 包的其他一些特性：

* 显示名称： **displayName**

* 说明：**说明**

* 作者：**作者**

* Framework 版本： **frameworkversion** （默认值为 3.0）

* 最低操作系统版本： **minOSversion** （这保留为将来的扩展）

* 丢失事件通知： **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>定义数据收集器集

数据收集器集定义 SPA 框架应从目标服务器收集的性能数据。 它支持注册表设置、 WMI、 性能计数器，从目标服务器和 ETW 文件。

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

**持续时间**的属性**&lt;dataCollectorSet /&gt;** 上一示例中定义的数据收集 （时间单位为秒） 的持续时间。 **持续时间**是必需的属性。 此设置控制由性能计数器和 ETW 收集持续时间。

### <a name="collect-registry-data"></a>收集注册表数据

可以从以下注册表配置单元收集注册表数据：

* HKEY\_类\_根

* HKEY\_CURrenT\_CONFIG

* HKEY\_CURrenT\_USER

* HKEY\_本地\_机

* HKEY\_用户

若要收集的注册表设置，请指定值名称的完整路径：HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

若要收集的所有注册表项下设置，请指定注册表项的完整路径：HKEY\_LOCAL\_MACHINE\\MyKey\\

若要收集的注册表项和 （PLA 以递归方式收集注册表数据） 及其子密钥下的所有值，使用两个反斜杠的最后一个路径分隔符：HKEY\_LOCAL\_MACHINE\\MyKey\\\\

若要从远程计算机收集的注册表信息，包括在注册表路径开头的计算机名称：HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

例如，您可能必须按如下所示显示的注册表项：

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

示例 1：返回仅 active PowerSchemes 和它们的值：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

示例 2：返回此路径下的所有键/值对：

> [!NOTE]
> PLA 在用户凭据下运行。 一些注册表项需要管理凭据。 无法访问的任何子密钥时，会停止枚举。

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

收集的所有数据将导都入到名为的临时表 **\#registryKeys** SQL 报表之前运行脚本。 下表显示了结果例如 2:

KeyName | KeytypeId | ReplTest1
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | 电源优化
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

有关架构 **#registryKeys**表是按如下所示：

列名 | SQL 数据类型 | 描述
-------- | -------- | --------
KeyName | Nvarchar(300) 不为 NULL | 注册表项的完整路径名称
KeytypeId | smallint NOT NULL | 内部类型 Id
ReplTest1 | Nvarchar(4000) 不为 NULL | 所有值

**KeytypeID**列可以具有以下类型之一：
ID | 在任务栏的搜索框中键入
--- | ---
1 | 字符串
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

您可以添加任何 WMI 查询。 有关如何编写 WMI 查询的详细信息，请参阅[WQL (SQL for WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx)。下面的示例查询页文件位置：

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

在上面的示例查询将返回一条记录：

标题 | 名称 | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

由于 WMI 返回具有不同列的表时收集的数据导入到数据库时，SPA 执行数据规范化，并添加对以下表：

**\#WMIObjects 表**

SequenceID | 命名空间 | ClassName | Relativepath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\pagefile.sys | 1

**\#WmiObjectsProperties 表**

ID | 查询
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#WmiQueries 表**

ID | 查询
--- | ---
1 | Root\Cimv2:select * FROM Win32_PageFileUsage

**\#WmiObjects 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceId | int NOT NULL | 关联的行和其属性
命名空间 | Nvarchar(200) 不为 NULL | WMI 命名空间
ClassName | Nvarchar(200) 不为 NULL | WMI 类名
Relativepath | Nvarchar(500) 不为 NULL | WMI 相对路径
WmiqueryId | int NOT NULL | 将相关联 #WmiQueries 的键

**\#WmiObjectProperties 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceId | int NOT NULL | 关联的行和其属性
名称 | Nvarchar(1000) 不为 NULL | 属性名
ReplTest1 | Nvarchar(4000) NULL | 当前属性的值

**\#WmiQueries 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
ID | int NOT NULL | > 唯一的查询 ID
查询 | Nvarchar(4000) 不为 NULL | 在设置元数据中的原始查询字符串

### <a name="collect-performance-counters"></a>收集性能计数器

下面举例说明如何收集性能计数器 s:

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

**间隔**属性是必需的全局设置的所有性能计数器。 它定义的间隔收集性能数据的 （时间单位为秒）。

在上一示例中，计数器\\PhysicalDisk (\*)\\avg.Disk sec/Transfer 将查询每隔一秒。

可能有两个实例：**\_总**和**0 c:D:**，并且输出可能如下所示：

timestamp | CategoryName | CounterName | _Total 实例值 | 实例值为 0 的 c:D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Avg.Disk sec/Transfer | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Avg.Disk sec/Transfer | 0.000933297607934224 | 0.000933297607934224

将数据导入到数据库，数据将规范化到名为的表 **\#PerformanceCounters**。

CategoryDisplayName | InstanceName | CounterdisplayName | ReplTest1
---- | ---- | ---- | ----
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00100008362473995
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00280023414927187
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.00385999853230048
PhysicalDisk | _Total | Avg.Disk sec/Transfer | 0.000933297607934224
PhysicalDisk | 0 C:D: | Avg.Disk sec/Transfer | 0.000933297607934224

**请注意**本地化的名称，如**CategoryDisplayName**并**CounterdisplayName**，目标服务器上使用的显示语言而异。 避免使用这些字段，如果你想要创建非特定于语言的 advisor 包。

**\#PerformanceCounters**表架构

列名 | SQL 数据类型 | 描述
---- | ---- | ---- | ----
timestamp | datetime2(3) NOT NULL | 在 UNC 中收集的日期时间
CategoryName | Nvarchar(200) 不为 NULL | 类别名称
CategoryDisplayName | Nvarchar(200) 不为 NULL | 本地化的类别名称
InstanceName | Nvarchar(200) NULL | 实例名
CounterName | Nvarchar(200) 不为 NULL | 计数器名
CounterdisplayName | Nvarchar(200) 不为 NULL | 本地化的计数器名称
值 | Float 不为 NULL | 收集的值

### <a name="collect-files"></a>收集文件

路径可以是绝对或相对。 文件名可以包含通配符字符 (\*) 和问号 （？）。 例如，若要收集的临时文件夹中的所有文件，可指定 c:\\temp\\\*。 通配符字符应用于指定的文件夹中的文件。

如果你想要从指定的文件夹的子文件夹中还收集文件，最后一个文件夹分隔符，例如，c： 使用两个反斜杠\\temp\\\\\*。

下面提供的示例，查询**applicationHost.config**文件：

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

可以在名为的表中找到结果**\#文件**，例如：

querypath | fullpath | Parentpath | FileName | 内容
----- | ----- | ----- | ----- | -----
%windir%\...\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#文件表架构**

列名 | SQL 数据类型 | 描述
---- | ---- | ----
querypath | Nvarchar(300) 不为 NULL | 原始查询语句
fullpath | Nvarchar(300) 不为 NULL | 绝对文件路径和文件名
Parentpath | Nvarchar(300) 不为 NULL | 文件路径
FileName | Nvarchar(300) 不为 NULL | 文件名
内容 | Varbinary （max) NULL | 二进制文件中的文件内容

### <a name="defining-rules"></a>定义规则

通过从目标服务器使用 PLA 收集足够的数据后，advisor 包可以使用此数据进行验证，并向系统管理员显示快速摘要。

规则提供有关服务器的快速概述的性能。 它们突出显示的问题并提供建议。 可以列出所有想要验证 advisor 包的规则。 例如，如果你想要开发核心操作系统顾问包，则可能规则可能包括：

* CPU 电源模式是否正在保存 power

* 服务器是否在虚拟化环境中

* 是否存在磁盘 I/O 压力

规则包含以下元素：

* 依赖阈值 （可配置规则的一部分）

* 规则定义 （警报和建议）

下面提供的一个简单的规则示例：

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

阈值是一个可配置的因素，使系统管理员可以决定时应显示一条规则，好或错误状态。 下面的示例显示了一个规则来检测上系统驱动器和警告的可用空间小于 10 GB 的可用空间时。

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

但是，在这种情况下，系统管理员会具有较小的硬盘驱动器。 他认为 5 GB 的可用空间可能仍处于良好状态，并且他不希望看到一条警告。 他可以更新通过 SPA 控制台的 5 到 10 中的默认值，而无需了解如何开发顾问包。

引入阈值可帮助系统管理员快速更改而无需修改 advisor 包的值。

在示例中，所有属性除外**说明**所需。 可以使用任意数量的**值**。

阈值可以在规则之间共享。

### <a name="alerts-and-recommendations"></a>警报和建议

规则定义不涉及任何逻辑计算。 它定义了 UI 可能看起来和 SQL Server 报告脚本的方式进行通信的 ui 的结果。

规则包含三个部分：

* 警报 （规则标题）

* 建议 （建议）

* 相关的阈值 （有关依赖关系的可选信息）

下面是规则的示例：

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

您可以定义尽可能多的建议根据需要，您通常需要定义的建议。 **级别**可以是建议**成功**或**警告**。

您可以链接到所需的任意多个阈值。 甚至可以链接到与当前的规则的阈值。 链接可帮助轻松地管理阈值的 SPA 控制台。

规则名称和建议密钥，并且它们都是将其唯一作用域内。 没有两个规则可以具有相同的名称，并在一个规则中的没有两个建议可以具有相同的名称。 编写 SQL 脚本报告时，这些名称将是非常重要。 您可以调用\[dbo\]。\[SetNotification\] API 来设置规则状态。

### <a name="defining-ui-display-elements"></a>定义显示的 UI 元素

在定义规则之后，系统管理员可以查看摘要报表。 但是，通常是系统管理员感兴趣的聚合数据，并且他们想要检查的性能规则中使用的数据源。

继续上述示例中，用户知道系统驱动器上是否存在足够的可用磁盘空间。 用户还可能感兴趣的可用空间的实际大小。 单个值组用于存储和显示此类结果。 可对多个单一值进行分组和 SPA 控制台中的表中所示。 表中的只有两个列，名称和值，如下所示。

名称 | 值
---- | ----
系统驱动器 (GB) 上的可用磁盘大小 | 100
安装的总磁盘大小 (GB) | 500 

如果用户想要查看安装在服务器和其磁盘大小的所有硬盘驱动器的列表，我们可以调用一个列表值，该值具有三个列和多个行，如下所示。

Disk | 可用磁盘大小 (GB) | 总大小 (GB)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

在顾问包中，可能有多个表 （单个值组和列出值表）。 我们可以使用部分进行组织和分类这些表。

总之，有三种类型的 UI 元素：

* [部分](#bkmk-ui-section)

* [单个值组](#bkmk-ui-svg)

* [列出值表](#bkmk-ui-lvt)

此处 s 示例，显示的 UI 元素：

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

### <a href="" id="bkmk-ui-section"></a>部分

部分是纯粹的 UI 布局。 它不参与任何逻辑计算。 每个单个报表包含一组不具有父节的顶级部分。 顶级部分显示为报表中的选项卡。 节可包含子节，最多 10 个级别。 可展开区域中会显示在顶级部分下的所有子节。 一节可以包含多个小节、 单个值组和列出值表。 单个值组和列出值表显示为表。

下面是顶级部分的示例。

``` syntax
<section name="CPU" caption="CPU"/>
```

节名称必须是唯一的。 它用作可以由其他部分、 单个值组和列出值表链接到的密钥。

下面的示例有一个属性，**父**，并指向部分 CPU。 CPUFacts 是节的名为 CPU 的子级。 **父**必须引用以前的部分名称; 否则，可能导致一个循环。

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

以下的单值组具有一个属性，**部分**，并可以指向任何部分中，根据您的 UI 设计。

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>数据类型

单个值组和列表值表包含不同类型的数据，例如字符串、 int 和 float。 由于这些值存储在 SQL Server 数据库中，可以定义每个数据属性的 SQL 数据类型。 但是，定义 SQL 数据类型是十分复杂。 您必须指定长度或精度，可能会变化。

若要定义逻辑的数据类型，可以使用的第一个子级 **&lt;reportDefinition /&gt;**，这是您可以在其中定义的 SQL 数据类型和逻辑类型的映射。

下面的示例定义两种数据类型。 一个是**字符串**，另一个是**companyCode**。

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

数据类型名称可以是任何有效的字符串。 下面是允许的 SQL 数据类型的列表：

* bigint

* 二进制文件

* 位

* Char

* date

* DATETIME

* datetime2

* datetimeoffset

* 十进制

* 浮点数

* int

* 资金

* nchar

* 数值

* nvarchar

* 真正

* smalldatetime

* smallint

* smallmoney

* 时间

* tinyint

* 唯一标识符

* varbinary

* varchar

有关这些 SQL 数据类型的详细信息，请参阅[数据类型 (Transact SQL)](https://msdn.microsoft.com/library/ms187752.aspx)。

### <a href="" id="bkmk-ui-svg"></a>单个值组

单个值组进行分组在一起，如下所示的表中显示的多个单个值。

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

在上一示例中，我们定义的单个值组。 它是子节点的部分**SystemoverviewSection**。 此组具有单个值，即**OsName**， **Osversion**，并**OsLocation**。

单个值必须具有全局唯一的名称属性。 在此示例中，全局唯一的名称属性是**Systemoverview**。 唯一名称将用于生成自定义报表的相应视图。 每个视图包含前缀**vw**，如 vwSystemoverview。

尽管可以定义多个单个值组，但没有两个单个值名称可以是相同的即使它们位于不同的组。 SQL 脚本报表使用的单个值名称以相应地设置值。

可以定义每个单个值的数据类型。 允许的输入**类型**中定义**&lt;数据类型 /&gt;**。 最终报表可能如下所示：

**Facts**

名称 | ReplTest1
--- | ---
操作系统 | &lt;_报表脚本时将设置一个值_&gt;
操作系统版本 | &lt;_报表脚本时将设置一个值_&gt;
OS 位置 | &lt;_报表脚本时将设置一个值_&gt;

**标题**的属性**&lt;值 /&gt;** 第一列中显示。 通过脚本报表值列中的值设置在将来\[dbo\]。\[SetSingleValue\]。 **描述**的属性**&lt;值 /&gt;** 工具提示中所示。 通常的工具提示显示用户的数据源。 工具提示的详细信息，请参阅[工具提示](#bkmk-tooltips)。

### <a href="" id="bkmk-ui-lvt"></a>列出值表

定义一个列表值是作为与定义表相同。

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

列表值名称必须全局唯一。 此名称将成为临时表的名称。 在上一示例中，名为的表\#NetworkAdapterInformation 将创建在执行环境初始化阶段，其中包含所述的所有列。 类似于单个值名称，列表值名称还用作自定义视图名称，例如，vwNetworkAdapterInformation 的一部分。

@type &lt;列 /&gt;定义的&lt;数据类型 /&gt;

最终报表的模拟的用户界面可能如下所示：

**物理网络适配器信息**

ID | 名称 | 在任务栏的搜索框中键入 | 速度 (Mbps) | MAC 地址
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


**标题**的属性&lt;列 /&gt;显示为列名称，并**说明**属性的&lt;列 /&gt;所示的工具提示相应的列标题。 通常在工具提示显示的用户的数据源。 有关详细信息，请参阅[工具提示](#bkmk-tooltips)。

在某些情况下，表可能具有大量的列，只有几行，因此交换行和列会使表看起来会好得多。 若要交换的列和行，可以添加下列样式属性：

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>定义图表元素

可以选择任何统计信息键，并查看历史图表或趋势图表中的值。 有两种类型的统计信息：

* **静态统计信息**在设计时已知的单个值。 例如，在系统驱动器上的可用磁盘空间将是静态的统计信息。

* **动态统计信息**可能在设计时未知。 例如，每个核心的平均 CPU 使用率是动态的统计信息，因为您不知道在设计时无法在系统中的 CPU 核心数。

统计信息键具有限制的数据必须与双精度数据类型兼容。 它可以是整数、 十进制数字，或者可以转换为双精度值的字符串。

SPA 使用单个值组来支持静态统计信息和支持动态统计信息的列表值表。 以下部分介绍如何定义静态统计量和动态统计信息键。

### <a name="static-statistics"></a>静态统计信息

如前所述，静态的统计信息是单个值。 在逻辑上，任何单个值可定义为静态的统计信息。 但是，它是无意义，若要查看单个值不能强制转换为数字类型。 若要定义静态统计信息，您可以只需添加属性**trendable**向相应单个值的项所示如下：

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>动态统计信息

因此数量的可能值是未知，动态统计信息键不会知道在设计时。 但是，由于列表值存储在多个行，它可以很容易地使用列表值表来存储动态统计信息。

例如，如果我们需要显示不同的内核的平均 CPU 使用率的图表，我们可以定义包含有关列的表**CpuId**并**AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

另一个属性， **columntype**，可以是**密钥**，**值**，或者**信息**。 数据类型**密钥**列必须是双精度或双精度的转换。 在中**密钥**列中，您不能向表中插入相同的密钥。 **值**或**Informational**列不具有此限制。

统计信息值存储在**值**列。

**信息性**列就像正常的列表值表中的普通列。 **信息性**是默认列类型，如果不指定其中一个。 这样的列将不会影响统计信息键数或者参与相关的统计信息计算。

继续上述示例中，如果服务器具有两个 CPU 内核，表中的结果可能如下所示：

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

在同一时间，由 SPA 框架生成两个统计信息键。 一个用于 CPU 0，另一个是有关 CPU 1。

如下面的示例指示多个**值**与多个列**密钥**支持的列。

CounterName | InstanceName | 平均值 | Sum
--- | :---: | :---: | :---:
% Processor time | _Total | 10 | 20
% Processor time | CPU0 | 20 | 30 

在此示例中，有两个**键**列和两个**值**列。 SPA 将生成两个统计信息键的平均的列和另一个 Sum 列的两个密钥。 统计信息键包括：

* CounterName （%处理器时间） / InstanceName (\_总) / 平均

* CounterName （%处理器时间） / InstanceName (卸下 cpu 0) / 平均

* CounterName （%处理器时间） / InstanceName (\_总) / 求和

* CounterName （%处理器时间） / InstanceName (卸下 cpu 0) / 求和

CounterName 和 InstanceName 都组合为一个键。 组合的密钥不能包含任何重复项。

SPA 生成统计信息的多个键。 其中一些可能感兴趣，并可能想要在 UI 中将其隐藏。 SPA 支持开发人员创建一个筛选器以显示仅有用的统计信息键。

对于上一示例中，系统管理员可能仅有兴趣实例名称是在其中的密钥\_总页数或 CPU1。 可以按如下所示定义的筛选器：

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

**&lt;trendableKeyValues /&gt;** 可以下任何键列定义。 如果有多个一个键列应用了筛选配置，并将应用逻辑。

### <a name="developing-report-scripts"></a>开发报表脚本

设置元数据定义后，我们可以开始编写报表脚本，这是 T-SQL 存储过程。

有**名称**并**reportScript**属性中设置元数据标头，如下所示：

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

主报表脚本名为通过组合**名称**并**reportScript**属性。 在以下示例中，它将是\[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\]。\[ReportScript\]。

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

**名称**属性将用作数据库架构名称，如命名空间。 此规则适用于所有属于当前 advisor 包，如列表值和存储的过程的其他数据库对象。

此架构名称的数据库对象的前面好处包括：

* 避免命名冲突有关的不同 advisor 包

* 提供更高的安全性

在 SQL Server 数据库的默认架构名称是**dbo**。 通常需要使用数据库所有者凭据才能运行下的数据库对象**dbo**。 如果我们没有创建每个 advisor 包的架构，则很可能两个顾问包将定义具有相同名称的列表值。 这应该是不相关，因为您可能会导致架构名称，若要解决此问题。 此外，取消预配 advisor 包要容易得多。 因为顾问包对象而不属于架构**dbo**，这使 SPA 使用较低的用户权限来访问它们。

正常报表脚本执行以下任务：

* 访问原始收集的数据

* 执行基于原始数据的计算

* 更改警报和建议

* 准备用于报表视图数据

### <a name="access-raw-collected-data"></a>访问原始收集的数据

收集的所有数据将都导入以下相应的表。 有关表架构的详细信息，请参阅[定义的数据收集器集](#bkmk-definedatacollector)。

* 注册表

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* 性能计数器

    * \#PerformanceCounters

* 文件

    * \#文件

* ETW

    * \#事件

    * \#EventProperties

### <a name="set-rule-status"></a>设置规则状态

\[Dbo\]。\[SetNotification\] API 设置规则状态，以便您可以看到**成功**或**警告**在 UI 中的图标。

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

设置元数据 XML 文件中存储的警报和建议消息。 这使得报表脚本更易于管理。

最初，每个规则状态是不适用。 此 API 可用于通过指定的建议名称设置规则状态。 级别的建议名称将用作规则状态。

还记得我们之前定义以下规则：

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

假设小于 2 GB 的可用空间，我们需要将规则设置为**警告**级别。 SQL 脚本将按如下所示：

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

\[Dbo\]。\[GetThreshold\] API 获取阈值：

* @key nvarchar(50)

* @value float 输出

> [!NOTE]
> 阈值是名称 / 值对，并可以任何规则中引用它们。 系统管理员可以使用 SPA 控制台来调整的阈值。

 继续上述示例中，针对某个阈值定义将按如下所示：

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

可以修改报表脚本，如下所示：

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)
 
```

### <a name="set-or-remove-the-single-value"></a>设置或删除单个值

\[Dbo\]。\[SetSingleValue\] API 集的单个值：

* @key nvarchar(50)

* @value sql\_variant

此值可以多次执行相同的单个值键。 保存的最后一个值。

下面的示例显示了一些定义单个值：

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

然后可以设置单个值，如下所示：

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

在极少数情况下，你可能想要删除以前使用设置的结果\[dbo\]。\[removeSingleValue\] API。

* @key nvarchar(50)

您可以使用以下脚本删除以前设置的值。

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>获取数据收集信息

\[Dbo\]。\[GetDuration\] API 获取用户指定持续时间以秒为单位的数据收集：

* @duration int 输出

此处 s 示例报告脚本：

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\[Dbo\]。\[GetInternal\] API 获取性能计数器的间隔。 如果当前报表不具有性能计数器信息，它可能返回 NULL。

* @interval int 输出

此处 s 示例报告脚本：

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>设置的列表值表

有关更新列表值表没有 API。 但是，你可以直接访问列表值表。 在初始化阶段中，将创建相应的临时表，每个列表值。

下面的示例显示一个列表值表：

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

然后可以编写 SQL 脚本，插入、 更新或删除的结果：

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

如果你想要与系统管理员进行通信的任何进一步信息，您可以编写日志。 如果没有为特定报表的任何日志，将在报表表头中显示黄色的横幅。 下面的示例演示如何编写一个日志：

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

第一个参数是所需的消息显示在日志中。 第二个参数是日志级别。 第二个参数的有效输入可以是**Informational**，**警告**，或**错误**。

### <a name="debug"></a>调试

SPA 控制台可以运行在两种模式中调试或发布。 发布模式下是默认值，并生成报表后清除所有收集的原始数据。 调试模式下在文件共享和数据库，将所有原始数据，以便您可以在将来调试报表脚本。

**若要调试报表脚本**

1.  安装 Microsoft SQL Server Management Studio (SSMS)。

2.  启动 SSMS 后，连接到本地主机\\SQLExpress。 请注意，您必须使用本地主机，而不是。 . 否则，您可能不能在 SQL Server 启动调试器。

3.  运行以下脚本以启用调试模式下：

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  启动 SPA 控制台并运行你想要调试的顾问包。

5.  等待任务完成。 如果已成功生成报告时，切换回 SSMS 并查找最新的任务。

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    例如，输出可能是：

    ID | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  您可以根据你想要对 Id 12 执行报表脚本多次运行以下脚本：

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **请注意**还可以按 F11 单步执行的上一条语句和调试。

     

运行\[dbo\]。\[DebugReportScript\]返回多个结果集，其中包括：

1.  Microsoft SQL Server 消息和顾问包日志

2.  规则的结果

3.  统计信息键和值

4.  单个值

5.  列表值的所有表

## <a name="best-practices"></a>最佳做法

### <a name="naming-convention-and-styles"></a>命名约定和样式

Pascal 大小写 | 驼峰式大小写 | 大写
--- | ---- | ---
<ul><li>ProvisionMetadata.xml 中的名称</li><li>存储的过程</li><li>函数</li><li>视图名称</li><li>临时表的名称</li></ul> | <ul><li>参数名称</li><li>本地变量</li></ul> | 用于所有 SQL 保留关键字

### <a name="other-recommendations"></a>其他建议

* 将最逻辑部分移动到其他存储的过程和用户定义的函数。

* 使主脚本越简单越好出于维护目的。

* 使用 SQL 对象的完整名称。

* 将 SQL 代码视为区分大小写。

* 添加**SET NOCOUNT ON**到每个存储过程的开头。

* 请考虑使用临时表来传输大量数据。

* 请考虑使用**设置 XACT\_中止 ON**终止进程，如果发生错误。

* 始终在顾问包显示名称中包含主版本号。

## <a href="" id="bkmk-advancedtopics"></a>高级的主题

### <a name="run-multiple-advisor-packs-simultaneously"></a>同时运行多个 advisor 包

SPA 支持同时运行多个 advisor 包。 如果想要查看 Internet 信息服务 (IIS) 和核心操作系统性能，同时，这是特别有用。 核心操作系统顾问包还可能使用的 IIS 顾问包使用的许多数据收集器。 当两个或多个 advisor 包相同的目标计算机上运行时，SPA 不会两次收集相同的数据。

下面的示例显示了用于运行两个顾问包工作流。

![运行多个 advisor 包](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

合并数据收集器集项仅用于收集性能计数器和 ETW 数据源。 以下合并规则适用：

1.  SPA 需要为新的持续时间的最大持续时间。

2.  其中有合并冲突，应遵循以下规则：

    1.  将作为新的时间间隔的最小时间间隔。

    2.  需要性能计数器的超集。 例如，对于**进程 (\*)\\处理器时间百分比**和**进程 (\*)\\\*，\\进程 (\*)\\\*** 返回更多的数据，因此**过程 (\*)\\处理器时间百分比**并**进程 (\*)\\ \*** 已从合并的数据收集器集。

### <a name="collect-dynamic-data"></a>收集动态数据

SPA 需求定义的数据收集器集在设计时。 它并不总是可能知道哪些数据需要生成报告，因为其依赖的数据可用之前不知道的动态数据和查询路径。

例如，如果你想要列出所有网络适配器的友好名称，则必须首先查询 WMI 来枚举所有网络适配器。 每个返回 WMI 对象具有注册表项路径，它可以将存储的友好名称。 在设计时是未知的注册表项路径。 在这种情况下，我们需要动态数据支持。

若要枚举所有网络适配器，可以使用 Windows PowerShell 中使用以下 WMI 查询：

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

它返回的网络适配器对象的列表。 每个对象都有一个名为属性**PNPDeviceID**，可以维护相对注册表项路径。 下面提供前面的查询的示例输出：

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000
 
```

若要查找**FriendlyName**值，打开注册表编辑器并导航到注册表的设置通过组合**HKEY\_本地\_机\\系统\\CurrentControlSet\\Enum\\** 前面的示例中的每一行。 例如：**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Enum\\ ROOT\\\*IPHTTPS\\0000**.

若要转换为 SPA 预配元数据的上一步骤，请在下面的代码示例中添加脚本：

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

在此示例中，您首先添加下 managementpaths WMI 查询和定义密钥名称**网络适配器**。 然后，添加注册表项和是指**网络适配器**使用语法 **$(NetworkAdapter.PNPDeviceID)**。

下表定义了如果在 SPA 中的数据收集器支持动态数据和是否可以引用它的其他数据收集器：

数据类型 | 支持动态数据 | 可以引用
--- | :---: | :---:
注册表项 | 是 | 是
WMI | 是 | 是
文件 | 是 | 否
性能计数器 | 否 | 否
ETW | 否 | 否

WMI 数据收集器，对于每个 WMI 对象具有多个附加的属性。 任何类型的 WMI 对象始终具有三个属性：\_\_命名空间中， \_\_类，并\_ \_RELpath。

若要定义一个数据收集器，引用的其他数据收集器，将分配**名称**与 ProvisionMetadata.xml 中唯一的键属性。 依赖于数据收集器使用此密钥来生成动态数据。

下面提供一个示例注册表项：

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

和 WMI 的一个示例：

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

若要定义依赖数据收集器，请使用以下语法: $(*{name}*。*{attribute}*).

*{name}* 并 *{attribute}* 是占位符。

SPA 收集数据时从目标服务器，动态替换模式 $(\*。\*) 与实际从其引用数据收集器收集的数据 (注册表项 / WMI)，例如：

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**请注意**SPA 支持无限的深度的引用，但请注意的性能开销，如果包含过多级别。 请确保不存在循环引用或自引用的不受支持。

### <a name="versioning-limitations"></a>版本控制限制

SPA 支持重置和次要版本更新。 这些进程使用相同的算法。 过程是刷新所有数据库对象和阈值设置，但保留现有数据。 这可以升级到更高版本或降级到更低版本。 选择顾问包，然后依次**重置**中**配置 Advisor 包**SPA，重置或将应用或更新中的对话框。

此功能主要是次要更新。 您不能显著更改 UI 显示元素。 如果你想要进行重大更改，您必须创建不同的 advisor 包。 应顾问包名称中包含的主要版本。

次要版本修改的限制是，你**无法**执行下列任一操作：

* 更改架构名称

* 更改任何单个值组的数据类型或表的列的列表值

* 添加或删除阈值

* 添加或删除规则

* 添加或删除建议

* 添加或删除单个值

* 添加或删除列表值

* 添加或删除列表值的列

### <a href="" id="bkmk-tooltips"></a>工具提示

几乎所有**说明**属性将显示为工具提示中的 SPA 控制台。

有关列表的值表，可以通过添加以下属性来实现基于行的工具提示：

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

**DescriptionColumn**属性引用的列的名称。 在此示例中，说明列不显示为物理列。 但是，它显示为工具提示时，将鼠标移到第一列的每个行。

我们建议在工具提示向用户显示数据源。 下面是用于显示数据源的格式：

数据源 | 格式 | 示例
--- | --- | ---
WMI | WMI: &lt;wmiclass&gt;/&lt;字段&gt; | WMI:Win32_OperatingSystem/Caption
性能计数器 | Perfcounter:&lt;CategoryName&gt;/&lt;InstanceName&gt; | Perfcounter:Process/%处理器时间百分比
注册表 | 注册表： &lt;registerKey&gt; | 注册表：HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
配置文件 | ConfigFile:&lt;Filepath&gt;\[;Xpath:&lt;Xpath&gt;\]<br>**注意**<br>Xpath 是可选的该文件是一个 xml 文件时，才会有效。 | ConfigFile: windir %\\System32\\inetsrv\config\\applicationHost.config<br>Xpath： 配置&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW:&lt;提供程序 /&gt;（关键字） | ETW:Windows 内核跟踪 （进程，net）

### <a name="table-collation"></a>表排序规则

当 advisor 包变得更复杂的时可以创建自己的变量的表或临时表来存储中间结果在报表的脚本中。

排序规则字符串列可能会出现问题，因为您创建的表排序规则可能不同于创建的 SPA 框架。 如果关联在不同表中的两个字符串列，可能会看到一个排序规则错误。 若要避免此问题，应始终定义为与列的排序规则的字符串**SQL\_Latin1\_常规\_CP1\_CI\_AS**时定义的表。

下面提供如何定义变量的表：

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>收集 ETW

下面提供如何定义 ETW ProvisionMetadata.xml 文件中：

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

以下提供程序属性是可用于收集 ETW 使用：

特性 | 在任务栏的搜索框中键入 | 描述
--- | --- | ---
GUID | GUID | 提供程序 GUID
会话 | string | ETW 会话名称 （可选，才需要执行内核事件）
keywordsany | Hex | 任何关键字 (可选，没有 0x 前缀)
keywordsAll | Hex | （可选） 的所有关键字
properties | Hex | 属性 （可选）
level | Hex | （可选） 级别
bufferSize | 整型 | 缓冲区大小 （可选）
flushtime | 整型 | （可选） 刷新时间
maxBuffer | 整型 | 最大缓冲区 （可选）
minBuffer | 整型 | 最小缓冲区 （可选）

有两个输出表，如下所示。

**\#事件表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceID | int NOT NULL | 相关序列 ID
EventtypeId | int NOT NULL | 事件类型 ID (请参阅 [dbo]。 [Eventtypes])
ProcessId | 不为 NULL 的 BigInt | 进程 ID
ThreadId | 不为 NULL 的 BigInt | 线程 ID
timestamp | datetime2 NOT NULL | timestamp
Kerneltime | 不为 NULL 的 BigInt | 内核时间
Usertime | 不为 NULL 的 BigInt | 用户时间

**\#EventProperties 表架构**

列名 | SQL 数据类型 | 描述
--- | --- | ---
SequenceID | int NOT NULL | 相关序列 ID
名称 | Nvarchar(100) | 属性名
ReplTest1 | Nvarchar(4000) | ReplTest1

### <a name="etw-schema"></a>ETW 架构

可以通过针对.etl 文件运行 tracerpt.exe 生成 ETW 架构。 生成 schema.man 文件。 因为.etl 文件的格式是依赖计算机，以下脚本将仅适用于以下情况下：

1.  从中收集相应的.etl 文件在计算机上运行该脚本。

2.  或使用相同的操作系统和安装组件的计算机上运行该脚本。

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>术语表


本文档中使用以下术语：

**Advisor 包**

Advisor 包是元数据和处理从目标服务器收集的性能日志的 SQL 脚本的集合。 Advisor 包然后从性能日志数据生成报表。 Advisor 包中的元数据定义要收集的目标服务器的性能度量值的数据。 元数据还定义规则、 阈值和报表格式的组。 大多数情况下，advisor 包写入专用于单个服务器角色，例如，Internet 信息服务 (IIS)。

**SPA 控制台**

SPA 控制台是指 SpaConsole.exe，这是服务器性能审查程序的核心部分。 SPA 不需要在要测试的目标服务器上运行。 SPA 控制台为 SPA，包含所有用户界面从运行分析和查看报表的项目进行设置。 根据设计，SPA 是一个两层应用程序。 SPA 控制台包含 UI 层和业务逻辑层的一部分。 SPA 控制台计划和处理性能分析请求。

**SPA 框架**

SPA 包含两个主要部分、 框架和 advisor 包。 SPA 框架提供了所有的用户界面、 性能日志处理、 配置、 错误处理和数据库 Api，并管理过程。

**SPA 项目**

SPA 项目是包含有关目标服务器、 advisor 包 advisor 包在目标服务器上生成的性能分析报告的所有信息的数据库。 你可以比较，并查看同一 SPA 项目中的历史记录和趋势图表。 用户可以创建多个项目。 SPA 项目是独立的并且不没有在项目之间共享任何数据。

**目标服务器**

目标服务器是物理计算机或运行 Windows Server，某些服务器角色，如 IIS 的虚拟机。

**数据分析会话**

数据分析会话是特定的目标服务器上的性能分析。 数据分析会话可以包含多个 advisor 包。 这些顾问包中的数据收集器集将合并到单个数据收集器集。 在同一时间段期间收集的单个数据分析会话的所有性能日志。 分析生成的 advisor 包相同的数据分析会话中运行的报表可以帮助用户了解总体性能状况并确定性能问题的根本原因。

**Windows 事件跟踪**

[事件跟踪](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)Windows (ETW) 是 Windows 操作系统中提供的高性能、 低开销、 可缩放跟踪系统。 它提供了分析和调试功能，可用于解决各种方案。 SPA 使用 ETW 事件作为数据源生成性能报表。 有关 ETW 的常规信息，请参阅[改善调试和性能优化使用 ETW](https://msdn.microsoft.com/magazine/cc163437.aspx)。

**WMI 查询**

Windows Management Instrumentation (WMI) 是用于管理数据和在 Windows 操作系统中的操作的基础结构。 您可以编写 WMI 脚本或应用程序自动执行管理任务在远程计算机上。 WMI 还提供了其他部分的操作系统和产品管理数据。 SPA 使用 WMI 类信息和数据点作为源生成的性能报表。

**性能计数器**

性能计数器用于提供有关如何执行操作系统或应用程序、 服务或驱动程序的信息。 性能计数器数据可以帮助确定系统瓶颈以及微调系统和应用程序性能。 操作系统、 网络和设备提供应用程序可以使用为用户提供的系统的执行图形视图的计数器数据。 SPA 使用性能计数器信息和数据点作为源来生成性能报告。

**性能日志和警报**

性能日志和警报 (PLA) 是 Windows 操作系统中内置的服务。 这为了收集性能日志和跟踪，并且在满足某些触发器时还将性能警报。 PLA 可以用于收集性能计数器、 事件跟踪 Windows (ETW)、 WMI 查询、 注册表项和配置文件。 PLA 还支持通过远程过程调用 (RPC) 的远程数据收集。 用户定义数据收集器集，其中包括有关要收集数据、 数据收集、 数据收集持续时间、 筛选器和保存结果文件的位置的频率的信息。 SPA 使用 PLA 从目标服务器中收集所有性能数据。

**单个报表**

单个报表是生成的 SPA 报表基于一个 advisor 包单个目标服务器上的一个数据分析会话。 它可以包含通知和各种数据部分。

**通过并行报表**

通过并行报表是 SPA 报告，用于比较两个相同 advisor 包的单个报表。 从不同的目标服务器或不同的性能分析运行相同的目标服务器上，可以生成两个报表。 通过并行报表创建比较两个报告，以帮助用户识别异常行为，或者在一个报表设置的功能。 通过并行报表包含通知和各种数据部分。 在每个部分中，这两个报告中的数据是列出的并排方案。

**趋势图**

趋势图是用于调查性能问题的重复模式的 SPA 报告。 从服务器或客户端计算机，可能每天或每周，许多重复的性能问题被由于计划的服务器负载发生变化的。 SPA 提供 24 小时趋势图表和 7 天的使用趋势图表，以识别这些问题。

用户可以在某个时间，如是数字值在单个报告中，选择一个或多个数据序列**平均总 CPU 使用率**。 更具体地说，数字值是从生成在给定的时间实例上单个 AP 的一台服务器一个标量值。 SPA 这些值进行分组为 24 的组，一个用于在一天 （7 天报表中，一个用于每个日期是星期几的 7 个） 的每个小时。 SPA 计算平均值、 最小值、 最大值和标准偏差为每个组。

**历史图表**

历史图表是用于更改特定数值内对给定的服务器和顾问包对单个报表中显示随时间推移的 SPA 报告。 用户可以选择多个数据序列和历史图表以了解不同的数据序列之间的相关性一起显示。

**数据序列**

数据序列是一段时间内从同一数据源中收集的数值数据。 相同的源代码就意味着这些数据必须来自同一个目标服务器，例如一台服务器上的 IIS 请求的平均队列长度。

**规则**

规则是逻辑、 阈值和说明的组合。 它们表示潜在的性能问题。 每个顾问包包含多个规则。 每个规则由报表生成过程触发。 某个规则应用于单个报表中的数据的逻辑和阈值。 如果满足条件，将引发警告通知。 如果不是，通知设置为**确定**状态。 如果规则不适用，通知将设置为不适用 (**NA**) 状态。

**通知**

通知是向用户显示一条规则的信息。 它包括规则的状态 (**确定**， **NA**，或**警告**)，则名称的规则，并建议解决性能问题。

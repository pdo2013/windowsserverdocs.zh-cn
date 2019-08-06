---
title: schtasks
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f898c67755fb8e7874932ea06a7cdf461d1e4bd
ms.sourcegitcommit: e40fce7b8b4bc0bef278e676435306f14078cf00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2019
ms.locfileid: "68787204"
---
# <a name="schtasks"></a>schtasks



计划定期或在特定时间运行的命令和程序。 添加和删除计划中的任务, 启动和停止按需任务, 并显示和更改计划任务。

若要查看命令语法, 请单击以下命令之一:
-   [schtasks 创建](#BKMK_create)
-   [schtasks 更改](#BKMK_change)
-   [schtasks 运行](#BKMK_run)
-   [schtasks 结束](#BKMK_end)
-   [schtasks 删除](#BKMK_delete)
-   [schtasks 查询](#BKMK_query)

## <a name="remarks"></a>备注

- **Schtasks.exe**与**控制面板**中的 "**计划任务**" 执行相同的操作。 可以将这些工具一起使用, 并且可以互换使用。
- **Schtasks**替代了在以前版本的 Windows 中包括的 **.exe。** 尽管 Windows Server 2003 家族仍包含 **.exe** , 但建议使用**schtasks**命令行任务计划工具。
- **Schtasks**命令中的参数可以按任意顺序出现。 在不使用任何参数的情况下键入**schtasks**会执行查询。
- 适用于**schtasks**的权限  
  -   您必须有权运行该命令。 任何用户都可以在本地计算机上计划任务, 并可以查看和更改他们计划的任务。 Administrators 组的成员可以计划、查看和更改本地计算机上的所有任务。
  -   若要计划、查看或更改远程计算机上的任务, 你必须是远程计算机上 Administrators 组的成员, 或者必须使用 **/u**参数提供远程计算机的管理员的凭据。
  -   仅当本地计算机和远程计算机位于同一个域中, 或者本地计算机位于远程计算机域信任的域中时, 才可以在 **/create**或 **/change**操作中使用 **/u**参数。 否则, 远程计算机将无法对指定的用户帐户进行身份验证, 并且无法验证该帐户是否为 Administrators 组的成员。
  -   该任务必须具有运行权限。 所需权限因任务而异。 默认情况下, 任务使用本地计算机的当前用户的权限运行, 或使用 **/u**参数指定的用户权限 (如果包括在内) 来运行。 若要使用其他用户帐户或系统权限的权限运行任务, 请使用 **/ru**参数。
- 若要验证计划任务是否运行, 或找出计划任务未运行的原因, 请参阅任务计划程序服务事务日志, *SystemRoot*\SchedLgU.txt。 此日志记录了所有使用该服务的工具 (包括**计划任务**和**schtasks.exe**) 启动的运行。
- 在极少数情况下, 任务文件会损坏。 损坏的任务不会运行。 尝试对损坏的任务执行操作时, **schtasks.exe**显示以下错误消息:  
  ```
  ERROR: The data is invalid.
  ```  
  你无法恢复损坏的任务。 若要还原系统的任务计划功能, 请使用**schtasks.exe**或**计划任务**从系统中删除任务并重新计划。

## <a name="BKMK_create"></a>schtasks 创建

计划任务。

**Schtasks**对每个计划类型使用不同的参数组合。 若要查看用于创建任务的组合语法或查看用于创建具有特定计划类型的任务的语法, 请单击以下选项之一。
-   [组合语法和参数说明](#BKMK_syntax)
-   [计划每 N 分钟运行一次的任务](#BKMK_minutes)
-   [计划每 N 小时运行一次的任务](#BKMK_hours)
-   [计划每 N 天运行的任务](#BKMK_days)
-   [计划每 N 周运行的任务](#BKMK_weeks)
-   [计划每 N 个月运行一次的任务](#BKMK_months)
-   [计划任务在一周的特定天运行](#BKMK_spec_day)
-   [计划任务在月份的特定周运行](#BKMK_spec_week)
-   [计划每月特定日期运行的任务](#BKMK_spec_date)
-   [计划任务在一个月的最后一天运行](#BKMK_last_day)
-   [计划运行一次的任务](#BKMK_once)
-   [计划每次系统启动时运行的任务](#BKMK_startup)
-   [计划在用户登录时运行的任务](#BKMK_logon)
-   [计划在系统空闲时运行的任务](#BKMK_idle)
-   [计划立即运行的任务](#BKMK_now)
-   [计划使用不同权限运行的任务](#BKMK_diff_perms)
-   [计划以系统权限运行的任务](#BKMK_sys_perms)
-   [计划运行多个程序的任务](#BKMK_multi_progs)
-   [计划在远程计算机上运行的任务](#BKMK_remote)

### <a name="BKMK_syntax"></a>组合语法和参数说明

#### <a name="syntax"></a>语法

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Parameters

##### <a name="sc-scheduletype"></a>/sc \<ScheduleType >

指定计划类型。 有效值为分钟、每小时、每天、每周、每月、一次、ONSTART、ONLOGON、ONIDLE。

|计划类型|描述|
|-------------|-----------|
|分钟、每小时、每日、每周、每月|指定计划的时间单位。|
|曾经|任务在指定的日期和时间运行一次。|
|ONSTART|该任务在系统每次启动时运行。 你可以指定开始日期, 或在系统下一次启动时运行该任务。|
|ONLOGON|只要用户 (任何用户) 登录, 任务就会运行。 你可以指定一个日期, 或在用户下次登录时运行该任务。|
|ONIDLE|只要系统在指定的时间段内处于空闲状态, 该任务就会运行。 您可以指定一个日期, 或在系统下一次空闲时运行该任务。|

##### <a name="tn-taskname"></a>/tn \<TaskName >

指定任务的名称。 系统上的每项任务都必须具有唯一的名称。 名称必须符合文件名规则, 并且不得超过238个字符。 使用引号将包含空格的名称括起来。

##### <a name="tr-taskrun"></a>/tr \<k >

指定任务运行的程序或命令。 键入可执行文件、脚本文件或批处理文件的完全限定路径和文件名。 路径名称不能超过262个字符。 如果省略路径, 则**schtasks**会假定文件位于*SystemRoot*\System32 目录中。

##### <a name="s-computer"></a>/s \<计算机 >

计划指定远程计算机上的任务。 键入远程计算机的名称或 IP 地址 (带有或不带反斜杠)。 默认值为本地计算机。 **/U**和 **/p**参数仅在使用 **/s**时有效。

##### <a name="u-domainuser"></a>/u [\<域 >\]<User>

用指定用户帐户的权限运行此命令。 默认值是本地计算机当前用户的权限。 **/U**和 **/p**参数仅对在远程计算机上计划任务有效 ( **/s**)。

指定帐户的权限用于计划任务和运行任务。 若要使用其他用户的权限运行任务, 请使用 **/ru**参数。

用户帐户必须是远程计算机上 Administrators 组的成员。 同时, 本地计算机必须与远程计算机位于同一域中, 或者必须位于远程计算机域信任的域中。

##### <a name="p-password"></a>/p \<密码 >

提供 **/u**参数中指定的用户帐户的密码。 如果你使用 **/u**参数, 但省略 **/p**参数或 password 参数, 则**schtasks**会提示你输入密码并掩盖你键入的文本。

**/U**和 **/p**参数仅对在远程计算机上计划任务有效 ( **/s**)。

##### <a name="ru-domainuser--system"></a>/ru {[\<域 >\] <User> |主板

用指定用户帐户的权限运行任务。 默认情况下, 任务使用本地计算机的当前用户的权限运行, 或使用 **/u**参数指定的用户的权限 (如果包括在内) 来运行。 在本地或远程计算机上计划任务时, **/ru**参数是有效的。


|       值        |                                                    描述                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<域 >\]<User> |                                       指定另一个用户帐户。                                        |
|    系统或 ""    | 指定本地系统帐户, 该帐户是操作系统和系统服务使用的一个高特权帐户。 |

##### <a name="rp-password"></a>/rp \<密码 >

提供 **/ru**参数中指定的用户帐户的密码。 如果在指定用户帐户时省略此参数, 则**schtasks.exe**会提示你输入密码并掩盖你键入的文本。

不要将任务的 **/rp**参数用于使用系统帐户凭据 ( **/ru system**) 运行的任务。 系统帐户没有密码, 并且**schtasks.exe**不会提示输入密码。

##### <a name="mo-modifier"></a>/mo \<修饰符 >

指定任务在其计划类型中的运行频率。 对于每分钟、每小时、每日、每周和每月计划, 此参数均有效, 但可选。 默认值为 1。

|计划类型|修饰符值|描述|
|-------------|---------------|-----------|
|分钟|1 - 1439|任务每 N > \<分钟运行一次。|
|工资|1 - 23|任务每 N > \<小时运行一次。|
|日历|1 - 365|任务每 N > \<天运行一次。|
|两|1 - 52|任务每 N > \<周运行一次。|
|曾经|无修饰符。|任务运行一次。|
|ONSTART|无修饰符。|任务在启动时运行。|
|ONLOGON|无修饰符。|当 **/u**参数指定的用户登录时, 该任务将运行。|
|ONIDLE|无修饰符。|当系统空闲时间为 **/i**参数指定的分钟数 (这是与 ONIDLE 一起使用所需的分钟数) 后, 任务运行。|
|每月|1 - 12|任务每 N > \<个月运行一次。|
|每月|LASTDAY|该任务在每月的最后一天运行。|
|每月|第一个、第二个、第三个、第四个、最后一个|与 **/d**\<Day > 参数一起使用, 以便在特定周和天运行任务。 例如, 在月份的第三个星期三。|

##### <a name="d-dayday--"></a>/d Day [, Day ...] |*

指定一周中的某一天 (或几天) 或一个月中的一天 (或几天)。 仅对每周或每月计划有效。


| 计划类型 |              修饰符              |     Day 值 (/d)      |                                                                                                 描述                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    两     |               1 - 52               | 周一至周日 [, 周一至周日 ...] |                                                                                                     \*                                                                                                      |
|    每月    | 第一个、第二个、第三个、第四个、最后一个 |        周一至周日         |                                                                                   对于特定周计划是必需的。                                                                                    |
|    每月    |          无或 {1-12}          |          1 - 31          | 可选且仅在不使用修饰符 ( **/mo**) 参数 (特定日期计划) 时或在 **/mo**为 1-12 ("每\<N > 月" 计划) 时有效。 默认值为 day 1 (每月的第一天)。 |

##### <a name="m-monthmonth"></a>/m Month [, Month ...]

指定计划的任务应在一年中的哪个月或几个月运行。 有效值为1月12日和 * (每月)。 **/M**参数仅对每月计划有效。 使用 LASTDAY 修饰符时是必需的。 否则, 它是可选的, 默认值为 * (每月)。

##### <a name="i-idletime"></a>/i \<IdleTime >

指定在任务启动前计算机处于空闲状态的分钟数。 有效值为从1到999的整数。 此参数仅对 ONIDLE 计划有效, 因此是必需的。

##### <a name="st-starttime"></a>/st \<StartTime >

指定任务在 HH: MM > 24 小时格式中\<开始 (每次启动) 的时间。 默认值为本地计算机上的当前时间。 **/St**参数对于分钟、每小时、每天、每周、每月和计划有效。 需要该计划。

##### <a name="ri-interval"></a>/ri \<间隔 >

指定重复间隔, 以分钟为单位。 这不适用于计划类型:分钟、小时、ONSTART、ONLOGON 和 ONIDLE。 有效范围为1到599940分钟 (599940 分钟 = 9999 小时)。 如果指定了/ET 或/DU, 则重复间隔默认为10分钟。

##### <a name="et-endtime"></a>/et \<EndTime >

指定每分钟或每小时任务计划以\<HH: MM > 24 小时格式结束的时间。 在指定的结束时间之后, **schtasks**不会再次启动任务, 直到开始时间重复。 默认情况下, 任务计划没有结束时间。 此参数是可选的, 仅在每分钟或每小时计划时有效。

有关示例, 请参阅:
-   "计划在非工作时间运行每100分钟一次的任务" 的计划**任务,** \<该任务将每 N >**分钟**运行一次。

##### <a name="du-duration"></a>/du \<Duration >

指定\<HHHH: MM > 24 小时格式的分钟或小时计划的最大时间长度。 经过指定的时间后, **schtasks**将不会重新启动任务, 直到开始时间重复。 默认情况下, 任务计划没有最大持续时间。 此参数是可选的, 仅在每分钟或每小时计划时有效。

有关示例, 请参阅:
-   "将每3小时运行一次的任务计划为10小时",**计划任务每** \<N >**小时**运行一次。

##### <a name="k"></a>遇到

停止任务在 **/et**或 **/du**指定的时间运行的程序。 如果不使用 **/k**, 则**schtasks**在到达由 **/et**或 **/du**指定的时间后不会重新启动该程序, 但如果程序仍在运行, 则它不会停止它。 此参数是可选的, 仅在每分钟或每小时计划时有效。

有关示例, 请参阅:
-   "计划在非工作时间运行每100分钟一次的任务" 的计划**任务,** \<该任务将每 N >**分钟**运行一次。

##### <a name="sd-startdate"></a>/sd \<开始 >

指定任务计划开始的日期。 默认值为本地计算机上的当前日期。 **/Sd**参数对于所有计划类型有效且可选。

"开始*日期*" 的格式因 "**控制面板**" 的 "**区域和语言选项**" 中为本地计算机选择的区域设置而异。 每个区域设置只有一种格式有效。

下表列出了有效的日期格式。 使用与在本地计算机上的 **"控制面板**"中 "**区域和语言选项**" 中选择的格式最相似的格式。


|       ReplTest1       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | 用于月份优先格式, 例如**英语 (美国)** 和**西班牙语 (巴拿马)** 。 |
| \<DD >/<MM>/<YYYY> |       用于第一天的格式, 如**保加利亚语**和**荷兰语 (荷兰)** 。        |
| \<YYYY >/<MM>/<DD> |          用于年份优先格式, 如**瑞典语**和**法语 (加拿大)** 。          |

/ed \<终止 >

指定计划的结束日期。 该参数为可选参数。 它在一次、ONSTART、ONLOGON 或 ONIDLE 计划中无效。 默认情况下, 计划无结束日期。

*结束*日期的格式因 "**控制面板**" 的 "**区域和语言选项**" 中为本地计算机选择的区域设置而异。 每个区域设置只有一种格式有效。

下表列出了有效的日期格式。 使用与在本地计算机上的 **"控制面板**"中 "**区域和语言选项**" 中选择的格式最相似的格式。


|       ReplTest1       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | 用于月份优先格式, 例如**英语 (美国)** 和**西班牙语 (巴拿马)** 。 |
| \<DD >/<MM>/<YYYY> |       用于第一天的格式, 如**保加利亚语**和**荷兰语 (荷兰)** 。        |
| \<YYYY >/<MM>/<DD> |          用于年份优先格式, 如**瑞典语**和**法语 (加拿大)** 。          |

##### <a name="it"></a>/it

指定仅当 "运行身份" 用户 (运行任务的用户帐户) 登录到计算机时才运行任务。 此参数对以系统权限运行的任务不起作用。

默认情况下, 当计划任务或使用 **/u**参数指定的帐户时, "运行身份" 用户为本地计算机的当前用户 (如果使用)。 但是, 如果该命令包含 **/ru**参数, 则 "运行身份" 用户是由 **/ru**参数指定的帐户。

有关示例, 请参阅:
-   "如果我登录", 则计划70每隔一天运行一次的任务 "计划任务"。
-   **若要计划使用不同权限运行的任务,** 请在中的 "仅在特定用户登录时运行任务" 一节。

##### <a name="z"></a>/z

指定在任务完成计划后删除任务。

##### <a name="f"></a>/f

指定创建任务并在指定的任务已经存在时禁止显示警告。

##### <a name=""></a>/?

在命令提示符下显示帮助。

### <a name="BKMK_minutes"></a>计划每 N 分钟运行一次的任务

#### <a name="minute-schedule-syntax"></a>分钟计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在一分钟的计划中, 需要使用 **/sc minute**参数。 **/Mo** (修饰符) 参数是可选的, 它指定每次运行任务之间间隔的分钟数。 **/Mo**的默认值为 1 (每分钟)。 **/Et** (结束时间) 和 **/du** (duration) 参数是可选的, 可以与或不使用 **/k** (end task) 参数一起使用。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>计划每隔20分钟运行一次的任务

以下命令计划每隔20分钟运行一次安全脚本, 每秒运行一次。 该命令使用 **/sc**参数来指定分钟计划, 并使用 **/mo**参数指定20分钟的间隔。

由于该命令不包含开始日期或时间, 因此该任务将在该命令完成20分钟后开始运行, 每隔20分钟运行一次。 请注意, 安全脚本源文件位于远程计算机上, 但该任务已计划并在本地计算机上执行。
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>计划在非工作时间运行每100分钟一次的任务

下面的命令将计划在本地计算机上运行的安全脚本 (Sec), 每隔100分钟在 5:00 P.M. 运行一次。 到上午7:59 每天。 该命令使用 **/sc**参数来指定分钟计划, 并使用 **/mo**参数指定时间间隔100分钟。 它使用 **/st**和 **/et**参数指定每日计划的开始时间和结束时间。 如果脚本仍在凌晨7:59 运行, 它还会使用 **/k**参数停止该脚本。 如果没有 **/k**, 则**schtasks**将不会在 7:59 a.m. 之后启动脚本, 而是在上午6:20 启动的实例。 仍在运行, 它不会将其停止。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>计划每 N 小时运行一次的任务

#### <a name="hourly-schedule-syntax"></a>每小时计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

按小时计划, 需要 **/sc 每小时**参数。 **/Mo** (修饰符) 参数是可选的, 它指定了任务每次运行之间的小时数。 **/Mo**的默认值为 1 (每小时)。 **/K** (end task) 参数是可选的, 可以与 **/et** (在指定时间之后结束) 或 **/du** (在指定间隔之后结束) 一起使用。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>计划每5小时运行一次的任务

以下命令将 MyApp 程序计划为从 3 2002 月1日的第一天起每5小时运行一次。 它使用 **/mo**参数指定间隔, 使用 **/sd**参数指定开始日期。 由于该命令未指定开始时间, 当前时间将用作开始时间。

因为本地计算机设置为在 **"控制面板**" 的 "**区域和语言选项**" 中使用 "**英语 (津巴布韦)** " 选项, 所以开始日期的格式为 MM/DD/YYYY (03/01/2002)。
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>计划任务, 该任务每小时运行一次, 持续时间为5分钟

以下命令将 MyApp 程序计划为从午夜晚五分钟开始每小时运行一次。 由于省略了 **/mo**参数, 因此该命令使用每小时计划的默认值, 即每 (1) 小时。 如果此命令在 12:05 A.M. 之后运行, 则在第二天之前, 程序将不会运行。
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>计划每隔3小时运行一次的任务10小时

以下命令将 MyApp 程序计划为每隔3小时运行10小时。

该命令使用 **/sc**参数来指定每小时计划, 并使用 **/mo**参数指定3小时的间隔。 它使用 **/st**参数在午夜启动计划, 并使用 **/du**参数结束10小时后的重复周期。 由于程序只运行几分钟, 因此不需要使用 **/k**参数 (如果该程序在持续时间到期时仍在运行)。
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
在此示例中, 任务在凌晨12:00、3:00 A.M.、6:00 A.M 和凌晨9:00 运行。 由于持续时间为10小时, 因此该任务不会在下午12:00 再次运行。 相反, 它将在凌晨12:00 重新开始。 第二天。

### <a name="BKMK_days"></a>计划每 N 天运行的任务

#### <a name="daily-schedule-syntax"></a>每日计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每日计划中, **/sc 每日**参数是必需的。 **/Mo** (修饰符) 参数是可选的, 它指定每次运行该任务的间隔天数。 **/Mo**的默认值为 1 (每天)。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-day"></a>计划每日运行的任务

以下示例计划 MyApp 程序每天凌晨8:00 运行一次。 2002年12月31日之前。 由于它省略了 **/mo**参数, 因此默认间隔为 1, 用于每天运行命令。

在此示例中, 因为本地计算机系统在 **"控制面板**" 的 "**区域和语言选项**" 中设置为 "**英语 (英国)** " 选项, 所以结束日期的格式为 DD/MM/YYYY (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>计划每12天运行一次的任务

下面的示例将 MyApp 程序计划为每隔12天在下午1:00 运行。 (13:00) 从2002年12月31日开始。 该命令使用 **/mo**参数指定两 (2) 天的间隔, 并使用 **/sd**和 **/st**参数指定日期和时间。

在此示例中, 由于 "**控制面板**" 的 "**区域和语言选项**" 中的 "系统" 设置为 "**英语 (津巴布韦)** " 选项, 因此结束日期的格式为 MM/DD/YYYY (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>计划每隔70天运行的任务 (如果我登录)

以下命令计划每70天运行一次安全脚本 (Sec)。 该命令使用 **/mo**参数指定70天的间隔。 它还使用 **/it**参数来指定仅在其帐户运行任务的用户登录到计算机时任务才运行。 由于任务将以用户帐户的权限运行, 因此该任务仅在我登录时才会运行。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> 若要使用仅交互 ( **/it**) 属性标识任务, 请使用详细查询 **(/query/v**)。 在带有 **/it**的任务的详细查询显示中, "**登录模式**" 字段的值仅为 "**交互式**"。

### <a name="BKMK_weeks"></a>计划每 N 周运行的任务

#### <a name="weekly-schedule-syntax"></a>每周计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每周计划中, **/sc 每周**参数是必需的。 **/Mo** (修饰符) 参数是可选的, 它指定每次运行任务之间的周数。 **/Mo**的默认值为 1 (每周)。

每周计划还具有一个可选的 **/d**参数, 用于计划任务在一周的指定天或所有天运行 ( *)。默认值为 "周一 (星期一)"。每天 (* ) 选项相当于计划每日任务。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>计划每六周运行一次的任务

以下命令计划 MyApp 程序每六周在远程计算机上运行。 该命令使用 **/mo**参数来指定间隔。 因为该命令省略了 **/d**参数, 所以该任务会在星期一运行。

此命令还使用 **/s**参数来指定远程计算机, 并使用 **/u**参数来使用用户的管理员帐户的权限运行命令。 由于省略了 **/p**参数, 因此**schtasks.exe**会提示用户输入管理员帐户密码。

此外, 由于命令以远程方式运行, 因此命令中的所有路径 (包括 MyApp 的路径) 都指远程计算机上的路径。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>计划每隔一周在星期五运行的任务

以下命令将任务计划为每隔一个星期五运行一次。 它使用 **/mo**参数指定两周的时间间隔, 并使用 **/d**参数指定一周中的某一天。 若要计划每个星期五运行的任务, 请省略 **/mo**参数或将其设置为1。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>计划每 N 个月运行一次的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在此计划类型中, **/sc 月度**参数是必需的。 **/Mo** (修饰符) 参数, 指定每次运行任务之间间隔的月数, 此参数是可选的, 默认值为 1 (每月)。 此计划类型还具有一个可选的 **/d**参数, 用于计划在指定月份日期运行任务。 默认值为 1 (一个月的第一天)。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>计划在每月第一天运行的任务

以下命令将 MyApp 程序计划为在每月的第一天运行。 由于值1是 **/mo** (修饰符) 参数和 **/d** (day) 参数的默认值, 因此从命令中省略这些参数。
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>计划每三个月运行一次的任务

以下命令将 MyApp 程序计划为每三个月运行一次。 它使用 **/mo**参数来指定间隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>计划在每隔一个月21天的午夜运行的任务

以下命令将 MyApp 程序计划为在每隔一个月的第21天的午夜运行。 此命令指定此任务应运行一年, 从2002年7月2日到6月 30 2003 日。

该命令使用 **/mo**参数指定每月间隔 (每两个月), 指定日期指定为 **/d**参数, 并使用 **/st**指定时间。 它还使用 **/sd**和 **/ed**参数分别指定开始日期和结束日期。 由于 "**控制面板**" 的 "**区域和语言选项**" 中的 "本地计算机" 设置为 "**英语 (南非)** " 选项, 因此将以本地格式 (YYYY/MM/DD) 来指定日期。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>计划任务在一周的特定天运行

#### <a name="weekly-schedule-syntax"></a>每周计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"一周的某一日" 计划是每周计划的一种变化形式。 在每周计划中, **/sc 每周**参数是必需的。 **/Mo** (修饰符) 参数是可选的, 它指定每次运行任务之间的周数。 **/Mo**的默认值为 1 (每周)。 **/D**参数是可选的, 它将任务计划为在一周的指定日期运行, 或在所有日期 (\*) 运行。 默认值为 "周一 (星期一)"。 "每天" 选项 ( **/d \*** ) 等效于计划每日任务。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>计划每周三运行的任务

以下命令将 MyApp 程序计划为每周在星期三运行。 该命令使用 **/d**参数指定一周中的某一天。 因为该命令省略了 **/mo**参数, 所以任务每周运行一次。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>计划在星期一和星期五每八周运行一次的任务

下面的命令将任务计划为在每八周的星期一和星期五运行。 它使用 **/d**参数指定天, 使用 **/mo**参数指定八周间隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>计划任务在月份的特定周运行

#### <a name="specific-week-syntax"></a>特定周语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在此计划类型中, **/sc 月度**参数、 **/mo** (修饰符) 参数和 **/d** (day) 参数是必需的。 **/Mo** (修饰符) 参数指定运行任务的周。 **/D**参数指定一周中的第几天。 (对于此计划类型, 只能指定一周中的某一天。)此计划还具有一个可选 **/m** (month) 参数, 该参数使你可以计划特定月份或每个月<em>的任务 ()。 **/M</em>* 参数的默认值是每月 (* )。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>为每月的第二个星期日计划任务

以下命令将 MyApp 程序计划为在每月的第二个星期日运行。 它使用 **/mo**参数来指定每月的第二周, 并使用 **/d**参数来指定日期。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>为三月和九月的第一个星期一计划任务

以下命令计划 MyApp 程序在3月和9月的第一个星期一运行。 它使用 **/mo**参数指定月份的第一周, 并使用 **/d**参数来指定日期。 它使用 **/m**参数指定月份, 并用逗号分隔月份参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>计划每月特定日期运行的任务

#### <a name="specific-date-syntax"></a>特定日期语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在特定的日期计划类型中, **/sc 月度**参数和 **/d** (day) 参数是必需的。 **/D**参数指定月份的日期 (1-31), 而不是一周中的某一天。 您只能在计划中指定一天。 **/Mo** (修饰符) 参数对于此计划类型无效。

**/M** (month) 参数对于此计划类型是可选的, 默认值为每个月 (<em>)。 **Schtasks</em>* 不允许你计划一个任务, 使其不会出现在由 **/m**参数指定的月份中。 但是, 如果省略 **/m**参数, 并为每个月未显示的日期 (例如31天) 计划任务, 则该任务不会在较短的月份运行。 若要为该月的最后一天计划任务, 请使用 "最后一天" 计划类型。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>为每个月的第一天计划任务

以下命令将 MyApp 程序计划为在每月的第一天运行。 由于默认修饰符为 none (无修饰符), 因此默认日期为第1天, 默认月份为每月, 该命令不需要任何其他参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>计划任务在5月15日

以下命令将 MyApp 程序计划为在5月15日和6月15日下午3:00 运行。 (15:00)。 它使用 **/m**参数指定日期, 使用 **/m**参数指定月份。 它还使用 **/st**参数指定开始时间。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>计划任务在一个月的最后一天运行

#### <a name="last-day-syntax"></a>最后一天的语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在最后一天的计划类型中, **/sc 月度**参数、 **/mo LASTDAY** (修饰符) 参数和 **/m** (month) 参数是必需的。 **/D** (day) 参数无效。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>计划每月最后一天的任务

以下命令将 MyApp 程序计划为在每月的最后一天运行。 它使用 **/mo**参数指定最后一天, 使用通配符 (*) 指定 **/m**参数, 以指示该程序每月运行一次。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>在下午6:00 计划任务 在二月份和三月的最后一天

以下命令计划 MyApp 程序在二月份的最后一天运行, 在3月的最后一天的下午6:00 运行。 它使用 **/mo**参数指定上一天, 使用 **/m**参数指定月份, 使用 **/st**参数指定开始时间。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>计划运行一次的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在 "运行一次" 计划类型中, " **/sc 一次**" 参数是必需的。 **/St**参数 (指定任务运行的时间) 是必需的。 **/Sd**参数 (指定任务运行的日期) 是可选的。 **/Mo** (修饰符) 和 **/ed** (结束日期) 参数对于此计划类型无效。

如果指定的日期和时间基于本地计算机的时间, 则**Schtasks**不允许将任务计划为运行一次。 若要计划在不同时区的远程计算机上运行一次的任务, 您必须在本地计算机上的日期和时间之前进行计划。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-one-time"></a>计划运行一次的任务

以下命令计划 MyApp 程序在2003年1月1日午夜运行。 它使用 **/sc**参数来指定计划类型, 使用 **/sd**和**st**指定日期和时间。

因为本地计算机使用 "**控制面板**" 的 "**区域和语言选项**" 中的 "**英语 (美国)** " 选项, 所以开始日期的格式为 MM/DD/YYYY。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>计划每次系统启动时运行的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在 "按启动计划" 类型中, **/sc onstart**参数是必需的。 **/Sd** (开始日期) 参数是可选的, 默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>计划在系统启动时运行的任务

以下命令计划 MyApp 程序在系统每次启动时运行, 从2001年3月15日开始:

因为本地计算机使用 "**控制面板**" 的 "**区域和语言选项**" 中的 "**英语 (美国)** " 选项, 所以开始日期的格式为 MM/DD/YYYY。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>计划在用户登录时运行的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"登录时" 计划类型计划在任何用户登录到计算机时运行的任务。 在 "登录时" 计划类型中, **/sc onlogon**参数是必需的。 **/Sd** (开始日期) 参数是可选的, 默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>计划用户登录到远程计算机时运行的任务

以下命令计划每次用户 (任何用户) 登录到远程计算机时运行的批处理文件。 它使用 **/s**参数来指定远程计算机。 因为该命令是远程的, 所以该命令中的所有路径 (包括批处理文件的路径) 都是指远程计算机上的路径。
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>计划在系统空闲时运行的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"空闲时" 计划类型计划在 **/i**参数指定的时间内没有用户活动时运行的任务。 在 "空闲时" 计划类型中, **/sc onidle**参数和 **/i**参数是必需的。 **/Sd** (开始日期) 是可选的, 默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>计划每次计算机空闲时运行的任务

以下命令计划 MyApp 程序在计算机空闲时运行。 它使用所需的 **/i**参数指定计算机必须在任务开始之前十分钟处于空闲状态。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>计划立即运行的任务

**Schtasks**没有 "立即运行" 选项, 但你可以通过创建运行一次并在几分钟内启动的任务来模拟该选项。

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>计划从现在开始运行几分钟的任务。

下面的命令将任务计划为运行一次, 在2002年11月13日下午2:18 运行。 本地时间。

因为本地计算机使用 "**控制面板**" 的 "**区域和语言选项**" 中的 "**英语 (美国)** " 选项, 所以开始日期的格式为 MM/DD/YYYY。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>计划使用不同权限运行的任务

你可以将所有类型的任务计划为在本地计算机和远程计算机上以备用帐户的权限运行。 除了特定计划类型所需的参数外, **/ru**参数是必需的, 并且 **/rp**参数是可选的。

#### <a name="examples"></a>示例

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>在本地计算机上使用管理员权限运行任务

以下命令计划 MyApp 程序在本地计算机上运行。 它使用 **/ru**指定该任务应以用户的管理员帐户 (Admin06) 的权限运行。 在此示例中, 任务计划每周二运行一次, 但你可以将任何计划类型用于具有替代权限的任务运行。
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
在响应中, **schtasks.exe**会提示输入 Admin06 帐户的 "运行身份" 密码, 然后显示一条成功消息。
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>在远程计算机上使用替代权限运行任务

以下命令计划 MyApp 程序每四天在市场营销计算机上运行。

该命令使用 **/sc**参数来指定每日计划, 并使用 **/mo**参数指定四天的间隔。

该命令使用 **/s**参数提供远程计算机的名称, 使用 **/u**参数来指定有权在远程计算机上计划任务的帐户 (营销计算机上的 Admin01)。 它还使用 **/ru**参数来指定任务应以用户的非管理员帐户 (Reskits 域中的 User01) 的权限运行。 如果没有 **/ru**参数, 任务将使用 **/u**所指定帐户的权限运行。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks**首先请求由 **/u**参数命名的用户的密码 (运行该命令), 然后请求由 **/ru**参数命名的用户的密码 (以运行该任务)。 对密码进行身份验证后, **schtasks**会显示一条消息, 指示已计划任务。
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>仅在特定用户登录时才运行任务

以下命令计划 AdminCheck 程序在每星期五凌晨4:00 的公共计算机上运行, 但前提是计算机的管理员登录。

该命令使用 **/sc**参数来指定每周计划, 使用 **/d**参数指定 day, 使用 **/st**参数指定开始时间。

该命令使用 **/s**参数提供远程计算机的名称, 使用 **/u**参数来指定有权在远程计算机上计划任务的帐户。 它还使用 **/ru**参数将任务配置为使用公共计算机的管理员的权限 (Public\Admin01) 和 **/it**参数来运行, 以指示仅当 Public\Admin01 帐户登录时任务才运行。
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**注意**
-   若要使用仅交互 ( **/it**) 属性标识任务, 请使用详细查询 **(/query/v**)。 在带有 **/it**的任务的详细查询显示中, "**登录模式**" 字段的值仅为 "**交互式**"。

### <a name="BKMK_sys_perms"></a>计划以系统权限运行的任务

所有类型的任务都可以使用系统帐户在本地和远程计算机上的权限运行。 除特定计划类型所需的参数外, **/ru 系统**(或 **/ru ""** ) 参数是必需的, 并且 **/rp**参数无效。

**重要须知**
-   系统帐户没有交互式登录权限。 用户无法通过系统权限查看或与程序或任务交互。
-   **/Ru**参数确定运行任务的权限, 而不是用于计划任务的权限。 无论 **/ru**参数的值如何, 只有管理员才能计划任务。

**注意**

若要标识以系统权限运行的任务, 请使用详细查询 ( **/query** **/v**)。 在系统运行任务的详细查询显示中, "**以用户身份运行**" 字段的值为 " **NT AUTHORITY\SYSTEM** ", "**登录模式**" 字段的值**仅**为 "背景"。

#### <a name="examples"></a>示例

#### <a name="to-run-a-task-with-system-permissions"></a>使用系统权限运行任务

以下命令计划 MyApp 程序在本地计算机上以系统帐户的权限运行。 在此示例中, 任务计划在每月的第15天运行, 但你可以将任何计划类型用于具有系统权限的任务运行。

该命令使用 **/Ru 系统**参数指定系统安全上下文。 因为系统任务不使用密码, 所以省略了 **/rp**参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
在响应中, **schtasks.exe**显示信息性消息和成功消息。 它不会提示输入密码。
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>在远程计算机上运行具有系统权限的任务

以下命令将 MyApp 程序计划为在每早上凌晨4:00 的 Finance01 计算机上运行。 具有系统权限。

该命令使用 **/tn**参数命名任务, 使用 **/Tr**参数指定 MyApp 程序的远程副本。 它使用 **/sc**参数指定每日计划, 但省略 **/mo**参数, 因为 1 (每天) 是默认值。 它使用 **/st**参数指定开始时间, 这也是任务每天运行的时间。

该命令使用 **/s**参数提供远程计算机的名称, 使用 **/u**参数来指定有权在远程计算机上计划任务的帐户。 它还使用 **/ru**参数来指定任务应在系统帐户下运行。 如果没有 **/ru**参数, 任务将使用 **/u**所指定帐户的权限运行。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks**请求由 **/u**参数命名的用户的密码, 并且在对密码进行身份验证后, 将显示一条消息, 指示已创建该任务, 并且它将以系统帐户的权限运行。
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>计划运行多个程序的任务

每个任务仅运行一个程序。 但是, 您可以创建运行多个程序的批处理文件, 然后计划运行该批处理文件的任务。 下面的过程演示了此方法:
1. 创建一个批处理文件, 用于启动要运行的程序。

   在此示例中, 将创建一个批处理文件, 该文件启动事件查看器 (Eventvwr.msc) 和系统监视器 (Perfmon)。  
   - 打开文本编辑器，例如记事本。
   - 键入每个程序的可执行文件的名称和完全限定路径。 在这种情况下, 该文件包含以下语句。  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - 将该文件另存为 MyApps。
2. 使用**schtasks.exe**创建运行 MyApps 的任务。

   以下命令将创建监视任务, 该任务将在每次登录时运行。 它使用 **/tn**参数命名任务, 使用 **/Tr**参数运行 MyApps。 它使用 **/sc**参数来指示 OnLogon 计划类型, 并使用 **/ru**参数来使用用户的管理员帐户的权限运行该任务。  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   此命令的结果是, 每当用户登录到计算机时, 此任务都会启动事件查看器和系统监视器。

### <a name="BKMK_remote"></a>计划在远程计算机上运行的任务

若要计划在远程计算机上运行的任务, 必须将任务添加到远程计算机的计划中。 可以在远程计算机上计划所有类型的任务, 但必须满足以下条件。
-   您必须具有计划任务的权限。 因此, 你必须使用作为远程计算机上 Administrators 组的成员的帐户登录到本地计算机, 或者必须使用 **/u**参数来提供远程计算机的管理员的凭据。
-   仅当本地计算机和远程计算机位于同一个域中, 或者本地计算机位于远程计算机域信任的域中时, 才可以使用 **/u**参数。 否则, 远程计算机将无法对指定的用户帐户进行身份验证, 并且无法验证该帐户是否为 Administrators 组的成员。
-   该任务必须具有足够的权限, 才能在远程计算机上运行。 所需权限因任务而异。 默认情况下, 任务使用本地计算机的当前用户的权限运行, 或者, 如果使用 **/u**参数, 则该任务将使用 **/u**参数指定的帐户的权限运行。 但是, 可以使用 **/ru**参数, 以使用其他用户帐户或系统权限来运行任务。

#### <a name="examples"></a>示例

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>管理员计划远程计算机上的任务

以下命令计划 MyApp 程序在 SRV01 远程计算机上每隔10天立即运行一次。 该命令使用 **/s**参数提供远程计算机的名称。 由于本地当前用户是远程计算机的管理员, 因此无需使用 **/u**参数 (为计划任务提供备用权限)。

请注意, 在远程计算机上计划任务时, 所有参数都是指远程计算机。 因此, **/tr**参数指定的可执行文件是指远程计算机上 MyApp 的副本。
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
作为响应, " **schtasks** " 会显示一条成功消息, 指示已计划任务。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>用户计划远程计算机上的命令 (案例 1)

以下命令计划 MyApp 程序每隔三小时在 SR-SRV06 远程计算机上运行。 由于需要管理员权限来计划任务, 因此该命令使用 **/u**和 **/p**参数提供用户的管理员帐户 (Reskits 域中的 Admin01) 的凭据。 默认情况下, 这些权限也用于运行任务。 但是, 因为该任务不需要管理员权限来运行, 所以该命令包括 **/u**和 **/rp**参数以替代默认值, 并以用户在远程计算机上的非管理员帐户的权限运行该任务。
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
作为响应, " **schtasks** " 会显示一条成功消息, 指示已计划任务。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>用户计划远程计算机上的命令 (案例 2)

以下命令将 MyApp 程序计划为在每月最后一天的 SRV02 远程计算机上运行。 由于本地当前用户 (user03) 不是远程计算机的管理员, 因此该命令使用 **/u**参数提供用户的管理员帐户 (Admin01 在 Reskits 域中) 的凭据。 管理员帐户权限将用于计划任务和运行任务。
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
由于该命令不包含 **/p** (password) 参数, 因此**schtasks**会提示输入密码。 然后, 将显示一条成功消息, 在本例中为警告。
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
此警告表示远程域无法对 **/u**参数指定的帐户进行身份验证。 在这种情况下, 远程域无法对用户帐户进行身份验证, 因为本地计算机不是远程计算机域所信任的域的成员。 出现这种情况时, 任务作业将显示在计划任务的列表中, 但该任务实际上是空的, 将不会运行。

来自详细查询的以下显示公开了该任务的问题。 在显示中, 请注意 "**下一次运行时间**"的值不是, 并且**无法从任务计划程序数据库中检索**"**以用户身份运行**" 的值。

如果此计算机是同一域或受信任域的成员, 则该任务将成功计划并按指定运行。
```
HostName: SRV44
TaskName: My App
Next Run Time: Never
Status:
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user03
Schedule: At 3:52 PM on day 31 of every month, start
 starting 12/14/2001
Task To Run: c:\program files\corpapps\myapp.exe
Start In: myapp.exe
Comment: N/A
Scheduled Task State: Disabled
Scheduled Type: Monthly
Start Time: 3:52:00 PM
Start Date: 12/14/2001
End Date: N/A
Days: 31
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
V,DEC
Run As User: Could not be retrieved from the task sched
uler database
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Every: Disabled
Repeat: Until: Time: Disabled
Repeat: Until: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle Time: Disabled
Power Management: Disabled
```

#### <a name="remarks"></a>备注

-   若要使用其他用户的权限运行 **/create**命令, 请使用 **/u**参数。 **/U**参数仅对远程计算机上的任务计划有效。
-   若要查看更多**schtasks/create**示例, 请键入**schtasks/create/？** 。
-   若要计划使用其他用户的权限运行的任务, 请使用 **/ru**参数。 **/Ru**参数对于本地和远程计算机上的任务都有效。
-   若要使用 **/u**参数, 本地计算机必须与远程计算机位于同一域中, 或者必须位于远程计算机域信任的域中。 否则, 要么不创建任务, 要么任务作业为空, 并且任务不会运行。
-   如果你不提供密码, 则**Schtasks**始终提示输入密码, 即使你使用当前用户帐户在本地计算机上计划任务也是如此。 这是适用于**schtasks**的正常行为。
-   **Schtasks**不验证程序文件位置或用户帐户密码。 如果没有为用户帐户输入正确的文件位置或正确的密码, 则将创建该任务, 但该任务不会运行。 此外, 如果某个帐户的密码更改或过期, 而你不更改任务中保存的密码, 则该任务不会运行。
-   系统帐户没有交互式登录权限。 用户看不到与系统权限一起运行的程序。
-   每个任务仅运行一个程序。 不过, 您可以创建一个批处理文件来启动多个任务, 然后计划一个运行该批处理文件的任务。
-   您可以在创建任务后立即对其进行测试。 使用**运行**操作来测试任务, 然后检查 SchedLgU 文件 (*SystemRoot*\SchedLgU.txt) 是否存在错误。

## <a name="BKMK_change"></a>schtasks 更改

更改任务的以下一个或多个属性。
-   任务运行的程序 ( **/tr**)。
-   用于运行任务的用户帐户 ( **/ru**)。
-   用户帐户 ( **/rp**) 的密码。
-   将仅交互属性添加到任务 ( **/it**)。

### <a name="syntax"></a>语法

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parameters

|          术语           |                                                                                                                                                                                                                                                                                                                                     定义                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /tn \<TaskName >     |                                                                                                                                                                                                                                                                                                               标识要更改的任务。 输入任务名称。                                                                                                                                                                                                                                                                                                               |
|     /s \<计算机 >      |                                                                                                                                                                                                                                                                               指定远程计算机的名称或 IP 地址 (带有或不带反斜杠)。 默认值为本地计算机。                                                                                                                                                                                                                                                                               |
|  /u [\<域 >\]<User>  |                                                                                                                                                                 用指定用户帐户的权限运行此命令。 默认值是本地计算机当前用户的权限。 指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**和 **/p**参数仅对在远程计算机上更改任务有效 ( **/s**)。                                                                                                                                                                  |
|     /p \<密码 >      |                                                                                                                                                                                              指定在 **/u**参数中指定的用户帐户的密码。 如果使用 **/u**参数, 但省略 **/p**参数或 password 参数, 则**schtasks**会提示输入密码。</br>**/U**和 **/p**参数仅在使用 **/s**时有效。                                                                                                                                                                                               |
| /ru {[\<域 >\]<User> |                                                                                                                                                                                                                                                                                                                                       主板                                                                                                                                                                                                                                                                                                                                       |
|     /rp \<密码 >     |                                                                                                                                                                                                                                                 为现有用户帐户或 **/ru**参数指定的用户帐户指定新密码。 使用本地系统帐户时, 将忽略此参数。                                                                                                                                                                                                                                                  |
|     /tr \<k >      |                                                                                                                                                                                  更改任务运行的程序。 输入可执行文件、脚本文件或批处理文件的完全限定路径和文件名。 如果省略路径, 则**schtasks**会假定文件位于\<systemroot > \System32 目录中。 指定的程序将替换任务运行的原始程序。                                                                                                                                                                                  |
|    /st \<Starttime >     |                                                                                                                                                                                                                                                              使用24小时时间格式 HH: mm 指定任务的开始时间。 例如, 14:30 的值相当于 2:30 PM 的12小时制。                                                                                                                                                                                                                                                               |
|     /ri \<间隔 >     |                                                                                                                                                                                                                                                                           指定计划任务的重复间隔, 以分钟为单位。 有效范围是 1-599940 (599940 分钟 = 9999 小时)。                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               使用24小时时间格式 HH: mm 指定任务的结束时间。 例如, 14:30 的值相当于 2:30 PM 的12小时制。                                                                                                                                                                                                                                                                |
|     /du \<Duration >     |                                                                                                                                                                                                                                                                                                     指定关闭\<EndTime <Duration>> 的任务, 如果指定, 则关闭。                                                                                                                                                                                                                                                                                                      |
|           遇到            |                                                                                                                                                                   停止任务在 **/et**或 **/du**指定的时间运行的程序。 如果不使用 **/k**, 则**schtasks**在到达由 **/et**或 **/du**指定的时间后不会重新启动该程序, 但如果程序仍在运行, 则它不会停止它。 此参数是可选的, 仅在每分钟或每小时计划时有效。                                                                                                                                                                   |
|    /sd \<开始 >     |                                                                                                                                                                                                                                                                                              指定任务应在其上运行的第一个日期。 日期格式为 MM/DD/YYYY。                                                                                                                                                                                                                                                                                               |
|     /ed \<终止 >      |                                                                                                                                                                                                                                                                                                 指定任务应在其上运行的最后日期。 格式为 MM/DD/YYYY。                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       指定启用计划任务。                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      指定禁用计划任务。                                                                                                                                                                                                                                                                                                                       |
|           /it           | 指定仅当 "运行身份" 用户 (运行任务的用户帐户) 登录到计算机时才运行计划的任务。</br>此参数不会影响使用系统权限运行的任务, 也不会影响已设置交互的属性的任务。 不能使用 change 命令从任务中删除仅交互属性。</br>默认情况下, 当计划任务或使用 **/u**参数指定的帐户时, "运行身份" 用户为本地计算机的当前用户 (如果使用)。 但是, 如果该命令包含 **/ru**参数, 则 "运行身份" 用户是由 **/ru**参数指定的帐户。 |
|           /z            |                                                                                                                                                                                                                                                                                                          指定在其计划完成后删除任务。                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>备注

-   **/Tn**和 **/s**参数用于识别任务。 **/Tr**、 **/ru**和 **/rp**参数指定可以更改的任务的属性。
-   **/Ru**和 **/rp**参数指定运行任务所用的权限。 **/U**和 **/p**参数指定用于更改任务的权限。
-   若要更改远程计算机上的任务, 用户必须登录到本地计算机, 该帐户是远程计算机上 Administrators 组的成员。
-   若要运行具有不同用户 ( **/u**, **/p**) 权限的 **/change**命令, 本地计算机必须与远程计算机位于同一域中, 或者必须位于远程计算机域信任的域中。
-   系统帐户没有交互式登录权限。 用户看不到与系统权限一起运行的程序。
-   若要使用 **/it**属性标识任务, 请使用详细查询 ( **/query/v**)。 在带有 **/it**的任务的详细查询显示中, "**登录模式**" 字段的值仅为 "**交互式**"。

### <a name="examples"></a>示例

### <a name="to-change-the-program-that-a-task-runs"></a>更改任务运行的程序

以下命令将病毒检查任务运行的程序从 VirusCheck 更改为 VirusCheck2。 此命令使用 **/tn**参数来标识任务, 使用 **/tr**参数为任务指定新程序。 (不能更改任务名称。)
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
在响应中, **schtasks.exe**显示以下成功消息:
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
此命令的结果是, 病毒检查任务现在运行 VirusCheck2。

### <a name="to-change-the-password-for-a-remote-task"></a>更改远程任务的密码

以下命令更改远程计算机 Svr01 上的 RemindMe 任务的用户帐户密码。 该命令使用 **/tn**参数来标识该任务, 使用 **/s**参数来指定远程计算机。 它使用 **/rp**参数指定新密码p@ssWord3。

只要用户帐户的密码过期或更改, 就需要执行此过程。 如果任务中保存的密码不再有效, 则任务不会运行。
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
在响应中, **schtasks.exe**显示以下成功消息:
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
此命令的结果是, RemindMe 任务现在在其原始用户帐户下运行, 但使用新密码。

### <a name="to-change-the-program-and-user-account-for-a-task"></a>更改任务的程序和用户帐户

下面的命令将更改任务运行的程序, 并更改运行任务的用户帐户。 实质上, 它对新任务使用旧计划。 此命令更改 ChkNews 任务, 该任务将在早上凌晨9:00 启动 Notepad.exe, 以改为启动 Internet Explorer。

该命令使用 **/tn**参数来标识该任务。 它使用 **/tr**参数更改任务运行的程序, 使用 **/ru**参数更改运行任务的用户帐户。

省略了用于提供用户帐户密码的 **/ru**和 **/rp**参数。 您必须提供帐户的密码, 但您可以使用 **/ru**和 **/rp**参数并以明文形式键入密码, 或者等待**schtasks.exe**提示您输入密码, 然后在 "模糊文本" 中输入密码。
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
在响应中, **schtasks.exe**请求用户帐户的密码。 它会遮盖您键入的文本, 因此密码不可见。
```
Please enter the password for DomainX\Admin01: 
```
请注意, **/tn**参数会标识任务, 而 **/tr**和 **/ru**参数更改任务的属性。 不能使用其他参数来识别任务, 也不能更改任务名称。

在响应中, **schtasks.exe**显示以下成功消息:
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
此命令的结果是, ChkNews 任务现在使用管理员帐户的权限运行 Internet Explorer。

### <a name="to-change-a-program-to-the-system-account"></a>将程序更改为系统帐户

以下命令将更改 SecurityScript 任务, 使其以系统帐户的权限运行。 它使用 **/ru ""** 参数来指示系统帐户。
```
schtasks /change /tn SecurityScript /ru ""
```
在响应中, **schtasks.exe**显示以下成功消息:
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
由于使用系统帐户权限运行的任务不需要密码, 因此**schtasks.exe**不会提示输入密码。

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>仅在登录时运行程序

以下命令将仅交互属性添加到 MyApp, 即现有任务。 此属性可确保任务仅在 "运行身份" 用户 (即运行任务的用户帐户) 登录到计算机时运行。

该命令使用 **/tn**参数来标识该任务, 并使用 **/it**参数将仅交互属性添加到任务。 由于任务已经用用户帐户的权限运行, 因此我不需要更改任务的 **/ru**参数。
```
schtasks /change /tn MyApp /it
```
在响应中, **schtasks.exe**显示以下成功消息。
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>schtasks 运行

立即启动计划任务。 **运行**操作将忽略计划, 但使用任务中保存的程序文件位置、用户帐户和密码来立即运行任务。

### <a name="syntax"></a>语法

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|         术语          |                                                                                                                                                                 定义                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName >    |                                                                                                                                                       必需。 标识任务。                                                                                                                                                        |
|    /s \<计算机 >     |                                                                                                           指定远程计算机的名称或 IP 地址 (带有或不带反斜杠)。 默认值为本地计算机。                                                                                                           |
| /u [\<域 >\]<User> | 用指定用户帐户的权限运行此命令。 默认情况下, 使用本地计算机当前用户的权限运行命令。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**和 **/p**参数仅在使用 **/s**时有效。 |
|    /p \<密码 >     |                          指定在 **/u**参数中指定的用户帐户的密码。 如果使用 **/u**参数, 但省略 **/p**参数或 password 参数, 则**schtasks**会提示输入密码。</br>**/U**和 **/p**参数仅在使用 **/s**时有效。                           |
|          /?           |                                                                                                                                                    在命令提示符下显示帮助。                                                                                                                                                     |

### <a name="remarks"></a>备注

-   使用此操作来测试您的任务。 如果任务未运行, 请检查任务计划程序服务事务日志、 \<系统根目录 > \SchedLgU.txt, 以了解错误。
-   运行任务不会影响任务计划, 也不会更改为任务计划的下一个运行时间。
-   若要远程运行任务, 则必须在远程计算机上计划任务。 运行该任务时, 任务仅在远程计算机上运行。 若要验证某一任务是否正在远程计算机上运行, 请使用任务管理器或任务计划程序的\<事务日志, Systemroot > \SchedLgU.txt。

### <a name="examples"></a>示例

### <a name="to-run-a-task-on-the-local-computer"></a>在本地计算机上运行任务

下面的命令启动 "安全脚本" 任务。
```
schtasks /run /tn "Security Script"
```
作为响应, **schtasks.exe**启动与任务相关联的脚本并显示以下消息:
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
正如消息中所示, **schtasks**尝试启动程序, 但它不能完全启动程序。

### <a name="to-run-a-task-on-a-remote-computer"></a>在远程计算机上运行任务

以下命令在远程计算机上启动更新任务: Svr01:
```
schtasks /run /tn Update /s Svr01
```
在这种情况下, **schtasks.exe**显示以下错误消息:
```
ERROR: Unable to run the scheduled task "Update".
```
若要找出错误的原因, 请查看 Svr01 上的计划任务事务日志 C:\Windows\SchedLgU.txt。 在这种情况下, 日志中会出现以下条目:
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
显然, 该任务中的用户名或密码在系统上无效。 以下**schtasks/change**命令在 Svr01 上更新更新任务的用户名和密码:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
**更改**命令完成后, 将重复**运行**命令。 此时, update.exe 程序启动, 而**schtasks.exe**将显示以下消息:
```
SUCCESS: Attempted to run the scheduled task "Update".
```
正如消息中所示, **schtasks**尝试启动程序, 但它不能完全启动程序。

## <a name="BKMK_end"></a>schtasks 结束

停止任务启动的程序。

### <a name="syntax"></a>语法

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|         术语          |                                                                                                                                                               定义                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName >    |                                                                                                                                         必需。 标识启动程序的任务。                                                                                                                                         |
|    /s \<计算机 >     |                                                                                                                        指定远程计算机的名称或 IP 地址。 默认值为本地计算机。                                                                                                                        |
| /u [\<域 >\]<User> | 用指定用户帐户的权限运行此命令。 默认情况下, 使用本地计算机当前用户的权限运行命令。 指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**和 **/p**参数仅在使用 **/s**时有效。 |
|    /p \<密码 >     |                        指定在 **/u**参数中指定的用户帐户的密码。 如果使用 **/u**参数, 但省略 **/p**参数或 password 参数, 则**schtasks**会提示输入密码。</br>**/U**和 **/p**参数仅在使用 **/s**时有效。                         |
|          /?           |                                                                                                                                                             显示帮助。                                                                                                                                                              |

### <a name="remarks"></a>备注

**Schtasks.exe**仅结束由计划任务启动的程序的实例。 若要停止其他进程, 请使用 TaskKill。 有关详细信息, 请参阅[Taskkill](taskkill.md)。

### <a name="examples"></a>示例

### <a name="to-end-a-task-on-a-local-computer"></a>在本地计算机上结束任务

以下命令将停止由 My Notepad 任务启动的 Notepad.exe 实例:
```
schtasks /end /tn "My Notepad"
```
在响应中, **schtasks.exe**停止任务启动的 notepad.exe 实例, 并显示以下成功消息:
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>结束远程计算机上的任务

以下命令停止远程计算机上的 InternetOn 任务启动的 Internet Explorer 实例, Svr01:
```
schtasks /end /tn InternetOn /s Svr01
```
在响应中, **schtasks.exe**停止任务启动的 Internet Explorer 实例, 并显示以下成功消息:
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks 删除

删除计划任务。

### <a name="syntax"></a>语法

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|         术语          |                                                                                                                                                                 定义                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /tn {\<TaskName >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  禁止显示确认消息。 删除任务但不发出警告。                                                                                                                                  |
|    /s \<计算机 >     |                                                                                                           指定远程计算机的名称或 IP 地址 (带有或不带反斜杠)。 默认值为本地计算机。                                                                                                           |
| /u [\<域 >\]<User> | 用指定用户帐户的权限运行此命令。 默认情况下, 使用本地计算机当前用户的权限运行命令。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**和 **/p**参数仅在使用 **/s**时有效。 |
|    /p \<密码 >     |                          指定在 **/u**参数中指定的用户帐户的密码。 如果使用 **/u**参数, 但省略 **/p**参数或 password 参数, 则**schtasks**会提示输入密码。</br>**/U**和 **/p**参数仅在使用 **/s**时有效。                           |
|          /?           |                                                                                                                                                    在命令提示符下显示帮助。                                                                                                                                                     |

### <a name="remarks"></a>备注

- **删除**操作从计划中删除任务。 它不会删除任务运行的程序, 也不会中断正在运行的程序。
- **Delete\\** * 命令将删除所有为计算机计划的任务, 而不只是由当前用户计划的任务。

### <a name="examples"></a>示例

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>从远程计算机计划中删除任务

以下命令将从远程计算机的计划中删除 "开始邮件" 任务。 它使用 **/s**参数来识别远程计算机。
```
schtasks /delete /tn "Start Mail" /s Svr16
```
在响应中, **schtasks.exe**显示以下确认消息。 若要删除任务, 请按 Y<strong>。</strong>若要取消命令, 请键入**n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>删除为本地计算机计划的所有任务

以下命令将从本地计算机的计划中删除所有任务, 包括其他用户计划的任务。 它使用 **/tn \\** * 参数来表示计算机上的所有任务, 使用 **/f**参数来禁止显示确认消息。
```
schtasks /delete /tn * /f
```
在响应中, **schtasks.exe**显示以下成功消息, 指示已计划的唯一任务 SecureScript。

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>schtasks 查询

显示计划在计算机上运行的任务。

### <a name="syntax"></a>语法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|         术语          |                                                                                                                                                                 定义                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /query        |                                                                                                                        操作名称是可选的。 在不使用任何参数的情况下键入**schtasks**会执行查询。                                                                                                                         |
|      /fo {TABLE       |                                                                                                                                                                    成员列表                                                                                                                                                                     |
|          /nh          |                                                                                                            省略表显示的列标题。 此参数对**表**和**CSV**输出格式有效。                                                                                                             |
|          /v           |                                                                                                         向显示添加任务的高级属性。</br>使用 **/v**的查询应该设置为**LIST**或**CSV**格式。                                                                                                          |
|    /s \<计算机 >     |                                                                                                           指定远程计算机的名称或 IP 地址 (带有或不带反斜杠)。 默认值为本地计算机。                                                                                                           |
| /u [\<域 >\]<User> | 用指定用户帐户的权限运行此命令。 默认情况下, 使用本地计算机当前用户的权限运行命令。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**和 **/p**参数仅在使用 **/s**时有效。 |
|    /p \<密码 >     |                                        指定在 **/u**参数中指定的用户帐户的密码。 如果使用 **/u**, 但省略 **/p**或 password 参数, 则**schtasks**会提示输入密码。</br>**/U**和 **/p**参数仅在使用 **/s**时有效。                                         |
|          /?           |                                                                                                                                                    在命令提示符下显示帮助。                                                                                                                                                     |

### <a name="remarks"></a>备注

**Schtasks.exe**仅结束由计划任务启动的程序的实例。 若要停止其他进程, 请使用 TaskKill。 有关详细信息, 请参阅[Taskkill](taskkill.md)。

### <a name="examples"></a>示例

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>显示本地计算机上的计划任务

以下命令显示为本地计算机计划的所有任务。 这些命令生成相同的结果, 可以互换使用。
```
schtasks
schtasks /query
```
作为响应, **schtasks.exe**以默认的简单表格式显示任务, 如下表所示:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>显示计划任务的高级属性

以下命令请求详细显示本地计算机上的任务。 它使用 **/v**参数来请求详细 (详细) 显示, 并使用 **/fo list**参数将显示格式设置为列表以方便阅读。 您可以使用此命令来验证您创建的任务是否具有预期的定期模式。

**schtasks/query/fo LIST/v**

作为响应, **schtasks.exe**显示所有任务的详细属性列表。 以下显示的任务列表显示计划在凌晨4:00 运行的任务。 在每个月的最后一个星期五:
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>记录为远程计算机计划的任务

以下命令请求为远程计算机计划的任务列表, 并将任务添加到本地计算机上以逗号分隔的日志文件中。 可以使用此命令格式收集和跟踪为多台计算机计划的任务。

该命令使用 **/s**参数来识别远程计算机 Reskit16, 使用 **/fo**参数指定格式, 使用 **/nh**参数来禁止显示列标题。 **>>** 追加符号会将输出重定向到本地计算机 Svr01 上的 p0102 任务日志。 由于此命令在远程计算机上运行, 因此本地计算机的路径必须是完全限定的。
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
作为响应, **schtasks.exe**将为 Reskit16 计算机计划的任务添加到本地计算机 Svr01 上的 p0102 文件中。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

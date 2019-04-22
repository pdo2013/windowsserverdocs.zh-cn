---
title: schtasks
description: 'Windows 命令主题 * * *- '
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
ms.openlocfilehash: 7aed2b88823c117a7424a4d90edd86829a53f786
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822878"
---
# <a name="schtasks"></a>schtasks



计划定期或在特定时间运行的命令和程序。 添加和从计划中删除任务、 启动和停止按需、 任务和显示和更改计划的任务。

若要查看命令语法，请单击以下命令之一：
-   [schtasks 创建](#BKMK_create)
-   [更改计划任务](#BKMK_change)
-   [schtasks 运行](#BKMK_run)
-   [schtasks 结束](#BKMK_end)
-   [schtasks delete](#BKMK_delete)
-   [schtasks 查询](#BKMK_query)

## <a name="remarks"></a>备注

-   **SchTasks.exe**执行相同的操作**计划任务**中**控制面板**。 结合使用和互换使用，可以使用这些工具。
-   **Schtasks**取代**At.exe**，以前版本的 Windows 中包含的工具。 尽管**At.exe**仍包含在 Windows Server 2003 家族**schtasks**是建议的命令行任务计划工具。
-   中的参数**schtasks**命令可以按任何顺序出现。 键入**schtasks**没有任何参数的方式执行查询。
-   权限**schtasks**  
    -   您必须有权运行该命令。 任何用户可以计划在本地计算机上的任务和他们可以查看和更改这些计划的任务。 Administrators 组的成员可以计划、 查看和更改本地计算机上的所有任务。
    -   若要计划、 查看或更改远程计算机上的任务，您必须在远程计算机上 Administrators 组的成员，或者必须使用 **/u**参数提供的远程计算机的管理员凭据。
    -   可以使用 **/u**中的参数 **/ 创建**或 **/change**操作仅在本地和远程计算机位于相同域或本地计算机是域中时，远程计算机域信任。 否则为远程计算机不能对指定的用户帐户进行身份验证，它无法验证该帐户是 Administrators 组的成员。
    -   该任务必须有权运行。 所需的权限因该任务。 默认情况下，任务运行的本地计算机的当前用户的权限时还是使用指定的用户权限 **/u**参数，如果包含。 若要使用不同的用户帐户的权限时还是使用系统权限运行的任务，请使用 **/ru**参数。
-   若要验证是否计划的任务运行，或若要找出为什么计划的任务未运行，请参阅任务计划程序服务事务日志中， *SystemRoot*\SchedLgU.txt。 此日志记录： 尝试启动使用该服务的所有工具运行包括**计划任务**并**SchTasks.exe**。
-   在少数情况下，任务文件已损坏。 损坏的任务不会运行。 当你尝试对已损坏的任务执行操作时**SchTasks.exe**显示以下错误消息：  
    ```
    ERROR: The data is invalid.
    ```  
    不能恢复已损坏的任务。 若要还原的任务计划系统功能，请使用**SchTasks.exe**或**计划任务**从系统中删除任务并重新计划这些。

## <a name="BKMK_create"></a>schtasks 创建

安排的任务。

**Schtasks**为每个计划类型使用不同的参数组合。 若要查看用于创建任务的组合的语法或若要查看使用特定的计划类型创建一项任务的语法，请单击下列选项之一。
-   [组合的语法和参数说明](#BKMK_syntax)
-   [若要计划的任务，每个 N 分钟运行一次](#BKMK_minutes)
-   [若要计划的任务，每个 N 小时运行一次](#BKMK_hours)
-   [若要计划的任务，每个 N 天运行一次](#BKMK_days)
-   [若要计划每 N 周运行的任务](#BKMK_weeks)
-   [若要计划的任务，每个 N 月运行一次](#BKMK_months)
-   [若要计划在特定日期是星期几运行的任务](#BKMK_spec_day)
-   [若要计划在个月的特定一周运行的任务](#BKMK_spec_week)
-   [若要计划在特定日期每月运行的任务](#BKMK_spec_date)
-   [若要计划的任务，在上一个月的最后一天运行](#BKMK_last_day)
-   [若要计划的任务，运行一次](#BKMK_once)
-   [若要计划每次系统启动时运行的任务](#BKMK_startup)
-   [若要计划的任务，在运行时用户登录](#BKMK_logon)
-   [若要计划会在系统空闲时运行的任务](#BKMK_idle)
-   [若要计划的任务，现在可以运行](#BKMK_now)
-   [若要计划使用不同的权限运行的任务](#BKMK_diff_perms)
-   [若要计划使用系统的权限运行的任务](#BKMK_sys_perms)
-   [若要计划的任务，运行多个程序](#BKMK_multi_progs)
-   [若要计划在远程计算机运行的任务](#BKMK_remote)

### <a name="BKMK_syntax"></a>组合的语法和参数说明

#### <a name="syntax"></a>语法

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Parameters

##### <a name="sc-scheduletype"></a>/sc \<ScheduleType>

指定的计划类型。 有效值为分钟、 小时、 每天、 每周、 每月一次、 ONSTART、 登录时、 ONIDLE。

|计划类型|描述|
|-------------|-----------|
|分钟，每小时、 每天、 每周、 每月|指定计划的时间单位。|
|一次|该任务在指定的日期和时间运行一次。|
|ONSTART|每次系统启动时运行任务。 您可以指定的开始日期、 或在下次系统启动的时运行任务。|
|登录时|在任务运行时在用户 （任何用户） 登录。 可以指定一个日期，或下次用户登录的时运行任务。|
|ONIDLE|在任务运行时系统指定的时间段内处于空闲状态。 可以指定一个日期，或系统处于空闲状态下一次运行该任务。|

##### <a name="tn-taskname"></a>/tn \<TaskName >

指定任务的名称。 在系统上的每个任务必须具有唯一的名称。 名称必须符合有关文件的名称的规则，并且不能超过 238 个字符。 使用引号括起包含空格的名称。

##### <a name="tr-taskrun"></a>/ t\<任务 >

指定的程序或任务运行的命令。 键入可执行文件、 脚本文件或批处理文件的完全限定的路径和文件名称。 路径名称不得超过 262 个字符。 如果省略路径， **schtasks**假定该文件位于*SystemRoot*\System32 目录。

##### <a name="s-computer"></a>/s\<计算机 >

计划指定的远程计算机上的任务。 键入名称或 IP 地址的远程计算机 （有或没有反斜杠）。 默认值为本地计算机。 **/U**并 **/p**参数都有效只能使用 **/s**。

##### <a name="u-domainuser"></a>/u [\<域 >\]<User>

使用指定的用户帐户的权限运行此命令。 默认值为本地计算机的当前用户的权限。 **/U**并 **/p**参数都是仅对计划任务在远程计算机上的有效 (**/s**)。

用于指定帐户的权限来计划任务，并运行任务。 若要使用的其他用户权限运行任务，请使用 **/ru**参数。

用户帐户必须是远程计算机上 Administrators 组的成员。 此外，本地计算机必须是远程计算机的计算机，在同一个域中，或必须是远程计算机域信任的受信任的域中。

##### <a name="p-password"></a>/p \<Password>

在指定的用户帐户提供密码 **/u**参数。 如果使用 **/u**参数，但省略 **/p**参数或在 password 参数， **schtasks**提示你输入密码，然后不显示你键入的文本。

**/U**并 **/p**参数都是仅对计划任务在远程计算机上的有效 (**/s**)。

##### <a name="ru-domainuser--system"></a>/ru {[\<域 >\] <User> |系统}

使用指定的用户帐户的权限运行该任务。 默认情况下，在任务运行使用本地计算机的当前用户的权限时还是使用指定的用户权限 **/u**参数，如果包含。 **/Ru**当在本地或远程计算机上计划任务参数才有效。

|ReplTest1|描述|
|-----|-----------|
|[\<域 >\]<User>|指定备用用户帐户。|
|系统或""|指定本地系统帐户，操作系统和系统服务使用的一个高特权帐户。|

##### <a name="rp-password"></a>/rp\<密码 >

中指定的用户帐户提供密码 **/ru**参数。 如果省略此参数指定用户帐户时**SchTasks.exe**提示你输入密码，然后不显示你键入的文本。

不要使用 **/rp**使用系统帐户凭据运行任务的参数 (**/ru 系统**)。 系统帐户不具有密码和**SchTasks.exe**不提示输入一个。

##### <a name="mo-modifier"></a>/ 月\<修饰符 >

指定任务在其计划类型内的运行频率。 此参数才有效，但可选的为一分钟，每小时、 每天、 每周和每月计划。 默认值为 1。

|计划类型|修饰符值|描述|
|-------------|---------------|-----------|
|分钟|1 - 1439|在任务运行每个\<N > 分钟。|
|每小时|1 - 23|在任务运行每个\<N > 小时。|
|每日|1 - 365|在任务运行每个\<N > 天。|
|每周|1 - 52|在任务运行每个\<N > 周。|
|一次|没有修饰符。|在任务运行一次。|
|ONSTART|没有修饰符。|在启动时运行该任务。|
|登录时|没有修饰符。|在任务运行时指定的用户 **/u**参数登录。|
|ONIDLE|没有修饰符。|在任务运行在系统空闲指定的分钟数后 **/i**参数，即使用与 ONIDLE 所必需的。|
|每月|1 - 12|在任务运行每个\<N > 个月。|
|每月|最后一天|任务在上个月的最后一天运行。|
|每月|首先，第二，第三，第四周、 最后|使用具有 **/d**\<天 > 参数，以在特定的周和一天运行任务。 例如，在该月第三个星期三。|

##### <a name="d-dayday--"></a>/d Day[,Day...] | *

指定在星期几或一天 （或天） 每月的某一天 （或天）。 仅对于每周或每月计划有效。

|计划类型|修饰符|日的值 (/d)|描述|
|-------------|--------|---------------|-----------|
|每周|1 - 52|MON - SUN[,MON - SUN...] | *|可选。 默认值为星期一。 通配符值 （*） 表示每一天。|
|每月|首先，第二，第三，第四周、 最后|星期一-SUN|所需的特定一周计划。|
|每月|无或 {1-12}|1 - 31|可选的仅在没有修饰符的有效 (**/月**) 参数 （特定日期计划） 时，或者当 **/月**为 1-12 ("每个\<N > 个月"计划)。 默认值是 1 （每月的第一天）。|

##### <a name="m-monthmonth"></a>/m 月 [，...月]

指定一个月或计划的任务应在此期间运行的月份。 有效值为年 1 月-12 月和 * （每个月）。 **/M**参数是仅对于每月计划有效。 使用最后一天修饰符时，它是必需的。 否则为它是可选的默认值为 * （每个月）。

##### <a name="i-idletime"></a>/i \<IdleTime>

指定多少分钟计算机处于空闲状态之前在任务开始。 有效的值是从 1 到 999 之间的整数。 此参数是仅对于 ONIDLE 计划有效，则需要。

##### <a name="st-starttime"></a>/st \<StartTime >

指定任务 （每的次启动时启动） 中的时间\<格式 hh: mm > 24-小时。 默认值为本地计算机上的当前时间。 **/St**参数是有效的分钟，每小时、 每天、 每周、 每月，并一次计划。 它可实现一次计划。

##### <a name="ri-interval"></a>/ri\<间隔 >

以分钟为单位指定的重复间隔。 这不是适用于计划类型：分钟、 每小时、 ONSTART、 登录时，和 ONIDLE 中。 有效范围是 1 到 599940 分钟 （599940 分钟 = 9999 小时）。 如果指定了 /ET 或 /DU，重复间隔时间默认为 10 分钟。

##### <a name="et-endtime"></a>/et \<EndTime >

指定以分钟或每小时任务计划结束的时间\<格式 hh: mm > 24-小时。 指定的结束时间之后, **schtasks**才会开始任务再次开始时间重复出现。 默认情况下，任务计划具有结束时间。 此参数是可选的仅与分钟或每小时计划有效。

有关示例，请参阅：
-   "计划的任务，每 100 分钟运行在非工作时间"**来计划运行的任务，每个** \<N >**分钟**部分。

##### <a name="du-duration"></a>/du\<持续时间 >

指定为分钟或每小时计划中的时间的最大长度\<HHHH:MM > 24-小时格式。 指定的时间过后之后, **schtasks**才会开始任务再次开始时间重复出现。 默认情况下，任务计划有没有最大持续时间。 此参数是可选的仅与分钟或每小时计划有效。

有关示例，请参阅：
-   "计划任务运行 10 个小时内每 3 小时"**来计划运行的任务，每个** \<N >**小时**部分。

##### <a name="k"></a>/k

任务在指定的时间运行的程序将停止 **/et**或 **/du**。 而无需 **/k**， **schtasks**不会启动该程序再次达到指定的时间后 **/et**或者 **/du**，但它不会停止如果它仍在运行该程序。 此参数是可选的仅与分钟或每小时计划有效。

有关示例，请参阅：
-   "计划的任务，每 100 分钟运行在非工作时间"**来计划运行的任务，每个** \<N >**分钟**部分。

##### <a name="sd-startdate"></a>/sd \<StartDate>

指定任务计划开始的日期。 默认值为本地计算机上的当前日期。 **/Sd**参数是有效的和可选的所有计划类型。

格式*StartDate*为本地计算机中选择的区域设置会随**区域和语言选项**中**控制面板**。 只有一种格式是有效的每个区域设置。

下表中列出有效的日期格式。 使用最相似的格式为选择的格式**短日期**中**区域和语言选项**中**控制面板**本地计算机上。

|ReplTest1|描述|
|-----|-----------|
|\<MM>/<DD>/<YYYY>|使用适用于月开头的格式，例如**英语 （美国）** 并**西班牙语 （巴拿马）**。|
|\<DD>/<MM>/<YYYY>|使用以日期开头的格式，如下所示**保加利亚语**并**荷兰语 （荷兰）**。|
|\<YYYY>/<MM>/<DD>|使用以年份开头的格式，如下所示**瑞典语**并**法语 （加拿大）**。|

/ed \<EndDate >

指定的计划结束的日期。 该参数为可选参数。 不在一次、 ONSTART、 登录时，或 ONIDLE 计划中有效。 默认情况下，计划具有无结束日期。

格式*EndDate*为本地计算机中选择的区域设置会随**区域和语言选项**中**控制面板**。 只有一种格式是有效的每个区域设置。

下表中列出有效的日期格式。 使用最相似的格式为选择的格式**短日期**中**区域和语言选项**中**控制面板**本地计算机上。

|ReplTest1|描述|
|-----|-----------|
|\<MM>/<DD>/<YYYY>|使用适用于月开头的格式，例如**英语 （美国）** 并**西班牙语 （巴拿马）**。|
|\<DD>/<MM>/<YYYY>|使用以日期开头的格式，如下所示**保加利亚语**并**荷兰语 （荷兰）**。|
|\<YYYY>/<MM>/<DD>|使用以年份开头的格式，如下所示**瑞典语**并**法语 （加拿大）**。|

##### <a name="it"></a>/it

指定运行任务时，才"运行方式"用户 （该任务运行的用户帐户） 登录到计算机。 此参数不起使用系统的权限运行的任务。

默认情况下，"运行方式"用户是本地计算机在计划任务时的当前用户或指定的帐户 **/u**参数，如果使用。 但是，如果该命令包含 **/ru**参数，然后"运行方式"用户是指定的帐户 **/ru**参数。

有关示例，请参阅：
-   "若要计划的任务，每个 70 天运行，如果在登录时"中**来计划运行的任务，每个** *N* **天**部分。
-   "运行任务仅当特定用户的登录"**计划使用不同的权限运行的任务**部分。

##### <a name="z"></a>/z

指定要删除其计划完成后的任务。

##### <a name="f"></a>/f

指定要创建任务，并禁止显示警告，如果指定的任务已存在。

##### <a name=""></a>/?

在命令提示符下显示帮助。

### <a name="BKMK_minutes"></a>若要计划的任务，每个 N 分钟运行一次

#### <a name="minute-schedule-syntax"></a>每分钟计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每分钟计划中， **/sc 分钟**参数是必需的。 **/月**（修饰符） 参数是可选的它指定该任务的每次运行之间的分钟数。 默认值为 **/月**为 1 （每隔一分钟）。 **/Et** （结束时间） 和 **/du** （持续时间） 参数是可选的可以使用带或不带 **/k** （结束任务） 参数。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>若要计划每隔 20 分钟运行一次的任务

下面的命令计划安全运行的脚本，Sec.vbs 每隔 20 分钟。 该命令使用 **/sc**参数来指定分钟计划并 **/月**参数来指定 20 分钟的间隔。

由于该命令不包括，起始日期或时间，任务将启动 20 分钟后该命令完成后，并运行每隔 20 分钟之后每当系统正在运行。 请注意，安全脚本源文件位于远程计算机，但任务计划，并在本地计算机上执行。
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>若要计划在非工作时间每个 100 分钟运行一次的任务

下面的命令计划安全，Sec.vbs 要运行的脚本在本地计算机上下午 5:00 之间每隔 100 分钟 和上午 7:59 每一天。 该命令使用 **/sc**参数来指定分钟计划并 **/月**参数来指定 100 分钟的间隔。 它使用 **/st**并 **/et**参数指定的开始时间和每日计划的结束时间。 它还使用 **/k**参数来停止脚本，如果它仍在运行上午 7:59 无需 **/k**， **schtasks**上午 7:59，但如果在上午 6:20 启动实例后无法启动脚本 是仍在运行，它不会停止它。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>若要计划的任务，每个 N 小时运行一次

#### <a name="hourly-schedule-syntax"></a>每小时计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每小时计划中， **/sc 每小时**参数是必需的。 **/月**（修饰符） 参数是可选的它指定该任务的每次运行之间的小时数。 默认值为 **/月**为 1 （每隔一小时）。 **/K** （结束任务） 参数是可选的可用于任一 **/et** （最终在指定的时间） 或 **/du** （在指定间隔之后结束）。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>若要计划的任务，每个五个小时运行一次

下面的命令计划 MyApp 程序 2002 年 3 月的第一天开始每五个小时运行一次。 它使用 **/月**参数来指定的时间间隔和 **/sd**参数来指定开始日期。 由于该命令未指定开始时间，当前时间作为开始时间使用。

因为本地计算机设置为使用**英语 （津巴布韦）** 选项**区域和语言选项**中**控制面板**，开始日期的格式为 MM/DD/YYYY(03/01/2002)。
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>若要计划在前一个小时的 5 分钟内每小时运行的任务

下面的命令计划要运行每小时开始午夜过后五分钟的 MyApp 程序。 因为 **/月**省略参数，该命令使用默认值为每小时计划，这是每隔一 (1) 的小时。 如果此命令运行后的上午 12:05，该程序将不运行直到下一天。
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>若要计划 10 个小时内每 3 小时运行一次的任务

下面的命令计划要运行 10 个小时内每 3 个小时的 MyApp 程序。

该命令使用 **/sc**参数来指定每小时计划并 **/月**参数来指定 3 个小时的时间间隔。 它使用 **/st**参数来启动在午夜的计划和 **/du**参数 10 小时后结束的重复周期。 因为该程序只需几分钟，在运行 **/k**参数，它仍在运行时持续时间到期时将停止该程序，没有必要。
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
在此示例中，在任务运行在中午 12:00、 凌晨 3:00，上午 6:00 和上午 9:00。 持续时间为 10 小时，因为在中午 12:00 再次不运行任务 相反，它启动再次在凌晨 12:00 下一天。

### <a name="BKMK_days"></a>若要计划的任务，每个 N 天运行一次

#### <a name="daily-schedule-syntax"></a>每日计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每日计划 **/sc 每天**参数是必需的。 **/月**（修饰符） 参数是可选的它指定的任务的每次运行之间的天数。 默认值为 **/月**为 1 （每天）。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-day"></a>若要计划每天运行一次的任务

下面的示例计划每天运行一次，每日上午 8:00 MyApp 程序 直到 2002 年 12 月 31日日。 因为它会省略 **/月**参数，默认间隔为 1 用于运行每日的命令。

在此示例中，因为本地计算机系统设置为**英语 （英国）** 选项**区域和语言选项**中**控制面板**，格式为结束日期为 DD/MM/YYYY (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>若要计划每隔 12 天运行一次的任务

下面的示例计划 MyApp 程序在下午 1:00 运行每隔 12 天 (13:00) 自 2002 年 12 月 31 日。 该命令使用 **/月**参数来指定两 （2） 天的间隔和 **/sd**并 **/st**参数指定的日期和时间。

在此示例中，因为系统设置为**英语 （津巴布韦）** 选项**区域和语言选项**中**控制面板**，结束日期的格式为 MM/DD /YYYY （2002 年 12 月 31）
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>若要计划的任务，如果在登录时运行每 70 天

下面的命令计划安全运行的脚本，Sec.vbs 每 70 天。 该命令使用 **/月**参数来指定 70 天的间隔。 它还使用 **/it**参数来指定仅当其帐户下运行任务的用户登录到计算机时运行任务。 因为该任务将运行我的用户帐户的权限，则仅当我登录时也会运行该任务。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> 来与交互式仅标识任务 (**/it**) 属性，请使用 verbose 查询 **(/ 查询 /v**)。 在详细的查询显示模式中的任务的 **/it**，则**登录模式**字段的值为**交互仅**。

### <a name="BKMK_weeks"></a>若要计划每 N 周运行的任务

#### <a name="weekly-schedule-syntax"></a>每周计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在每周计划中， **/sc 每周**参数是必需的。 **/月**（修饰符） 参数是可选的它指定该任务的每次运行之间的周数。 默认值为 **/月**为 1 （每周）。

每周计划还具有一个可选 **/d**参数来计划任务运行指定的周天数或所有天 (*)。默认值为周一 （星期一）。每一天 (*) 选项相当于计划每日任务。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>若要计划每隔六周运行一次的任务

下面的命令计划要运行的远程计算机上每隔六周的 MyApp 程序。 该命令使用 **/月**参数来指定的时间间隔。 由于该命令忽略 **/d**参数，在星期一运行的任务。

此命令还使用 **/s**参数来指定远程计算机并 **/u**参数以通过用户的管理员帐户的权限运行该命令。 因为 **/p**省略参数，则**SchTasks.exe**提示用户输入管理员帐户密码。

此外，由于远程运行命令时，该命令，包括到 MyApp.exe，路径中的所有路径都引用到远程计算机上的路径。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>若要计划的任务，在星期五运行每隔一周

下面的命令计划要运行其他每个星期五的任务。 它使用 **/月**参数来指定两周时间间隔和 **/d**参数来指定日期是星期几。 若要计划运行的任务，每个星期五，省略 **/月**参数或将其设置为 1。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>若要计划的任务，每个 N 月运行一次

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在此计划类型，**每月 /sc**参数是必需的。 **/月**（修饰符） 参数，指定任务的每次运行之间的月数，该参数是可选的默认值为 1 （每个月）。 此计划类型还具有一个可选 **/d**参数来计划任务在指定日期的月份运行。 默认值为 1 （每月的第一天）。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>若要计划每月的第一天运行的任务

下面的命令计划要运行的每个月的第一天的 MyApp 程序。 因为值为 1 是两个默认 **/月**（修饰符） 参数和 **/d** （天） 参数，从命令中省略这些参数。
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>若要计划每隔三个月运行一次的任务

下面的命令计划要运行每三个月的 MyApp 程序。 它使用 **/月**参数来指定的时间间隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>若要计划每隔一个月的第 21 日午夜运行的任务

下面的命令计划在午夜月 21 日运行每隔一个月的 MyApp 程序。 该命令指定，此任务应运行为一年，从 2002 年 7 月 2 日为 2003 年 6 月 30 日。

该命令使用 **/月**参数指定的每月的时间间隔 （每隔两个月）， **/d**参数来指定日期，并 **/st**可以指定的时间。 它还使用 **/sd**并 **/ed**参数指定的日期和结束日期，分别。 因为本地计算机设置为**英语 （南非）** 选项**区域和语言选项**中**控制面板**，以本地格式指定日期YYYY/MM/DD。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>若要计划在特定日期是星期几运行的任务

#### <a name="weekly-schedule-syntax"></a>每周计划语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"Day 一周中的"计划是每周计划的变体。 在每周计划中， **/sc 每周**参数是必需的。 **/月**（修饰符） 参数是可选的它指定该任务的每次运行之间的周数。 默认值为 **/月**为 1 （每周）。 **/D**参数，这是可选的可以计划任务运行指定的周天数或所有天 （*）。 默认值为周一 （星期一）。 每天选项 (**/d \*** ) 等效于计划每日任务。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>若要计划运行的任务，每个星期三

下面的命令计划要在星期三运行每周的 MyApp 程序。 该命令使用 **/d**参数来指定日期是星期几。 由于该命令忽略 **/月**参数，该任务运行每周。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>若要计划在星期一和星期五运行每个 8 周内的任务

下面的命令计划在星期一和每个第八个每周的星期五运行的任务。 它使用 **/d**参数来指定日期并 **/月**参数来指定八周间隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>若要计划在个月的特定一周运行的任务

#### <a name="specific-week-syntax"></a>特定一周语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在此计划类型，**每月 /sc**参数， **/月**（修饰符） 参数，并 **/d** （天） 参数是必需的。 **/月**（修饰符） 参数指定该任务在其运行的星期日期。 **/D**参数指定日期是星期几。 （可以指定仅对此计划类型一周中的一天。）此计划还具有可选 **/m** （月） 参数，可让你的特定几个月或每月计划的任务 (*)。默认值为 **/m**参数是每个月 (*)。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>若要计划任务在每个月的第二个星期日

下面的命令计划 MyApp 程序上的每个月的第二个星期日运行。 它使用 **/月**参数来指定月份的第二个星期和 **/d**参数以指定的天。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>若要计划任务在 3 月和年 9 月的第一个星期一

下面的命令计划要运行在 3 月和年 9 月的第一个星期一的 MyApp 程序。 它使用 **/月**参数来指定月份的第一周和 **/d**参数以指定的天。 它使用 **/m**参数指定的月份，并用逗号分隔的月份自变量。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>若要计划在特定日期每月运行的任务

#### <a name="specific-date-syntax"></a>指定日期的语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在特定日期计划类型**每月 /sc**参数和 **/d** （天） 参数是必需的。 **/D**参数指定的日期的月份 (1-31)，不能日期是星期几。 在计划中，可以指定仅一天。 **/月**（修饰符） 参数与此计划类型无效。

**/M** （月） 参数是可选的此计划类型和默认值是每个月 （*）。 **Schtasks**不允许您计划的任务的日期的指定月份中不会出现 **/m**参数。 但是，如果省略 **/m**参数，而不显示在每个月，如第 31 天，则该任务的日期的任务不运行在较短的几个月的计划。 若要为该月的最后一天中计划的任务，使用最后一天的计划类型。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>若要计划任务在每个月的第一天

下面的命令计划要运行的每个月的第一天的 MyApp 程序。 因为默认修饰符是 none （即没有修饰符），默认日期是第一天，并默认的月份是每个月，该命令不需要任何其他参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>若要计划任务在 5 月和年 6 月的第 15 天

下面的命令计划在下午 3:00 点在 5 月 15 日和年 6 月 15 上运行的 MyApp 程序 (15:00). 它使用 **/m**参数来指定日期并 **/m**参数来指定在几个月。 它还使用 **/st**参数指定的开始时间。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>若要计划的任务，在上一个月的最后一天运行

#### <a name="last-day-syntax"></a>最后一天语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在最后一天的计划类型中， **/sc 每月**参数， **/月最后一天**（修饰符） 参数并 **/m** （月） 参数是必需的。 **/D** （天） 参数无效。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>若要计划任务在每个月的最后一天

下面的命令计划要运行的每个月的最后一天的 MyApp 程序。 它使用 **/月**参数指定的最后一天和 **/m**带通配符 （*） 以指示该程序以运行每个月的参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>要将任务安排在下午 6:00 年 2 月和年 3 月的最后一天

下面的命令计划在下午 6:00 运行 2 月份的最后一天和年 3 月的最后一天的 MyApp 程序 它使用 **/月**参数指定的最后一天 **/m**参数来指定月份数，并且 **/st**参数指定的开始时间。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>若要计划的任务，运行一次

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在运行一次计划类型中， **/sc 一次**参数是必需的。 **/St**参数，它指定在任务运行的时间，是必需的。 **/Sd**参数，指定在任务运行的日期，该参数是可选的。 **/月**（修饰符） 和 **/ed** （结束日期） 参数不能对此计划类型。

**Schtasks**不允许您计划任务运行一次的日期和时间指定是在过去，如果基于本地计算机的时间。 若要计划的任务，在不同时区的远程计算机上运行一次，你必须计划其在该日期之前和在本地计算机上发生的时间。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-one-time"></a>若要计划的任务，运行一次

下面的命令计划在 2003 年 1 月 1 日午夜运行的 MyApp 程序。 它使用 **/sc**参数来指定计划类型和 **/sd**并**st**来指定日期和时间。

因为本地计算机使用**英语 （美国）** 选项**区域和语言选项**中**控制面板**，开始日期的格式为 MM/DD/YYYY。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>若要计划每次系统启动时运行的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

在开始计划类型中 **/sc onstart**参数是必需的。 **/Sd** （开始日期） 参数是可选的默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>若要计划在系统启动时运行的任务

下面的命令计划要运行每次在系统启动时，从 2001 年 3 月 15 日的 MyApp 程序：

因为本地计算机是使用**英语 （美国）** 选项**区域和语言选项**中**控制面板**，开始日期的格式为 MM/DD/YYYY.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>若要计划的任务，在运行时用户登录

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"登录"计划类型计划后，只要任何用户登录到计算机上运行的任务。 在"登录"计划类型中， **/sc 登录时**参数是必需的。 **/Sd** （开始日期） 参数是可选的默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>若要计划的任务，在运行时用户登录到远程计算机

下面的命令计划每次用户 （任何用户） 登录到远程计算机时运行的批处理文件。 它使用 **/s**参数来指定远程计算机。 由于命令是远程的该命令，包括批处理文件的路径中的所有路径都引用到远程计算机上的路径。
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>若要计划会在系统空闲时运行的任务

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>备注

"在空闲"计划类型计划任务运行时通过指定的时间内没有任何用户活动 **/i**参数。 在"在空闲"计划类型， **/sc onidle**参数和 **/i**参数是必需的。 **/Sd** （开始日期） 是可选的默认值为当前日期。

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>若要计划每次在计算机处于空闲状态时运行的任务

下面的命令计划 MyApp 程序每当计算机处于空闲状态时运行。 它使用所需 **/i**参数来指定，计算机必须保持空闲状态的任务开始运行之前的十分钟。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>若要计划的任务，现在可以运行

**Schtasks**不具有"运行现在"选项，但可以通过创建一次运行，并在几分钟内启动的任务来模拟该选项。

#### <a name="syntax"></a>语法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>示例

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>若要计划的任务，从现在运行几分钟时间。

下面的命令计划任务运行一次，在 2002 年 11 月 13 日下午 2:18 本地时间。

因为本地计算机是使用**英语 （美国）** 选项**区域和语言选项**中**控制面板**，开始日期的格式为 MM/DD/YYYY.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>若要计划使用不同的权限运行的任务

您可以计划在本地和远程计算机上使用备用帐户的权限运行的所有类型的任务。 除了特定的计划类型所需的参数外 **/ru**参数是必需的并且 **/rp**参数是可选的。

#### <a name="examples"></a>示例

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>若要在本地计算机上具有管理员权限运行任务

下面的命令计划要运行在本地计算机上的 MyApp 程序。 它使用 **/ru**用于指定该任务应运行的用户的管理员帐户 (Admin06) 的权限。 在此示例中，计划任务运行每个星期二，但可以使用备用的权限运行的任务使用所有的计划类型。
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
在响应中， **SchTasks.exe** Admin06 帐户的"运行方式"密码提示，然后显示一条成功消息。
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>若要在远程计算机上使用备用的权限运行任务

下面的命令计划 MyApp 程序每隔四天的市场营销计算机上运行。

该命令使用 **/sc**参数来指定每日计划并 **/月**参数来指定四天的间隔。

该命令使用 **/s**参数来提供的远程计算机的名称和 **/u**参数来指定具有权限才能计划远程计算机上的任务的帐户 (在市场营销人员 Admin01计算机）。 它还使用 **/ru**参数来指定该任务应使用用户的非管理员帐户的权限运行 (User01 域中的)。 无需 **/ru**参数，具有指定的帐户的权限才能运行该任务 **/u**。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks**第一次请求通过名为的用户的密码 **/u**参数 （以运行该命令），然后，请求通过名为的用户的密码 **/ru**参数 （以运行任务）。 进行身份验证密码后**schtasks**显示一条消息，该值指示计划任务。
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.

```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>若要运行任务，仅当特定用户登录

下面的命令计划 AdminCheck.exe 要运行的程序在公用计算机上每星期五在凌晨 4:00，但如果计算机的管理员登录。

该命令使用 **/sc**参数来指定每周计划 **/d**参数以指定的天和 **/st**参数指定的开始时间。

该命令使用 **/s**参数来提供的远程计算机的名称和 **/u**参数来指定具有权限才能计划远程计算机上的任务的帐户。 它还使用 **/ru**参数来配置要使用的公用计算机 (Public\Admin01) 的管理员权限运行的任务和 **/it**参数以指示该任务以仅运行当登录 Public\Admin01 帐户。
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**注意**
-   来与交互式仅标识任务 (**/it**) 属性，请使用 verbose 查询 **(/ 查询 /v**)。 在详细的查询显示模式中的任务的 **/it**，则**登录模式**字段的值为**交互仅**。

### <a name="BKMK_sys_perms"></a>若要计划使用系统的权限运行的任务

所有类型的任务可以使用系统帐户的权限运行在本地和远程计算机上。 除了特定的计划类型所需的参数外 **/ru 系统**(或 **/ru""**) 参数是必需的并且 **/rp**参数无效。

**重要须知**
-   系统帐户没有交互登录权限。 用户无法看到或与程序交互或使用系统的权限运行的任务。
-   **/Ru**参数用于确定在任务运行，不是用于计划任务的权限的权限。 仅管理员可以计划任务，而不考虑的值 **/ru**参数。

**注意**

若要标识使用系统的权限运行的任务，请使用详细的查询 (**/查询** **/v**)。 在详细的查询显示模式中的系统运行任务，**用户身份运行**字段的值为**NT AUTHORITY\SYSTEM**并**登录模式**字段的值为**仅后台**。

#### <a name="examples"></a>示例

#### <a name="to-run-a-task-with-system-permissions"></a>若要使用系统的权限运行任务

下面的命令计划要运行使用系统帐户的权限在本地计算机上的 MyApp 程序。 在此示例中，计划任务在每月的第十五天运行，但可将所有的计划类型用于以 system 权限运行的任务。

该命令使用 **/ru 系统**参数来指定系统安全上下文。 系统任务不使用密码，因为 **/rp**省略参数。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
在响应中， **SchTasks.exe**显示一条信息性消息和一条成功消息。 它不会提示输入密码。
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.

```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>若要在远程计算机上具有系统权限运行任务

下面的命令计划 MyApp 程序 Finance01 计算机上运行每个第二天早上的凌晨 4:00 使用系统的权限。

该命令使用 **/tn**参数来命名该任务并 **/tr**参数指定 MyApp 程序的远程副本。 它使用 **/sc**参数来指定每日计划，但省略 **/月**参数 1 （每天） 是默认值。 它使用 **/st**参数来指定开始时间，也是该任务将运行每一天的时间。

该命令使用 **/s**参数来提供的远程计算机的名称和 **/u**参数来指定具有权限才能计划远程计算机上的任务的帐户。 它还使用 **/ru**参数来指定任务应在系统帐户下运行。 无需 **/ru**参数，具有指定的帐户的权限才能运行该任务 **/u**。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks**请求通过名为的用户的密码 **/u**参数，并进行身份验证密码后, 显示一条消息，指示该任务创建将使用系统的权限运行帐户。
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.

```

### <a name="BKMK_multi_progs"></a>若要计划的任务，运行多个程序

每个任务运行只有一个程序。 但是，可以创建运行多个程序的批处理文件，并计划要运行的批处理文件的任务。 以下过程演示了此方法：
1.  创建你想要运行的程序将启动一个批处理文件。

    在此示例中，创建一个批处理文件，启动事件查看器 (Eventvwr.exe) 和系统监视器 (Perfmon.exe)。  
    -   打开文本编辑器，例如记事本。
    -   键入名称和每个程序的可执行文件的完全限定的路径。 在这种情况下，该文件包括以下语句。  
        ```
        C:\Windows\System32\Eventvwr.exe 
        C:\Windows\System32\Perfmon.exe
        ```  
    -   将文件另存 MyApps.bat。
2.  使用**Schtasks.exe**若要创建一个运行 MyApps.bat 的任务。

    以下命令将创建监视器任务中，每次有人登录时运行。 它使用 **/tn**参数来命名该任务，并 **/tr**参数运行 MyApps.bat。 它使用 **/sc**参数来指示登录时计划类型和 **/ru**参数来运行任务的用户的管理员帐户的权限。  
    ```
    schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
    ```  
    此命令的结果时在用户登录到计算机，任务将启动事件查看器和系统监视器。

### <a name="BKMK_remote"></a>若要计划在远程计算机运行的任务

若要计划要在远程计算机上运行的任务，必须将任务添加到远程计算机的计划。 可以在远程计算机上计划的所有类型的任务，但必须满足以下条件。
-   必须具有权限才能计划该任务。 在这种情况下，您必须登录到本地计算机是远程计算机上 Administrators 组的成员的帐户也必须使用 **/u**参数提供的远程计算机的管理员凭据.
-   可以使用 **/u**参数仅在本地和远程计算机位于相同域或本地计算机是远程计算机域信任的域中时。 否则为远程计算机不能对指定的用户帐户进行身份验证，它无法验证该帐户是 Administrators 组的成员。
-   该任务必须有足够的权限在远程计算机上运行。 所需的权限因该任务。 默认情况下，在任务运行与本地计算机的当前用户的权限; 如果 **/u**参数，任务使用指定的帐户的权限运行 **/u**参数。 但是，可以使用 **/ru**参数运行该任务使用不同的用户帐户的权限时还是使用系统权限。

#### <a name="examples"></a>示例

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>管理员安排远程计算机上的任务

下面的命令计划要运行 SRV01 远程计算机上立即启动每个十天的 MyApp 程序。 该命令使用 **/s**参数来提供的远程计算机的名称。 因为本地的当前用户是远程计算机的管理员 **/u**参数，它提供备用计划任务的权限，没有必要。

请注意，计划在远程计算机上的任务时，所有参数都指的是远程计算机。 因此，指定的可执行文件 **/tr**参数指 MyApp.exe 远程计算机上的副本。
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
在响应中， **schtasks**显示一条成功消息，指出系统计划任务。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>用户计划 (用例 1) 在远程计算机上的命令

下面的命令计划 MyApp 程序 SRV06 远程计算机上运行每隔三个小时。 由于需要管理员权限来计划任务，该命令使用 **/u**和 **/p**参数提供的凭据的用户的管理员帐户 (在 Reskits Admin01域）。 默认情况下，这些权限还用于运行任务。 但是，因为该任务不需要管理员权限才能运行，该命令包含 **/u**并 **/rp**参数来覆盖默认值并运行具有权限的用户的任务在远程计算机上的非管理员帐户。
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
在响应中， **schtasks**显示一条成功消息，指出系统计划任务。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>用户计划 (案例 2) 在远程计算机上的命令

下面的命令计划 MyApp 程序 SRV02 远程计算机上运行的每个月的最后一天。 由于本地的当前用户 (user03) 不是远程计算机的管理员，该命令使用 **/u**参数提供的凭据的用户的管理员帐户 (域中的 Admin01)。 计划的任务并运行该任务将使用管理员帐户权限。
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
由于该命令未包括 **/p** （密码） 参数**schtasks**提示输入密码。 然后，它显示一条成功消息以及在这种情况下，一条警告。
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.

```
此警告指示远程域无法验证指定的帐户 **/u**参数。 在这种情况下，远程域无法验证的用户帐户，因为本地计算机不是远程计算机域信任的域的成员。 此操作时，任务作业会显示在列表中的计划的任务，但任务已实际空并且将不会运行。

显示在下面的详细的查询中公开该任务的问题。 在显示中，请注意的值**下次运行时间**是**Never**并且的值**作为用户运行**是**无法检索与任务计划程序数据库**。

此计算机曾同一个域或受信任的域的成员，该任务将具有已成功计划且方式运行所指定。
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

-   若要运行 **/create**命令与其他用户，使用的权限 **/u**参数。 **/U**参数无效，仅在远程计算机上计划任务。
-   若要查看更多**schtasks /create**示例中，键入**schtasks / 创建 /？** 。
-   若要计划使用其他用户的权限运行的任务，请使用 **/ru**参数。 **/Ru**参数是有效的本地和远程计算机上的任务。
-   若要使用 **/u**参数在本地计算机必须位于远程计算机的域或必须是远程计算机域信任的域中。 否则为要么不创建任务，或任务作业为空，该任务不会运行。
-   **Schtasks**始终提示输入密码，除非您提供一个，即使你计划使用当前用户帐户在本地计算机上的任务。 这是正常行为**schtasks**。
-   **Schtasks**不会验证程序文件位置或用户帐户密码。 如果不执行操作的用户帐户输入正确的文件位置或正确的密码，创建任务，但它不会运行。 此外，如果帐户密码更改或过期，并且在不更改保存在任务中的密码，然后该任务不运行。
-   系统帐户没有交互登录权限。 用户不会看到，并且不能以 system 权限运行的程序进行交互。
-   每个任务运行只有一个程序。 但是，您可以创建启动多个任务的批处理文件，然后计划的任务，用于运行批处理文件。
-   当你创建它，你可以测试任务。 使用**运行**操作来测试该任务，然后检查 SchedLgU.txt 文件 (*SystemRoot*\SchedLgU.txt) 错误。

## <a name="BKMK_change"></a>更改计划任务

更改一个或多个任务的下列属性。
-   在任务运行程序 (**/tr**)。
-   任务运行的用户帐户 (**/ru**)。
-   用户帐户的密码 (**/rp**)。
-   将仅交互属性添加到任务 (**/it**)。

### <a name="syntax"></a>语法

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parameters

|术语|定义|
|----|----------|
|/tn \<TaskName >|标识要更改的任务。 输入任务名称。|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （有或没有反斜杠）。 默认值为本地计算机。|
|/u [\<域 >\]<User>|使用指定的用户帐户的权限运行此命令。 默认值为本地计算机的当前用户的权限。 指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**并 **/p**参数是有效仅用于更改远程计算机上的任务 (**/s**)。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。 如果您使用 **/u**参数，但省略 **/p**参数或在 password 参数， **schtasks**提示你输入密码。</br>**/U**并 **/p**参数都有效只能使用 **/s**。|
|/ru {[\<Domain>\]<User> | 系统}|指定要更改任务运行的用户帐户。 有效项为指定的本地系统帐户，""，"NT AUTHORITY\SYSTEM"或"系统"。</br>当您更改用户帐户时，还必须更改用户密码。 如果命令具有 **/ru**参数，但不是 **/rp**参数， **schtasks**提示输入新密码。</br>使用本地系统帐户的权限运行的任务不需要或提示输入密码。|
|/rp\<密码 >|指定用于现有的用户帐户或指定的用户帐户的新密码 **/ru**参数。 忽略此参数是使用与本地系统帐户一起使用。|
|/ t\<任务 >|更改任务运行的程序。 输入完全限定的路径和文件名的可执行文件、 脚本文件或批处理文件的名称。 如果省略路径， **schtasks**假定该文件位于\<systemroot > \System32 目录。 指定的程序将替换原始任务运行程序。|
|/st \<Starttime >|指定任务中，使用 24 小时时间格式 hh: mm 的开始时间。 例如，值为 14:30 相当于下午 2:30 的 12 小时制时间。|
|/ri\<间隔 >|以分钟为单位指定为计划任务重复间隔时间。 有效范围是 1-599940 （599940 分钟 = 9999 小时）。|
|/et \<EndTime >|指定任务中，使用 24 小时时间格式 hh: mm 的结束时间。 例如，值为 14:30 相当于下午 2:30 的 12 小时制时间。|
|/du\<持续时间 >|指定要关闭任务时，在\<EndTime > 或<Duration>，如果指定。|
|/k|任务在指定的时间运行的程序将停止 **/et**或 **/du**。 而无需 **/k**， **schtasks**不会启动该程序再次达到指定的时间后 **/et**或者 **/du**，但它不会停止如果它仍在运行该程序。 此参数是可选的仅与分钟或每小时计划有效。|
|/sd \<StartDate>|指定应在其运行任务的第一个日期。 日期格式为 MM/DD/YYYY。|
|/ed \<EndDate >|指定应在其运行任务的最后一个日期。 格式为 MM/DD/YYYY。|
|/ENABLE|指定要启用计划的任务。|
|/DISABLE|指定要禁用计划的任务。|
|/it|指定运行计划的任务仅当"运行方式"用户 （该任务运行的用户帐户） 登录到计算机。</br>此参数对使用系统的权限运行的任务或任务的已有仅限交互式属性设置无效。 更改命令不能用于从任务中删除了只交互式属性。</br>默认情况下，"运行方式"用户是本地计算机在计划任务时的当前用户或指定的帐户 **/u**参数，如果使用。 但是，如果该命令包含 **/ru**参数，然后"运行方式"用户是指定的帐户 **/ru**参数。|
|/z|指定要删除其计划完成后的任务。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   **/Tn**并 **/s**参数标识任务。 **/Tr**， **/ru**，并 **/rp**参数指定可以更改任务的属性。
-   **/Ru**，并 **/rp**参数指定该任务运行的权限。 **/U**并 **/p**参数指定用来更改任务的权限。
-   若要更改远程计算机上的任务，用户必须被登录到本地计算机是远程计算机上 Administrators 组的成员的帐户。
-   若要运行 **/更改**命令与其他用户的权限 (**/u**， **/p**)，本地计算机必须位于远程计算机的域或必须位于域中远程计算机域信任。
-   系统帐户没有交互登录权限。 用户不会看到，并且不能以 system 权限运行的程序进行交互。
-   若要确定与任务 **/it**属性，请使用详细的查询 (**/查询 /v**)。 在详细的查询显示模式中的任务的 **/it**，则**登录模式**字段的值为**交互仅**。

### <a name="examples"></a>示例

### <a name="to-change-the-program-that-a-task-runs"></a>若要更改的任务运行程序

以下命令将更改从 VirusCheck.exe 运行到 VirusCheck2.exe 的病毒检查任务的程序。 此命令使用 **/tn**参数来标识该任务并 **/tr**参数来指定该任务的新程序。 （不能更改任务名称。）
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
在响应中， **SchTasks.exe**会显示以下成功消息：
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
此命令的结果的病毒检查任务现在运行 VirusCheck2.exe。

### <a name="to-change-the-password-for-a-remote-task"></a>若要更改远程任务的密码

以下命令将更改的远程计算机上，Svr01 RemindMe 任务的用户帐户的密码。 该命令使用 **/tn**参数来标识该任务并 **/s**参数来指定远程计算机。 它使用 **/rp**参数来指定新密码， p@ssWord3。

用户帐户的密码过期或发生更改时，此过程是必需的。 如果在任务中保存的密码将不再有效，该任务不运行。
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
在响应中， **SchTasks.exe**会显示以下成功消息：
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
此命令的结果 RemindMe 任务现在运行在其原始的用户帐户，但使用新密码。

### <a name="to-change-the-program-and-user-account-for-a-task"></a>若要更改任务的程序和用户帐户

以下命令将更改的任务运行程序和更改的用户帐户下运行任务。 从根本上来说，它使用旧的计划的新任务。 此命令将更改 ChkNews 任务中，在上午 9:00，每天早晨启动 Notepad.exe 改为启动 Internet Explorer。

该命令使用 **/tn**参数来标识该任务。 它使用 **/tr**参数来更改在任务运行的程序和 **/ru**参数，以更改任务运行的用户帐户。

**/Ru**，并 **/rp**省略参数，为用户帐户提供密码，该参数。 您必须提供密码的帐户，但可以使用 **/ru**，并 **/rp**参数和类型中的密码清除文本，或等待**SchTasks.exe**以提示您输入密码，然后在文本输入密码。
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
在响应中， **SchTasks.exe**请求的用户帐户的密码。 它会遮盖你键入的文本，因此密码不可见。
```
Please enter the password for DomainX\Admin01: 
```
请注意， **/tn**参数标识任务，并 **/tr**并 **/ru**参数更改任务的属性。 不能使用另一个参数以标识任务并不能更改任务名称。

在响应中， **SchTasks.exe**会显示以下成功消息：
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
此命令的结果 ChkNews 任务现在使用管理员帐户的权限运行 Internet Explorer。

### <a name="to-change-a-program-to-the-system-account"></a>若要将程序更改为系统帐户

以下命令更改 SecurityScript 任务，以便使其具有权限的系统帐户运行。 它使用 **/ru""** 参数来指示系统帐户。
```
schtasks /change /tn SecurityScript /ru ""
```
在响应中， **SchTasks.exe**会显示以下成功消息：
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
使用系统帐户权限运行的任务不需要密码，因为**SchTasks.exe**不提示输入一个。

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>仅当我登录时运行程序

以下命令将仅交互属性添加到 MyApp，现有的任务。 此属性可确保仅当"运行方式"用户，即，运行任务的用户帐户登录到计算机时运行任务。

该命令使用 **/tn**参数来标识该任务并 **/it**参数，将仅交互属性添加到任务。 任务已使用我的用户帐户的权限运行，因为我不需要更改 **/ru**任务参数。
```
schtasks /change /tn MyApp /it
```
在响应中， **SchTasks.exe**会显示以下成功消息。
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>schtasks 运行

立即启动计划的任务。 **运行**操作忽略计划，但使用的程序文件位置、 用户帐户和密码保存在任务中来立即运行该任务。

### <a name="syntax"></a>语法

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|术语|定义|
|----|----------|
|/tn \<TaskName >|必需。 标识的任务。|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （有或没有反斜杠）。 默认值为本地计算机。|
|/u [\<域 >\]<User>|使用指定的用户帐户的权限运行此命令。 默认情况下，该命令运行的本地计算机的当前用户的权限。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**并 **/p**参数都有效只能使用 **/s**。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。 如果您使用 **/u**参数，但省略 **/p**参数或在 password 参数， **schtasks**提示你输入密码。</br>**/U**并 **/p**参数都有效只能使用 **/s**。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   此操作用于测试你的任务。 如果任务不运行，请检查任务计划程序服务事务日志， \<Systemroot > \SchedLgU.txt，是否有错误。
-   运行任务不会影响任务计划并不会更改计划的任务的下次运行时间。
-   若要远程运行的任务，必须在远程计算机上计划任务。 当你运行它时，任务将运行只能在远程计算机上。 若要确认远程计算机上正在运行一个任务，请使用任务管理器或任务计划程序事务日志， \<Systemroot > \SchedLgU.txt。

### <a name="examples"></a>示例

### <a name="to-run-a-task-on-the-local-computer"></a>若要在本地计算机上运行任务

以下命令启动"安全脚本"任务。
```
schtasks /run /tn "Security Script"
```
在响应中， **SchTasks.exe**启动与任务关联的脚本并显示以下消息：
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
正如所示的消息， **schtasks**尝试启动程序，但它不能非常实际启动该程序。

### <a name="to-run-a-task-on-a-remote-computer"></a>若要在远程计算机上运行任务

以下命令启动的远程计算机上，Svr01 更新任务：
```
schtasks /run /tn Update /s Svr01
```
在这种情况下， **SchTasks.exe**显示以下错误消息：
```
ERROR: Unable to run the scheduled task "Update".
```
若要查找错误的原因，请查看计划任务的事务日志，C:\Windows\SchedLgU.txt Svr01 上。 在这种情况下，在日志中将显示以下条目：
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.

```
显然，用户名或任务中的密码不是在系统上有效。 以下**schtasks /change**命令将更新的用户名和密码更新任务 Svr01 上：
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
之后**更改**命令完成后，**运行**重复命令。 这一次，Update.exe 程序启动并**SchTasks.exe**会显示以下消息：
```
SUCCESS: Attempted to run the scheduled task "Update".
```
正如所示的消息， **schtasks**尝试启动程序，但它不能非常实际启动该程序。

## <a name="BKMK_end"></a>schtasks 结束

停止由任务启动的程序。

### <a name="syntax"></a>语法

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|术语|定义|
|----|----------|
|/tn \<TaskName >|必需。 标识启动计划的任务。|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址。 默认值为本地计算机。|
|/u [\<域 >\]<User>|使用指定的用户帐户的权限运行此命令。 默认情况下，该命令运行的本地计算机的当前用户的权限。 指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**并 **/p**参数都有效只能使用 **/s**。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。 如果您使用 **/u**参数，但省略 **/p**参数或在 password 参数， **schtasks**提示你输入密码。</br>**/U**并 **/p**参数都有效只能使用 **/s**。|
|/?|显示帮助。|

### <a name="remarks"></a>备注

**SchTasks.exe**结束仅由计划任务启动的程序的实例。 若要停止其他进程，可以使用 TaskKill。 有关详细信息，请参阅[Taskkill](taskkill.md)。

### <a name="examples"></a>示例

### <a name="to-end-a-task-on-a-local-computer"></a>若要结束本地计算机上的任务

以下命令停止已由我记事本任务启动 Notepad.exe 的实例：
```
schtasks /end /tn "My Notepad"
```
在响应中， **SchTasks.exe**停止 Notepad.exe 启动任务，并显示以下成功消息的实例：
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>若要结束远程计算机上的任务

以下命令停止在远程计算机上，Svr01 InternetOn 任务已启动的 Internet Explorer 的实例：
```
schtasks /end /tn InternetOn /s Svr01
```
在响应中， **SchTasks.exe**停止的任务已开始，并显示以下成功消息的 Internet Explorer 实例：
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks delete

删除计划的任务。

### <a name="syntax"></a>语法

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|术语|定义|
|----|----------|
|/tn {\<TaskName >| *}|必需。 标识要删除的任务。</br>**\<任务名 >** -删除命名的任务。</br>**<\*>** -删除计算机上的所有计划的任务。|
|/f|禁止显示确认消息。 此任务是已删除而不发出警告。|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （有或没有反斜杠）。 默认值为本地计算机。|
|/u [\<域 >\]<User>|使用指定的用户帐户的权限运行此命令。 默认情况下，该命令运行的本地计算机的当前用户的权限。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**并 **/p**参数都有效只能使用 **/s**。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。 如果您使用 **/u**参数，但省略 **/p**参数或在 password 参数， **schtasks**提示你输入密码。</br>**/U**并 **/p**参数都有效只能使用 **/s**。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   **删除**操作从计划中删除该任务。 它不删除该任务运行的程序或中断正在运行的程序。
-   **删除\*** 命令将删除为该计算机，计划的所有任务而不仅仅是由当前用户计划的任务。

### <a name="examples"></a>示例

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>若要从远程计算机的计划中删除任务

以下命令从远程计算机的计划中删除"启动邮件"任务。 它使用 **/s**参数来标识在远程计算机。
```
schtasks /delete /tn "Start Mail" /s Svr16
```
在响应中， **SchTasks.exe**显示以下确认消息。 若要删除的任务，请按 Y **。** 若要取消该命令，请键入**n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>若要删除计划为本地计算机的所有任务

以下命令从本地计算机，包括由其他用户计划的任务的计划中删除所有任务。 它使用 **/tn \*** 参数以表示在计算机上的所有任务和 **/f**参数来禁止显示确认消息。
```
schtasks /delete /tn * /f
```
在响应中， **SchTasks.exe**显示指出的唯一任务计划，然后 SecureScript，将删除以下成功消息。

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>schtasks 查询

显示计划的计算机上运行的任务。

### <a name="syntax"></a>语法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parameters

|术语|定义|
|----|----------|
|[/query]|操作名称是可选的。 键入**schtasks**没有任何参数的方式执行查询。|
|/fo {TABLE| 列表| CSV}|指定的输出格式。 **表**是默认值。|
|/nh|省略表格显示中的列标题。 此参数才有效，且**表**并**CSV**输出格式。|
|/v|将任务的高级的属性添加到显示。</br>使用的查询 **/v**的格式应为**列表**或**CSV**。|
|/s\<计算机 >|指定的名称或远程计算机的 IP 地址 （有或没有反斜杠）。 默认值为本地计算机。|
|/u [\<域 >\]<User>|使用指定的用户帐户的权限运行此命令。 默认情况下，该命令运行的本地计算机的当前用户的权限。</br>指定的用户帐户必须是远程计算机上 Administrators 组的成员。 **/U**并 **/p**参数都有效只能使用 **/s**。|
|/p \<Password>|指定在指定的用户帐户的密码 **/u**参数。 如果您使用 **/u**，但省略 **/p**或密码参数**schtasks**提示你输入密码。</br>**/U**并 **/p**参数都有效只能使用 **/s**。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

**SchTasks.exe**结束仅由计划任务启动的程序的实例。 若要停止其他进程，可以使用 TaskKill。 有关详细信息，请参阅[Taskkill](taskkill.md)。

### <a name="examples"></a>示例

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>若要在本地计算机上显示计划的任务

下面的命令显示本地计算机为计划的所有任务。 这些命令生成相同的结果，并且可以互换使用。
```
schtasks
schtasks /query
```
在响应中， **SchTasks.exe**中默认情况下，简单的表格格式显示的任务下, 表中所示：
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001

```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>若要显示的计划任务的高级的属性

以下命令请求在本地计算机上的任务的详细的显示。 它使用 **/v**参数来请求 （详细） 的详细的显示和 **/fo 列表**参数设置为列表以供轻松阅读的格式显示。 此命令可用于验证你创建的任务具有预期的重复执行模式。

**schtasks /query /fo 列表 /v**

在响应中， **SchTasks.exe**显示所有任务的详细的属性列表。 显示在下面的显示计划在早晨 4:00 运行的任务的任务列表 在每个月的最后一个星期五：
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

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>若要记录远程计算机为计划的任务

以下命令请求的远程计算机，为计划的任务列表，并将任务添加到本地计算机上的以逗号分隔日志文件。 此命令格式可用于收集和跟踪为多台计算机计划的任务。

该命令使用 **/s**参数来标识远程计算机，Reskit16， **/fo**参数指定的格式并 **/nh**参数来禁止显示列标题。 **>>** 将符号重定向输出追加到该任务的日志，p0102.csv 的本地计算机上，Svr01。 在远程计算机上运行该命令，因为本地计算机路径必须是完全限定。
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
在响应中， **SchTasks.exe**将添加到在本地计算机上，Svr01 p0102.csv 文件 Reskit16 计算机计划的任务。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

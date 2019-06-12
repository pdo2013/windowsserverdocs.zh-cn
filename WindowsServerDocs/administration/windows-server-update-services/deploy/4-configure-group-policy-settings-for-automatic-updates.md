---
title: 第 4 步-配置为自动更新的组策略设置
description: Windows Server Update Service (WSUS) 主题-自动更新配置组策略设置是在四个步骤中部署 WSUS 的第四步
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9205565486b75edcd550174fc89990a5aa2d69b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439853"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>步骤 4：配置为自动更新的组策略设置

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在 active directory 环境中，可以使用组策略来定义如何计算机和用户 （称为本文档中将 WSUS 客户端） 可以与 Windows 更新获取自动更新从 Windows Server Update Services (WSUS) 交互。

本主题包含两个主要部分：

[WSUS 客户端的更新的组策略设置](#group-policy-settings-for-wsus-client-updates)，其中提供了规范性指南和行为有关的详细信息组策略的 Windows 更新和维护计划程序设置该 WSUS 客户端可以与 Windows 更新中进行交互的控件若要获取自动更新。

[补充信息](#supplemental-information)包含以下部分：

-   [访问组策略中的 Windows 更新设置](#accessing-the-windows-update-settings-in-group-policy)，其中提供了有关使用组策略管理编辑器和访问的更新服务策略扩展和维护计划程序中的设置信息的常规指南组策略。

-   [将更改为 WSUS 与本指南相关](#changes-to-wsus-relevant-to-this-guide)： 对于管理员熟悉 WSUS 3.2 及更低版本，此部分可为 WSUS 与本指南相关的当前和过去版本之间的主要差异的简短摘要。

-   [术语和定义](#terms-and-definitions)： 与本指南中使用的 WSUS 和更新服务相关的各个术语的定义。

## <a name="group-policy-settings-for-wsus-client-updates"></a>WSUS 客户端的更新的组策略设置
本部分提供的信息就以下三个扩展的组策略。 在这些扩展将找到可用于配置 WSUS 客户端与 Windows 更新接收自动更新的交互方式的设置。

-   [计算机配置&gt;Windows 更新策略设置](#computer-configuration--windows-update-policy-settings)

-   [计算机配置&gt;维护计划程序策略设置](#computer-configuration--maintenance-scheduler-policy-settings)

-   [用户配置&gt;Windows 更新策略设置](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> 本主题假定你已使用并熟悉组策略。 如果您不熟悉组策略，因此建议您查看中的信息[的补充信息](#supplemental-information)然后再尝试配置 WSUS 的策略设置本文档的部分。

### <a name="computer-configuration--windows-update-policy-settings"></a>计算机配置 > Windows 更新策略设置
本部分提供有关以下基于计算机的策略设置的详细信息：

-   [允许自动更新立即安装](#allow-automatic-updates-immediate-installation)

-   [允许非管理员会收到更新通知](#allow-non-administrators-to-receive-update-notifications)

-   [允许来自 intranet Microsoft 更新服务位置的签名的更新](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [自动更新检测频率](#automatic-updates-detection-frequency)

-   [配置自动更新](#configure-automatic-updates)

-   [计划安装的的延迟重新启动](#delay-restart-for-scheduled-installations)

-   [不调整为"安装更新并关机"关闭 Windows 对话框中的默认选项](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [不要关闭 Windows 对话框中显示"安装更新并关机"选项](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [允许客户端的目标](#enable-client-side-targeting)

-   [启用 Windows Update 电源管理以自动唤醒计算机以安装计划的更新](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [不为已登录用户的计划自动重新启动更新安装](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [重新提示重启，并计划安装](#re-prompt-for-restart-with-scheduled-installations)

-   [重新计划自动更新计划的安装](#reschedule-automatic-updates-scheduled-installations)

-   [指定 intranet Microsoft 更新服务位置](#specify-intranet-microsoft-update-service-location)

-   [启用通过自动更新的建议更新](#turn-on-recommended-updates-via-automatic-updates)

-   [打开软件通知](#turn-on-software-notifications)

在 GPME 中，为基于计算机的配置的 Windows 更新策略位于路径中：*PolicyName* > **计算机配置** > **策略** > **管理模板** > **Windows 组件** > **Windows Update**。

> [!NOTE]
> 默认情况下，不配置这些设置。

#### <a name="allow-automatic-updates-immediate-installation"></a>允许自动更新立即安装
指定是否自动更新将自动安装更新不会中断 Windows 服务或重新启动 Windows。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果"配置自动更新"策略设置设置为**禁用**，此策略不起作用。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定不会立即安装更新。 本地管理员可以使用本地组策略编辑器更改此设置。|
|**Enabled**|指定自动更新立即安装更新，会将已下载并准备好进行安装。|
|**已禁用**|指定不会立即安装更新。|

**选项：** 没有为此设置的选项。

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>允许非管理员会收到更新通知
指定是否非管理用户将收到更新通知基于配置自动更新策略设置。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|请参阅下表中的详细信息。|

> [!NOTE]
> 如果"配置自动更新"策略设置已禁用或未配置，则此策略设置无效。

> [!IMPORTANT]
> 从开始 Windows 8 和 Windows RT 中，默认情况下启用此策略设置。 在所有以前版本的 Windows，它是默认情况下禁用。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定用户将始终看到帐户控制窗口和需要提升的权限来执行这些任务。 本地管理员可以通过使用本地组策略编辑器更改此设置。|
|**Enabled**|指定 Windows 自动更新和 Microsoft Update 将包含非管理员，确定哪个登录的用户将收到更新通知时。 非管理用户将无法安装他们接收到通知的所有可选、 建议和重要更新内容。 用户不会看到一个用户帐户控制窗口中，并且他们不需要提升的权限才能安装这些更新，除非在包含用户界面、 最终用户许可协议或 Windows Update 设置更改的更新的情况下。<br /><br />有两种情况下，此设置生效则取决于操作系统的计算机:<br /><br />1.**隐藏**或**还原**更新<br />2.**取消**更新安装<br /><br />在 Windows Vista 或 Windows XP 中，如果启用此策略设置，用户不会看到一个用户帐户控制窗口中，并且他们不需要提升的权限来隐藏、 还原或取消的更新。<br /><br />在 Windows Vista 中，如果启用此策略设置，用户不会看到一个用户帐户控制窗口中，并且他们不需要提升的权限来隐藏、 还原或取消的更新。 如果未启用此策略设置，用户将始终看到帐户控制窗口中，并且需要提升的权限来隐藏、 还原或取消的更新。<br /><br />在 Windows 7 中，此策略设置不起。 用户将始终看到帐户控制窗口中，并且需要提升的权限来执行这些任务。<br /><br />在 Windows 8 和 Windows RT 中，此策略设置不起。|
|**已禁用**|指定仅登录的管理员收到更新通知。 **注意：** 在 Windows 8 和 Windows RT 中，默认情况下启用此策略设置。 在所有以前版本的 Windows，它是默认情况下禁用。|

**选项：** 没有为此设置的选项。

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>允许使用来自 Intranet Microsoft 更新服务位置的签名更新
指定是否自动更新接受 intranet Microsoft 更新服务位置上找到更新时，不是 Microsoft 的实体进行签名的更新。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 来自 intranet Microsoft 更新服务以外的服务的更新必须始终由 Microsoft 进行签名和不受此策略设置。

> [!NOTE]
> 此策略不支持在 Windows rt。 如果启用此策略将不能在运行 Windows RT 的计算机上的任何作用

**选项：** 没有为此设置的选项。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定来自 intranet Microsoft 更新服务位置的更新必须由 Microsoft 进行签名。|
|**Enabled**|指定自动更新接受收到通过 intranet Microsoft 更新服务位置，如果它们由本地计算机的"受信任的发行者"证书存储中找到的证书签名的更新。|
|**已禁用**|指定来自 intranet Microsoft 更新服务位置的更新必须由 Microsoft 进行签名。|

**选项：** 没有为此设置的选项。

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>始终在计划的时间自动重启
指定是否重新启动计时器始终将 Windows Update 安装重要更新，而不是在至少两天的登录屏幕上的第一个通知用户之后立即开始。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果启用了"不为已登录用户的计划自动重新启动更新安装"策略设置，则此策略无效。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定 Windows 更新不会更改计算机的重新启动行为。|
|**Enabled**|指定 Windows Update 安装重要更新，而不是在至少两天的登录屏幕上的第一个通知用户之后立即将始终以重新启动计时器。<br /><br />可以配置重新启动计时器开始从 15 到 180 分钟的任何值。 当计时器超时时，在重新启动将继续进行，即使计算机具有登录的用户。|
|**已禁用**|指定 Windows 更新不会更改计算机的重新启动行为。|

**选项：** 如果启用此设置，则可以指定之前发生强制的计算机重启安装更新后经过的时间量。

#### <a name="automatic-updates-detection-frequency"></a>自动更新检测频率
指定 Windows 将用于确定在检查可用更新前等待的时长的小时数。 使用此处指定的小时数减去指定小时数的 0% 到 20% 来确定准确的等待时间。 例如，如果此策略用于指定 20 小时检测频率，此策略应用到的所有客户端将检查 16 到 20 个小时之间的任意位置的更新。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 必须启用此策略生效的"指定 intranet Microsoft 更新服务位置"设置。
>
> 如果禁用了"配置自动更新"策略设置，则此策略无效。

> [!NOTE]
> 此策略不支持在 Windows rt。 如果启用此策略将不能在运行 Windows RT 的计算机上的任何作用

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定 Windows 将检查的 22 小时的默认间隔的可用更新。|
|**Enabled**|指定 Windows 将检查的在指定的时间间隔的可用更新。|
|**已禁用**|指定 Windows 将检查的 22 小时的默认间隔的可用更新。|

**选项：** 如果启用此设置，则可以指定 Windows 更新检查更新之前等待的时间间隔 （以小时为单位）。

#### <a name="configure-automatic-updates"></a>配置自动更新
指定指定是否在此计算机上启用自动更新。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

如果启用，则必须选择一个此组策略设置中提供的四个选项。

若要使用此设置，请选择**已启用**，然后在**选项**下**配置自动更新**，选择一个选项 （2、 3、 4 或 5）。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定自动更新使用未指定在组策略级别。 但是，计算机管理员仍可以在控制面板中配置自动更新。|
|**Enabled**|指定 Windows 识别时在计算机处于联机状态，并使用其 Internet 连接的可用更新搜索 Windows Update。<br /><br />启用时，将允许本地管理员使用 Windows Update 控制面板选择所选的配置选项。 但是，本地管理员将不能禁用自动更新的配置。<br /><br />-   **2-下载通知和通知安装**<br />    Windows 更新类型可以查找应用于此计算机的更新，用户将收到通知准备好下载更新。 然后，用户可以运行 Windows 更新来下载和安装可用更新。<br />-   **3-自动下载和通知安装**（默认设置）<br />    Windows Update 查找适用的更新，并将它们下载在后台;用户未收到通知或在过程中中断。 下载完成后，通知用户有更新准备好进行安装。 然后，用户可以运行 Windows 更新安装已下载的更新。<br />-   **4-自动下载并计划安装**<br />    可以通过使用选项在此组策略设置中指定的计划。 如果指定无计划，则所有安装的默认计划将每隔一天的凌晨 3:00 如果更新要求重新启动才能完成安装，Windows 将自动重新在计算机启动。 （如果用户登录到计算机准备好重新启动 Windows 时，用户将会收到通知并选择延迟重新启动。）**注意：** 自 Windows 8，可以设置而不是使用特定计划绑定到 Windows 更新自动维护过程中要安装的更新。 自动维护的当计算机不在使用中，安装更新，并避免当计算机使用电池电源运行时安装更新。 如果自动维护程序无法安装天内的更新，Windows 更新将立即安装更新。 挂起的重新启动，然后将通知用户。 如果没有任何可能发生了意外的数据丢失，挂起的重启只会进行。    可以在 GPME 维护计划程序设置中，位于路径中，指定计划选项*PolicyName* > **计算机配置** >  **策略** > **管理模板** > **Windows 组件** > **维护计划程序** > **自动维护激活边界**。 请参阅标题为本参考部分：[维护计划程序设置](#computer-configuration--maintenance-scheduler-policy-settings)，用于设置的详细信息。    **5-允许本地管理员选择设置**<br />-指定是否允许本地管理员使用自动更新控制面板来选择所选的配置选项，例如，无论本地管理员可以选择计划的安装时间。<br />    不允许本地管理员禁用自动更新配置。|
|**已禁用**|指定可从公共 Windows 更新服务的任何客户端更新，必须手动从 Internet 下载并安装。|

#### <a name="delay-restart-for-scheduled-installations"></a>计划安装的的延迟重新启动
指定自动更新在继续进行计划的重新启动之前要等待时间的量。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略仅适用于自动更新配置为执行计划的安装更新。 如果禁用了"配置自动更新"策略设置，则此策略无效。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定安装更新后，默认等待时间是 15 分钟会经过任何计划的重新启动之前。|
|**Enabled**|指定在安装完成后，计划的重新启动会发生后指定的分钟数已过期。|
|**已禁用**|指定安装更新后，默认等待时间是 15 分钟会经过任何计划的重新启动之前。|

**选项：** 如果启用此设置，可以使用此选项以指定在继续进行计划的重新启动之前等待的时间 （以分钟为单位） 自动更新。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>不调整安装更新并关机关闭 Windows 对话框中的默认选项
此策略设置，可指定是否**安装更新并关机**中的默认选项为允许使用选项**关闭 Windows**对话框。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略设置没有任何影响，如果*PolicyName* > **计算机配置** > **策略** >  **管理模板** > **Windows 组件** > **Windows Update** > **不会显示"安装更新并关机"对话框中的选项，关闭 Windows**启用策略设置。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定的**安装更新并关机**将中的默认选项**关闭 Windows**是否可用于在用户选择为关机选项时安装更新对话框关闭计算机。|
|**Enabled**|如果启用此策略设置，用户的最后一个关机的选项 （例如，休眠或重新启动） 是中的默认选项**关闭 Windows**对话框中的，而不管是否**安装更新并关机**选项现已推出**希望计算机做什么？** 菜单。|
|**已禁用**|指定的**安装更新并关机**将中的默认选项**关闭 Windows**是否可用于在用户选择为关机选项时安装更新对话框关闭计算机。|

**选项：** 没有为此设置的选项。

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>不要连接任何 Windows 更新 Internet 位置
甚至当 Windows 更新配置为从 intranet 更新服务接收更新，它会定期检索信息从公共的 Windows 更新服务，以便将来连接到 Windows 更新和其他服务，例如 Microsoft 更新或 Microsoft Store。

如果启用此策略将禁用定期从公共的 Windows Server 更新服务中检索信息的功能，它可能会导致连接到公共服务，如 Microsoft Store 停止工作。

|支持：|不包括：|
|---------|-------|
|从 Windows Server 2012 R2、 Windows 8.1 或 Windows RT 8.1，Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略适用仅当计算机配置为连接到 intranet 更新服务通过使用"指定 intranet Microsoft 更新服务位置"策略设置。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|要从公共 Windows Server 更新服务检索信息的默认行为仍然存在。|
|**Enabled**|指定计算机将从公共 Windows Server 更新服务检索信息。|
|**已禁用**|要从公共 Windows Server 更新服务检索信息的默认行为仍然存在。|

**选项：** 没有为此设置的选项。

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>不要关闭 Windows 对话框中显示安装更新和关机选项
指定是否**安装更新并关机**中显示选项**关闭 Windows**对话框。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定的**安装更新并关机**选项位于**关闭 Windows**对话框中，如果用户选择关机选项来关闭计算机时有可用更新。 本地管理员可以通过使用本地策略中更改此设置。|
|**Enabled**|指定的**安装更新并关机**不会作为中的选项**关闭 Windows**对话框中，即使当用户选择为关机选项时，有可用于安装更新关闭计算机。|
|**已禁用**|指定的**安装更新并关机**选项将中的默认选项**关闭 Windows**是否可用于在用户选择关闭的时间安装更新对话框若要关闭计算机的选项。|

**选项：** 没有为此设置的选项。

#### <a name="enable-client-side-targeting"></a>启用客户端定位
指定的目标组名称或在 WSUS 控制台中配置要从 WSUS 接收更新的名称。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!NOTE]
> 此策略仅适用于此计算机配置为在 WSUS 中支持指定的目标组名称。 如果在 WSUS 中不存在目标组名称，将被忽略，直到创建它。 如果禁用或未配置"指定 intranet Microsoft 更新服务位置"策略设置，则此策略无效。

> [!NOTE]
> 此策略不支持在 Windows rt。 如果启用此策略将不能在运行 Windows RT 的计算机上的任何作用

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定的任何目标组信息已发送到 WSUS。 本地管理员可以通过使用本地策略中更改此设置。|
|**Enabled**|指定指定的目标组信息发送到使用它来确定应将哪些更新部署到此计算机的 WSUS。 如果 WSUS 支持多个目标组，可以使用此策略指定多个组名称，用分号分隔，如果你在 WSUS 的计算机组列表中添加的目标组名称。 否则，必须指定单个组。|
|**已禁用**|指定的任何目标组信息已发送到 WSUS。|

**选项：** 使用此空间指定一个或多个目标组名称。

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>启用 Windows Update 电源管理以自动唤醒计算机以安装计划的更新
指定 Windows 更新是否将使用 Windows 电源管理或电源选项功能以自动唤醒计算机从休眠状态，如果计划安装的更新。

仅当 Windows 更新配置为自动安装更新，将自动唤醒计算机。 如果计算机处于休眠状态，当发生的计划的安装时间，而应用更新时，Windows 更新将使用 Windows 电源管理或电源选项功能以自动唤醒计算机以安装更新。 Windows 更新还将唤醒计算机并安装更新，如果采用安装截止时间发生。

除非安装更新，否则，将不会唤醒计算机。 如果计算机是电池供电时 Windows 更新将其唤醒，则不会安装更新，计算机将自动，返回到休眠状态以两分钟为单位。

|支持：|不包括：|
|---------|-------|
|从 Windows Vista 和 Windows Server 2008 (Windows 7)，Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|Windows 更新无法唤醒计算机从休眠状态以安装更新。 本地管理员可以通过使用本地策略中更改此设置。|
|**Enabled**|Windows 更新将计算机从休眠状态以安装更新之前列出的条件下唤醒。|
|**已禁用**|Windows 更新无法唤醒计算机从休眠状态以安装更新。|

**选项：** 没有为此设置的选项。

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>对于有已登录用户的计算机，计划的自动更新安装不执行自动重启
指定要完成计划的安装，自动更新将等待的任何用户都已登录，而不是导致计算机重新启动自动重新启动计算机。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略仅适用于自动更新配置为执行计划的安装更新。 如果禁用了"配置自动更新"策略设置，则此策略无效。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定自动更新会通知用户计算机将自动重新在五分钟时间才能完成安装。|
|**Enabled**|某些更新需要更新才能生效，必须重新启动计算机。 如果状态设置为已启用，自动更新将不自动重新启动计算机在计划安装过程中如果用户登录到计算机。 相反，自动更新会通知用户重新启动计算机。|
|**已禁用**|指定自动更新会通知用户计算机将自动重新在五分钟时间才能完成安装。|

**选项：** 没有为此设置的选项。

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>对计划的安装再次提示重启
指定自动更新会再次提示与计划重新启动之前要等待的时间量。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows RT|

> [!IMPORTANT]
> 此策略仅适用于自动更新配置为执行计划的安装更新。 如果禁用了"配置自动更新"策略设置，则此策略无效。

> [!NOTE]
> 此策略将在运行 Windows RT 的计算机上不起作用

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|计划发生重启十分钟的提示重新启动后关闭消息。 本地管理员可以通过使用本地策略中更改此设置。|
|**Enabled**|指定推迟重启的上一提示后，计划的重新启动会发生分钟经历了指定次数后。|
|**已禁用**|计划发生重启十分钟的提示重新启动后关闭消息。|

**选项：** 启用时，可以使用此设置选项以指定 （以分钟为单位） 的系统会提示用户重新计划重新启动前经过的时间的持续时间。

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>重新计划自动更新计划的安装
指定自动更新以下计算机启动时，以前未完成的计划安装继续之前等待的时间量。

如果将状态设置为**未配置**，丢失计划的安装会发生一分钟后下, 一步是在计算机启动。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略仅适用于自动更新配置为执行计划的安装更新。 如果禁用了"配置自动更新"策略设置，则此策略无效。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定错过了计划的安装将出现一分钟后下, 一步是在计算机启动。|
|**Enabled**|指定计划的安装的未发生之前将发生指定的下一次启动计算机后的分钟数。|
|**已禁用**|指定错过了计划的安装将出现在下一步计划安装。|

**选项：** 启用此策略设置后，可以使用它来指定在数分钟后接下来启动计算机，未采用的计划的安装前面放置会发生。

#### <a name="specify-intranet-microsoft-update-service-location"></a>指定 Intranet Microsoft 更新服务位置
指定用于托管来自 Microsoft 更新的更新的 Intranet 服务器。 然后，您可以使用 WSUS 以自动更新的计算机在网络上。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|Windows rt。|

此设置，可指定将用作内部更新服务在网络上的 WSUS 服务器。 而不是在 Internet 上使用 Microsoft 更新，WSUS 客户端将搜索此服务中的应用的更新。

若要使用此设置，必须设置两个服务器名称值： 服务器从其客户端检测到并下载的更新，并向其上传更新后的工作站，统计信息的服务器。 值不需要不同，如果在同一服务器上配置了这两个服务。

> [!NOTE]
> 如果禁用了"配置自动更新"策略设置，则此策略无效。

> [!NOTE]
> 此策略不支持在 Windows rt。 如果启用此策略将不能在运行 Windows RT 的计算机上的任何作用

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|如果自动更新不禁用了策略或用户首选项，此策略指定客户端连接直接访问 Internet 上的 Windows 更新站点。|
|**Enabled**|指定客户端连接到指定的 WSUS 服务器，而不是 Windows 更新来搜索并下载更新。 启用此设置意味着你的组织中的最终用户无需通过防火墙来获取更新，请转，它使您能够测试前部署这些更新。|
|**已禁用**|如果自动更新不禁用了策略或用户首选项，此策略指定客户端连接直接访问 Internet 上的 Windows 更新站点。|

**选项：** 启用此策略设置后，必须指定的 intranet 更新服务时检测更新，WSUS 客户端将使用和 Internet 统计服务器向其更新 WSUS 客户端将上传的统计信息。 示例值：


|                    设置选项：                    |    示例值：    |
|-------------------------------------------------------|----------------------|
| 设置检测更新的 intranet 更新服务 |  http://wsus01:8530  |
|          设置 intranet 统计服务器           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>启用通过自动更新的建议更新
指定是否自动更新将提供重要信息和建议从 WSUS 的更新。

|支持：|不包括：|
|---------|-------|
|启动 Windows Vista，Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定自动更新将继续提供重要更新，如果已配置为执行此操作。|
|**Enabled**|指定自动更新将安装推荐的更新和重要更新从 WSUS。|
|**已禁用**|指定自动更新将继续提供重要更新，如果已配置为执行此操作。|

**选项：** 没有为此设置的选项。

#### <a name="turn-on-software-notifications"></a>打开软件通知
此策略设置，可以控制是否用户可查看有关从 Microsoft 更新服务功能完备的软件的详细增强的通知消息。 增强的通知消息带来的价值，并将提升的安装和使用的可选软件。 此策略设置用于在松散管理的环境，从而允许对 Microsoft Update 服务的最终用户访问。

如果不使用 Microsoft Update 服务，则"软件通知"策略设置无效。

如果"配置自动更新"策略设置已禁用或未配置，则"软件通知"策略设置无效。

|支持：|不包括：|
|---------|-------|
|从 Windows Server 2008 (Windows Vista) 和 Windows 7，Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 默认情况下禁用此策略设置。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|运行的 Windows 7 的计算机上的用户不会提供可选的应用程序的消息。 运行的 Windows Vista 的计算机上的用户不会提供可选应用程序或更新的消息。 本地管理员可以通过使用控制面板或本地策略中更改此设置。|
|**Enabled**|如果启用此策略设置，一条通知消息将显示推荐的用户的计算机上的软件可。 用户可以单击通知以打开 Windows 更新并获取有关软件的详细信息或将其安装。 用户还可以单击**关闭此消息**或**向我显示更高版本**延迟作为相应通知。<br /><br />在 Windows 7 中，此策略设置仅控制可选应用程序的详细的通知。 在 Windows Vista 中，此策略设置控制可选应用程序和更新的详细的的通知。|
|**已禁用**|指定运行 Windows 7 的用户将不会提供详细的通知消息的可选应用程序，并运行 Windows Vista 的用户将不会提供可选的应用程序或可选更新的详细的通知消息。|

**选项：** 没有为此设置的选项。

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>计算机配置 > 维护计划程序策略设置
在配置自动更新设置中，选择了选项**4-自动下载并计划安装**，可以指定计划运行 Windows 8 和 Windows RT 的计算机在 GPMC 中的维护计划程序设置 如果未在"配置自动更新"设置中选择选项 4，您不需要配置用于自动更新这些设置。 维护计划程序设置路径中的位置：*PolicyName* > **计算机配置** > **策略** > **管理模板** > **Windows 组件** > **维护计划程序**。 组策略的维护计划程序扩展包含以下设置：

-   [自动维护激活边界](#automatic-maintenance-activation-boundary)

-   [自动维护随机延迟](#automatic-maintenance-random-delay)

-   [自动唤醒策略](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>自动维护激活边界
此策略可用于配置"自动维护激活边界"设置。

维护激活边界是自动维护开始每日计划的时间。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此设置与中的选项 4**配置自动更新**。 如果未选择选项 4 中的**配置自动更新**，不需要配置此设置。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|如果未配置此策略设置，每日计划中的客户端计算机上指定的时间**操作中心** > **自动维护**控制面板将应用。|
|**Enabled**|启用此策略设置将覆盖任何默认值或修改客户端计算机上配置的设置**Control Panel** > **操作中心** >  **自动维护**(或在某些客户端版本中，**维护**)。|
|**已禁用**|如果将此策略设置设置为**已禁用**、 每日计划的时间中的规定**操作中心** > **自动维护**，在控件中面板将应用。|

#### <a name="automatic-maintenance-random-delay"></a>自动维护随机延迟
此策略设置允许您配置自动维护激活随机延迟。

维护随机延迟是时间的之前，自动维护将延迟从其激活边界开始量。 此设置可用于虚拟机随机维护可能性能要求。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此设置与中的选项 4**配置自动更新**。 如果未选择选项 4 中的**配置自动更新**，不需要配置此设置。

默认情况下，启用时，常规维护随机延迟设置为**PT4H**。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|四小时随机延迟应用于**自动**。|
|**Enabled**|自动维护将延迟到指定的时间内通过其激活边界从开始。|
|**已禁用**|不随机延迟将应用于自动维护。|

#### <a name="automatic-wakeup-policy"></a>自动唤醒策略
此策略设置允许你配置的自动维护唤醒策略。

维护唤醒策略指定是否自动维护来建立与每日计划的维护操作计算机的唤醒请求。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 如果运行的计算机的电源唤醒显式禁用策略，此设置不起作用。

> [!NOTE]
> 此设置与中的选项 4**配置自动更新**。 如果未选择选项 4 中的**配置自动更新**，不需要配置此设置。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|如果不配置此策略设置，在中指定设置为唤醒**操作中心** > **自动维护**控制面板将应用。|
|**Enabled**|如果启用此策略设置，自动维护将尝试设置操作系统唤醒策略并为每日计划的时间，使唤醒请求必要。|
|**已禁用**|如果禁用此策略设置，在中指定设置为唤醒**操作中心** > **自动维护**控制面板将应用。|

### <a name="user-configuration--windows-update-policy-settings"></a>用户配置 > Windows 更新策略设置
本部分提供有关以下基于用户的策略设置的详细信息：

-   [无法在关闭 Windows 对话框中显示"安装更新并关机"选项](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [在对话框中关闭 Windows 不调整为"安装更新并关机"的默认选项](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [删除使用所有 Windows 更新功能的访问权限](#remove-access-to-use-all-windows-update-features)

在 GPMC 中，路径中的位置自动计算机更新的用户设置：*PolicyName* > **用户配置** > **策略** > **管理模板** >  **Windows 组件** > **Windows 更新**。 出现在计算机配置和用户配置组策略中的扩展插件的相同顺序列出的设置时**设置**Windows 更新策略选项卡上选择进行排序设置按字母顺序。

> [!NOTE]
> 默认情况下，除非另行说明，否则不是配置这些设置。

> [!TIP]
> 对于每个这些设置，可以使用以下步骤来启用、 禁用或设置之间导航：

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>无法在关闭 Windows 对话框中显示安装更新并关机选项
指定是否**安装更新并关机**中显示选项**关闭 Windows**对话框。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定的**安装更新并关机**选项将出现在**关闭 Windows**对话框中，如果用户选择关机选项来关闭计算机时有可用更新。|
|**Enabled**|如果启用此策略设置，**安装更新并关机**不会作为中的选项**关闭 Windows**对话框中，即使当用户选择时，有可用于安装更新关闭选项来关闭计算机。|
|**已禁用**|指定的**安装更新并关机**选项将出现在**关闭 Windows**对话框中，如果用户选择关机选项来关闭计算机时有可用更新。|

**选项：** 没有为此设置的选项。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>在对话框中关闭 Windows 不调整为"安装更新并关机"的默认选项
指定是否**安装更新并关机**中的默认选项为允许使用选项**关闭 Windows**对话框。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 此策略设置没有任何影响，如果*PolicyName* > **用户配置** > **策略** >  **管理模板** > **Windows 组件** > **Windows Update** > **不会显示"关闭 Windows 对话框中的安装更新并关机"选项**已启用。

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|指定是否**安装更新并关机**选项将中的默认选项**关闭 Windows**是否可用于在用户选择关闭的时间安装更新对话框向下关闭计算机的选项。|
|**Enabled**|指定用户的最后一个关机的选项 （例如，休眠或重新启动） 是中的默认选项**关闭 Windows**对话框中的，而不管是否**安装更新并关机选项**现已推出**希望计算机做什么？** 菜单。|
|**已禁用**|指定是否**安装更新并关机**选项将中的默认选项**关闭 Windows**是否可用于在用户选择关闭的时间安装更新对话框向下关闭计算机的选项。|

**选项：** 没有为此设置的选项。

#### <a name="remove-access-to-use-all-windows-update-features"></a>删除使用所有 Windows 更新功能的访问权限
此设置，可删除对 Windows 更新 WSUS 客户端访问权限。

|支持：|不包括：|
|---------|-------|
|Windows 操作系统，仍在其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**状态策略设置**|**行为**|
|**未配置**|用户还可以连接到 Windows Update 网站。|
|**Enabled**|**重要说明：** 如果启用，所有 Windows 更新功能都都删除。 这包括阻止在 Windows 更新网站的访问权限 https://windowsupdate.microsoft.com，从 Windows 更新超链接在开始菜单或启动屏幕，以及**工具**在 Internet Explorer 中的菜单。 Windows 自动更新也被禁用;用户都不会通知也不从 Windows 更新接收重要更新。 此设置还会阻止设备管理器自动从 Windows 更新网站安装驱动程序更新。<br /><br />启用时，可以配置以下通知选项之一：<br /><br />-   **0-不显示任何通知**<br />    此设置将删除所有 Windows 更新功能的访问权限，并且会显示任何通知。<br />-   **1-显示重新启动所需的通知**<br />    此设置将显示有关重新启动以完成安装所需的通知。 **注意：** 在计算机上将显示运行 Windows 8 和 Windows RT，如果启用此策略，仅与重新启动和无法检测更新相关的通知。 不支持的通知选项。 始终显示在登录屏幕上的通知。|
|**已禁用**|用户还可以连接到 Windows Update 网站。|

**选项：** 请参阅**已启用**此设置的表中。

## <a name="supplemental-information"></a>补充信息
本部分提供有关使用打开和本指南中使用的术语将 WSUS 设置保存在组策略和定义的其他信息。 对于熟悉以前版本的 WSUS （WSUS 3.2 及早期版本） 的管理员，没有简单概述了 WSUS 版本之间的差异的表。

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>访问组策略中的 Windows 更新设置
后面的过程介绍如何打开 GPMC 在域控制器上。 该过程然后介绍如何打开现有域级组策略对象 (GPO) 并进行编辑，或创建一个新的域级别 GPO 并打开以进行编辑。

> [!NOTE]
> 您必须是属于**Domain Admins**组或等效的若要执行此过程。

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>若要打开或添加并打开组策略对象

1.  在你的域控制器上转到**服务器管理器**，>**工具**，>**组策略管理**。 此时将打开组策略管理控制台。

2.  在左窗格中，展开林。 例如，双击**林： example.com**。

3.  在左窗格中，双击**域**，然后双击你想要管理的组策略对象的域。 例如，双击**example.com**。

4.  执行下列操作之一：

    -  **若要打开现有的域级别 GPO 进行编辑**，双击包含您想要管理中，右键单击你想要管理，然后单击域策略的组策略对象的域**编辑**。 打开组策略管理编辑器 (GPME)。

    -  **若要创建新的组策略对象并打开以进行编辑**:
        1.  右键单击你想要创建新的组策略对象，并单击的该域**在此域中创建 GPO 并在此处链接**。

        2.  在中**新的 GPO**，在**名称**，为新的组策略对象中，键入一个名称，然后单击**确定**。

        3.  右键单击新建的组策略对象，然后依次**编辑**。 GPME 将打开。

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>若要打开组策略的 Windows 更新或维护计划程序扩展

1.  在组策略管理编辑器中，执行以下操作：

    -   **打开计算机配置 > 组策略的 Windows 更新扩展**。 导航到：*PolicyName* > **计算机配置** > **策略** / **管理模板** > **Windows 组件** > **Windows Update**。

    -   **打开用户配置 > Windows 更新组策略扩展**。 导航到：*PolicyName* > **用户配置** > **策略** > **管理模板** >  **Windows 组件** > **Windows 更新**。

    -   **打开计算机配置 > 组策略的维护计划程序扩展**。 在 GPOE，导航到*PolicyName* > **计算机配置** > **策略** > **管理模板** > **Windows 组件** > **维护计划程序**。

有关组策略的详细信息，请参阅[组策略概述](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12))。

> [!TIP]
> 打开组策略所需的扩展后，可以使用以下步骤来启用、 禁用或设置之间导航：

##### <a name="to-configure-group-policy-settings"></a>配置组策略设置

1.  在中*ExtensionOfGroupPolicy*，双击你想要查看或修改的设置。

2.  若要配置的设置，请执行以下操作：

    -   若要保留默认值未指定的设置，选择状态**未配置**。

    -   若要启用该设置，请选择**已启用**。

    -   若要禁用此设置，请选择**禁用**。

3.  在中**选项**，如果未列出任何选项，请保留默认值或根据需要修改它们。

4.  执行下列操作之一：

    -   若要保存所做的更改并继续执行下一步的设置，请单击**Apply**，然后单击**下一步设置**。

    -   若要保存所做的更改并关闭对话框，请单击**确定**。

    -   若要放弃所有未保存的更改并关闭对话框中，单击**取消**。

### <a name="changes-to-wsus-relevant-to-this-guide"></a>对 WSUS 与本指南相关的更改
下表总结了与本指南的 WSUS 的当前和过去版本之间的主要差异。

|Windows Server 和 WSUS 版本|描述|
|------------------|--------|
| WSUS 6.0 和后续版本的 Windows Server 2012 R2|从 Windows Server 2012 开始，WSUS 服务器角色集成到操作系统，并将 WSUS 客户端的相关联的组策略设置，默认情况下，包含在组策略。|
| Windows Server 2008 （和更早版本的 Windows Server） 与 WSUS 3.2 及更早版本|Windows Server 2008 （和更早版本的 Windows Server） 中使用 WSUS 版本 3.2 （及更早版本），在这些 Windows Server 操作系统的系统中不包括管理 WSUS 客户端的组策略设置。 在 WSUS 管理模板策略设置都**wuau.adm**。 在这些服务器版本中，WSUS 管理必须首先将模板添加到组策略管理控制台 (GPMC) 可以配置 WSUS 的客户端设置。|

### <a name="terms-and-definitions"></a>术语和定义
下面是本指南中使用的术语的列表。

|术语|定义|
|----|-------|
|自动更新|**在 Windows 计算机运行的服务**（自动更新）：引用内置于 Microsoft Windows Vista、 Windows Server 2003、 Windows XP 和 Windows 2000 SP3 操作系统，若要从 Microsoft 更新或 Windows 更新获取更新的客户端计算机组件。<br /><br />**非法引用**（自动更新）：术语用于描述时 Windows Update 代理自动计划并下载更新。|
|自治服务器|用于向管理员可以在其管理 WSUS 组件下游的 Windows Server Update Services (WSUS) 服务器，请参阅。|
|下游服务器|用于引用从另一台 WSUS 服务器而不是从 Microsoft 更新或 Windows 更新获取更新的 Windows Server Update Services (WSUS) 服务器。|
|组策略扩展 (和： 组策略扩展|用于控制用户和计算机 （其策略应用到） 可以配置和使用各种 Windows 服务和功能组策略中设置的集合。 管理员可以使用 WSUS 使用组策略客户端配置的自动更新客户端，以帮助确保最终用户不能禁用或绕过公司更新策略。<br /><br />WSUS 不需要使用 active directory 或组策略。 使用本地组策略或修改 Windows 注册表，也可以应用客户端配置。|
|内部更新服务|对使用一个或多个 WSUS 服务器将更新分发的网络基础结构的非法引用。|
|副本服务器|用于向下游的 Windows Server Update Services (WSUS) 服务器来镜像审批和连接到上游服务器上的设置，请参阅。 你不能管理 WSUS 副本服务器上。|
|Microsoft 更新|**基于 Internet 的 Microsoft 下载网站：** Microsoft Internet 站点的存储和分发的 Windows 计算机 （设备驱动程序）、 Windows 操作系统和其他 Microsoft 软件产品的更新。|
|软件更新服务 (SUS)|SUS 是前身产品为 Windows Server Update Services (WSUS)。|
|“更新”|任何软件版本、 修补程序、 service pack、 功能包和设备驱动程序，可以扩展功能，一台计算机上安装或提高性能和安全的集合。|
|更新文件|若要在计算机上安装更新所需的文件。|
|更新信息 （也称为更新元数据）|有关更新，而不是更新包中的更新二进制文件的信息。 例如，元数据提供的属性的更新，从而使你可找到适合的更新是有用的信息。 元数据还包括 Microsoft 软件许可条款。 下载的更新的元数据包是通常比实际更新文件包小得多。|
|更新源|Windows Server Update Services (WSUS) 服务器同步以获取更新文件位置。 此位置可以是 Microsoft 更新或上游 WSUS 服务器。|
|上游服务器|提供更新文件复制到另一台 WSUS 服务器，又称为下游服务器的 Windows Server Update Services (WSUS) 服务器。|
|Windows Server Update Services (WSUS)|在企业网络上的一个或多个 Windows Server 计算机运行的服务器角色程序。 WSUS 基础结构使您能够管理计算机的网络为安装更新。<br /><br />可以使用 WSUS 来批准或拒绝之前版本中，若要强制更新安装在给定日期的更新，以获取哪些更新您的网络上的每台计算机上的大量报表需要。 可以配置 WSUS 以自动批准更新的某些类 （关键更新、 安全更新、 service pack、 驱动程序等）。 WSUS 还可以批准更新以进行"检测"，以便您可以看到哪些计算机需要给定的更新，而无需安装更新。<br /><br />在 WSUS 实现中，网络中的至少一台 WSUS 服务器必须能够连接到 Microsoft 更新以获取可用更新。 根据网络安全和配置，管理员可以确定其他服务器如何直接连接到 Microsoft 更新。<br /><br />可以配置 WSUS 服务器以通过 Internet 从为这类位置获取更新：<br /><br />-公共 Microsoft 更新<br />-公共 Windows 更新<br />-   Microsoft Store|
|Windows 更新|**基于 Internet 的 Microsoft 下载网站：** Microsoft Internet 站点的存储和分发适用于 Windows 计算机 （设备驱动程序） 和 Windows 操作系统的更新。<br /><br />**计算机服务：** 在计算机运行的 Windows 更新服务的名称。 Windows Update 检测、 下载、 和 Windows 的计算机上安装更新。<br /><br />具体取决于计算机和策略配置，Windows 更新代理可以从下载更新：<br /><br />-   Microsoft Update<br />-Windows 更新<br />-   Microsoft Store<br />-一个 Internet （网络） 更新服务 (WSUS)<br /><br />未在基于 WSUS 的环境中管理的计算机通常将使用 Windows Update 连接直接-通过 Internet-Windows Update、 Microsoft Update 或 Microsoft Store 以获取更新。|
|WSUS 客户端|从 WSUS intranet 更新服务接收更新的计算机。<br /><br />在组策略设置的控制通过自动更新的最终用户交互的情况下： WSUS 环境中的计算机的用户。|

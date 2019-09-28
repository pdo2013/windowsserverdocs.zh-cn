---
title: 步骤 4-配置自动更新组策略设置
description: Windows Server Update Service （WSUS）主题-为自动更新配置组策略设置是部署 WSUS 的四个步骤中的步骤4
ms.prod: windows-server
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
ms.openlocfilehash: a01d8881e8f0f7ca6feff691938f926a12460db0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361660"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>步骤 4：为自动更新配置组策略设置

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 active directory 环境中，你可以使用组策略来定义计算机和用户（在本文档中称为 WSUS 客户端）如何与 Windows 更新交互，以从 Windows Server Update Services （WSUS）获取自动更新。

本主题包含两个主要部分：

[Wsus 客户端更新组策略设置](#group-policy-settings-for-wsus-client-updates)，提供有关控制 WSUS 客户端如何与 Windows 更新进行交互的组策略的 Windows 更新和维护计划程序设置的说明性指导和行为详细信息获取自动更新。

[补充信息](#supplemental-information)包含以下部分：

-   [访问组策略中的 Windows 更新设置](#accessing-the-windows-update-settings-in-group-policy)，该设置提供有关使用组策略管理编辑器的常规指导，以及有关访问组中的更新服务策略扩展和维护计划程序设置的信息政策.

-   与[本指南相关的 wsus 的更改](#changes-to-wsus-relevant-to-this-guide)：对于熟悉 wsus 3.2 和以前版本的管理员，本部分简要概述了与本指南相关的 wsus 的当前版本和以前版本之间的主要差异。

-   [术语和定义](#terms-and-definitions)：本指南中使用的有关 WSUS 和更新服务的各种术语的定义。

## <a name="group-policy-settings-for-wsus-client-updates"></a>WSUS 客户端更新组策略设置
本部分提供有关组策略三个扩展的信息。 在这些扩展中，你将找到一些设置，你可以使用这些设置来配置 WSUS 客户端如何与 Windows 更新进行交互来接收自动更新。

-   [计算机配置 &gt; Windows 更新策略设置](#computer-configuration--windows-update-policy-settings)

-   [计算机配置 &gt; 维护计划程序策略设置](#computer-configuration--maintenance-scheduler-policy-settings)

-   [用户配置 &gt; Windows 更新策略设置](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> 本主题假定你已使用并且熟悉组策略。 如果你不熟悉组策略，建议你在尝试为 WSUS 配置策略设置之前查看此文档的 "[补充信息](#supplemental-information)" 部分中的信息。

### <a name="computer-configuration--windows-update-policy-settings"></a>计算机配置 > Windows 更新策略设置
本部分提供有关以下基于计算机的策略设置的详细信息：

-   [允许自动更新立即安装](#allow-automatic-updates-immediate-installation)

-   [允许非管理员接收更新通知](#allow-non-administrators-to-receive-update-notifications)

-   [允许来自 intranet Microsoft 更新服务位置的签名更新](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [自动更新检测频率](#automatic-updates-detection-frequency)

-   [配置自动更新](#configure-automatic-updates)

-   [计划安装延迟重新启动](#delay-restart-for-scheduled-installations)

-   [不要调整 "关闭 Windows" 对话框中的 "安装更新并关闭" 默认选项](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [不显示 "关闭 Windows" 对话框中的 "安装更新并关闭" 选项](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [启用客户端目标设定](#enable-client-side-targeting)

-   [启用 Windows 更新电源管理以自动唤醒计算机以安装计划的更新](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [对于计划的自动更新安装，已登录的用户无法自动重新启动](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [通过计划安装重新启动重新启动](#re-prompt-for-restart-with-scheduled-installations)

-   [重排自动更新计划安装](#reschedule-automatic-updates-scheduled-installations)

-   [指定 intranet Microsoft 更新服务位置](#specify-intranet-microsoft-update-service-location)

-   [通过自动更新打开建议的更新](#turn-on-recommended-updates-via-automatic-updates)

-   [启用软件通知](#turn-on-software-notifications)

在 GPME 中，基于计算机的配置 Windows 更新策略位于以下路径中：*PolicyName* > **计算机配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **Windows 更新**。

> [!NOTE]
> 默认情况下，不会配置这些设置。

#### <a name="allow-automatic-updates-immediate-installation"></a>允许自动更新立即安装
指定自动更新是否将自动安装不会中断 Windows 服务或重新启动 Windows 的更新。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 如果将 "配置自动更新" 策略设置设置为 "**已禁用**"，则此策略不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定不会立即安装更新。 本地管理员可以通过使用本地组策略编辑器来更改此设置。|
|**Enabled**|指定自动更新在下载并准备好安装更新后立即安装更新。|
|**已禁用**|指定不会立即安装更新。|

**选项：** 没有此设置的选项。

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>允许非管理员接收更新通知
指定非管理用户是否将基于 "配置自动更新策略" 设置接收更新通知。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|请参阅下表中的详细信息。|

> [!NOTE]
> 如果 "配置自动更新" 策略设置已禁用或未配置，则此策略设置不起作用。

> [!IMPORTANT]
> 从 Windows 8 和 Windows RT 开始，默认情况下启用此策略设置。 在所有以前版本的 Windows 中，默认情况下该功能处于禁用状态。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定用户将始终看到帐户控制窗口，并要求提升的权限来执行这些任务。 本地管理员可以通过使用本地组策略编辑器来更改此设置。|
|**Enabled**|指定在确定哪些登录用户将接收更新通知时，Windows 自动更新和 Microsoft 更新将包括非管理员。 非管理用户将能够安装他们收到通知的所有可选的、推荐的和重要的更新内容。 用户将看不到 "用户帐户控制" 窗口，并且它们不需要提升的权限即可安装这些更新，但包含用户界面、最终用户许可协议或 Windows 更新设置更改的情况除外。<br /><br />在以下两种情况下，此设置的效果取决于操作系统：<br /><br />1.**隐藏**或**还原**更新<br />2.**取消**更新安装<br /><br />在 Windows Vista 或 Windows XP 中，如果启用此策略设置，用户将看不到 "用户帐户控制" 窗口，并且它们不需要提升的权限即可隐藏、还原或取消更新。<br /><br />在 Windows Vista 中，如果启用此策略设置，用户将看不到 "用户帐户控制" 窗口，并且它们不需要提升的权限即可隐藏、还原或取消更新。 如果未启用此策略设置，则用户将始终看到 "帐户控制" 窗口，并且他们需要提升的权限才能隐藏、还原或取消更新。<br /><br />在 Windows 7 中，此策略设置不起作用。 用户将始终看到 "帐户控制" 窗口，并需要提升的权限来执行这些任务。<br /><br />在 Windows 8 和 Windows RT 中，此策略设置不起作用。|
|**已禁用**|指定只有登录的管理员才能接收更新通知。 **注意：** 在 Windows 8 和 Windows RT 中，此策略设置在默认情况下处于启用状态。 在所有以前版本的 Windows 中，默认情况下该功能处于禁用状态。|

**选项：** 没有此设置的选项。

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>允许使用来自 Intranet Microsoft 更新服务位置的签名更新
指定在 intranet Microsoft 更新服务位置上发现更新时，自动更新是否接受由 Microsoft 以外的实体签署的更新。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT|

> [!NOTE]
> 来自 intranet Microsoft 更新服务之外的服务的更新必须始终由 Microsoft 签名，并且不受此策略设置的影响。

> [!NOTE]
> Windows RT 不支持此策略。 启用此策略不会对运行 Windows RT 的计算机产生任何影响。

**选项：** 没有此设置的选项。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定来自 intranet Microsoft 更新服务位置的更新必须由 Microsoft 签名。|
|**Enabled**|指定自动更新接受通过 intranet Microsoft 更新服务位置收到的更新（如果这些更新通过本地计算机的 "受信任的发行者" 证书存储区中找到的证书签名）。|
|**已禁用**|指定来自 intranet Microsoft 更新服务位置的更新必须由 Microsoft 签名。|

**选项：** 没有此设置的选项。

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>始终在计划的时间自动重启
指定在 Windows 更新安装重要更新后是否始终立即开始重新启动计时器，而不是首先在登录屏幕上通知用户至少两天。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 如果启用了 "不通过登录的用户进行计划的自动更新安装的自动重新启动" 策略设置，则此策略不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定 Windows 更新将不会更改计算机的重新启动行为。|
|**Enabled**|指定在 Windows 更新安装重要更新后，将始终立即开始重新启动计时器，而不是首先在登录屏幕上通知用户至少两天。<br /><br />重新启动计时器可以配置为以从15到180分钟的任何值开始。 当计时器运行时，即使计算机已登录用户，重新启动仍将继续。|
|**已禁用**|指定 Windows 更新将不会更改计算机的重新启动行为。|

**选项：** 如果启用此设置，你可以指定在安装更新之后经过一段时间后，才会发生强制计算机重新启动。

#### <a name="automatic-updates-detection-frequency"></a>自动更新检测频率
指定 Windows 将用于确定在检查可用更新前等待的时长的小时数。 使用此处指定的小时数减去指定小时数的 0% 到 20% 来确定准确的等待时间。 例如，如果使用此策略来指定20小时检测频率，则应用此策略的所有客户端都将在16到20小时之间检查是否有更新。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT|

> [!NOTE]
> 必须启用 "指定 intranet Microsoft 更新服务位置" 设置，此策略才会生效。
>
> 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

> [!NOTE]
> Windows RT 不支持此策略。 启用此策略不会对运行 Windows RT 的计算机产生任何影响。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定 Windows 将在22小时的默认时间间隔内检查可用更新。|
|**Enabled**|指定 Windows 将按指定的时间间隔检查可用更新。|
|**已禁用**|指定 Windows 将在22小时的默认时间间隔内检查可用更新。|

**选项：** 如果启用此设置，则可以指定在检查更新之前 Windows 更新等待的时间间隔（以小时为单位）。

#### <a name="configure-automatic-updates"></a>配置自动更新
指定是否在此计算机上启用自动更新。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT|

如果启用，则必须选择此组策略设置中提供的四个选项之一。

若要使用此设置，请选择 "**已启用**"，然后在 "**配置自动更新**" 下的 "**选项**" 中，选择一个选项（2、3、4或5）。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定未在组策略级别指定使用自动更新。 但是，计算机管理员仍可以在 "控制面板" 中配置 "自动更新"。|
|**Enabled**|指定 Windows 在计算机处于联机状态时识别，并使用其 Internet 连接在 Windows 更新中搜索可用更新。<br /><br />启用后，将允许本地管理员使用 Windows 更新控制面板选择其选择的配置选项。 但是，将不允许本地管理员禁用自动更新的配置。<br /><br />-   **2-通知下载并通知安装**<br />    当 Windows 更新查找适用于计算机的更新时，系统将通知用户更新已准备就绪，可供下载。 然后，用户可以运行 Windows 更新以下载和安装任何可用的更新。<br />-   **3-自动下载并通知安装**（默认设置）<br />    Windows 更新查找适用的更新，并在后台下载它们;在此过程中，用户不会收到通知或中断。 下载完成后，会通知用户有更新可供安装。 然后，用户可以运行 Windows 更新以安装已下载的更新。<br />-   **4-自动下载并计划安装**<br />    您可以使用此组策略设置中的选项来指定计划。 如果未指定计划，则所有安装的默认计划将在每天凌晨3:00。 如果任何更新需要重新启动才能完成安装，则 Windows 将自动重新启动计算机。 （如果在 Windows 准备好重新启动时用户已登录到计算机，则会通知用户并提供延迟重新启动的选项。）**注意：** 启动 Windows 8 时，可以将更新设置为在自动维护期间安装，而不是使用绑定到 Windows 更新的特定计划。 自动维护将在计算机未使用时安装更新，并避免在计算机使用电池电源运行时安装更新。 如果自动维护无法在数天内安装更新，Windows 更新会立即安装更新。 然后，用户将收到有关挂起的重新启动的通知。 只有在不可能发生数据丢失的情况下，才会发生挂起的重新启动。    你可以在 "GPME 维护计划程序" 设置中指定计划选项，该设置位于路径*PolicyName* > **计算机配置** > **策略**中  > **管理模板** > **Windows 组件** > **维护计划程序**1**自动维护激活边界**。 请参阅此引用中标题为的部分：用于设置详细信息的[维护计划程序设置](#computer-configuration--maintenance-scheduler-policy-settings)。    **5-允许本地管理员选择设置**<br />-指定是否允许本地管理员使用自动更新控制面板来选择其选择的配置选项，例如，本地管理员是否可以选择计划的安装时间。<br />    不允许本地管理员禁用自动更新配置。|
|**已禁用**|指定必须从 Internet 上手动下载并安装来自公共 Windows 更新服务的任何客户端更新。|

#### <a name="delay-restart-for-scheduled-installations"></a>计划安装延迟重新启动
指定在继续进行计划的重新启动之前自动更新等待的时间量。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 此策略仅在自动更新配置为执行更新的计划安装时才适用。 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定在安装更新之后，在进行任何计划的重新启动之前，将会经历默认等待时间15分钟。|
|**Enabled**|指定在安装完成时，将在指定的分钟数之后发生计划的重新启动。|
|**已禁用**|指定在安装更新之后，在进行任何计划的重新启动之前，将会经历默认等待时间15分钟。|

**选项：** 如果启用此设置，则可以使用此选项来指定在继续计划的重新启动之前自动更新等待的时间量（以分钟为单位）。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>请勿调整用于安装更新的默认选项，并在 "关闭 Windows" 对话框中关闭
使用此策略设置，可以指定是否允许 "**安装更新" 和 "关机**" 选项作为 "**关闭 Windows** " 对话框中的默认选项。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 如果*PolicyName* > **计算机配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **Windows 更新**，则此策略设置不会产生任何影响 1**不显示 "关闭 Windows" 对话框中的 "安装更新并关闭" 选项**启用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|如果在用户选择 "关机" 选项关闭计算机时可以安装更新，则指定 "**安装更新并关闭**" 将是 "**关闭 Windows** " 对话框中的默认选项。|
|**Enabled**|如果启用此策略设置，则用户的 "上次关机" 选项（例如 "休眠" 或 "重新启动"）是 "**关闭 Windows** " 对话框中的默认选项，不管 "**安装更新" 和 "关机**" 选项是否在**希望计算机做什么？** 下拉菜单.|
|**已禁用**|如果在用户选择 "关机" 选项关闭计算机时可以安装更新，则指定 "**安装更新并关闭**" 将是 "**关闭 Windows** " 对话框中的默认选项。|

**选项：** 没有此设置的选项。

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>不要连接任何 Windows 更新 Internet 位置
即使 Windows 更新配置为接收来自 intranet 更新服务的更新，它也会定期从公共 Windows 更新服务检索信息，以便将来能够连接到 Windows 更新和其他服务，例如 Microsoft 更新或 Microsoft Store。

如果启用此策略，则将禁用从公共 Windows Server Update 服务定期检索信息的功能，并且可能会导致与 Microsoft Store 等公共服务的连接停止工作。

|支持：|排除|
|---------|-------|
|从 Windows Server 2012 R2 开始，Windows 8.1 或 Windows RT 8.1，仍在其 Microsoft 产品中的 Windows 操作系统[支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 仅当计算机配置为使用 "指定 intranet Microsoft 更新服务位置" 策略设置连接到 intranet 更新服务时，此策略才适用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|默认情况下，从公共 Windows Server Update Service 中检索信息的行为将保持不变。|
|**Enabled**|指定计算机将不会检索来自公共 Windows Server Update 服务的信息。|
|**已禁用**|默认情况下，从公共 Windows Server Update Service 中检索信息的行为将保持不变。|

**选项：** 没有此设置的选项。

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>不显示 "关闭 Windows" 对话框中的 "安装更新" 和 "关机" 选项
指定是否在 "**关闭 Windows** " 对话框中显示 "**安装更新" 和 "关机**" 选项。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定当用户选择 "关闭" 选项关闭计算机时，"**安装更新" 和 "关机**" 选项在 "**关闭 Windows** " 对话框中可用。 本地管理员可以使用本地策略更改此设置。|
|**Enabled**|指定 "**安装更新并关闭**" 将不会显示为 "**关闭 Windows** " 对话框中的选项，即使当用户选择 "关闭" 选项关闭计算机时，也可以安装更新。|
|**已禁用**|指定如果在用户选择 "关闭" 选项关闭计算机时可以安装更新，则 "**安装更新" 和 "关机**" 选项将是 "**关闭 Windows** " 对话框中的默认选项。|

**选项：** 没有此设置的选项。

#### <a name="enable-client-side-targeting"></a>启用客户端定位
指定在 WSUS 控制台中配置的目标组名称，这些名称将从 WSUS 接收更新。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT|

> [!NOTE]
> 仅当此计算机配置为支持 WSUS 中指定的目标组名称时，此策略才适用。 如果目标组名称在 WSUS 中不存在，则在创建它之前将忽略该名称。 如果 "指定 intranet Microsoft 更新服务位置" 策略设置已禁用或未配置，则此策略不起作用。

> [!NOTE]
> Windows RT 不支持此策略。 启用此策略不会对运行 Windows RT 的计算机产生任何影响。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定不向 WSUS 发送目标组信息。 本地管理员可以通过使用本地策略来更改此设置。|
|**Enabled**|指定将指定的目标组信息发送到 WSUS，后者使用它来确定应将哪些更新部署到此计算机。 如果 WSUS 支持多个目标组，则可以使用此策略指定多个组名称（以分号分隔），前提是已在 WSUS 的 "计算机组" 列表中添加目标组名称。 否则，必须指定单个组。|
|**已禁用**|指定不向 WSUS 发送目标组信息。|

**选项：** 使用此空间可以指定一个或多个目标组名称。

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>启用 Windows 更新电源管理以自动唤醒计算机以安装计划的更新
指定在有计划安装更新的情况，Windows 更新是否将使用 Windows 电源管理或电源选项功能自动唤醒计算机。

只有将 Windows 更新配置为自动安装更新时，计算机才会自动唤醒。 如果计算机在发生计划的安装时间且存在要应用的更新时处于休眠状态，Windows 更新将使用 Windows 电源管理或电源选项功能自动唤醒计算机以安装更新。 如果安装截止时间发生，Windows 更新还将唤醒计算机并安装更新。

除非有要安装的更新，否则计算机将无法唤醒。 如果计算机使用电池电源，则在 Windows 更新唤醒计算机时，它将不安装更新，并且计算机会在两分钟后自动返回到休眠状态。

|支持：|排除|
|---------|-------|
|从 Windows Vista 和 Windows Server 2008 （Windows 7）开始，仍在其 Microsoft 产品中的 Windows 操作系统[支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|Windows 更新不会将计算机从休眠中唤醒以安装更新。 本地管理员可以通过使用本地策略来更改此设置。|
|**Enabled**|Windows 更新将计算机从休眠中唤醒，以便在前面列出的条件下安装更新。|
|**已禁用**|Windows 更新不会将计算机从休眠中唤醒以安装更新。|

**选项：** 没有此设置的选项。

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>对于有已登录用户的计算机，计划的自动更新安装不执行自动重启
指定完成计划的安装，自动更新将等待任何已登录的用户重新启动计算机，而不是使计算机自动重新启动。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 此策略仅在自动更新配置为执行更新的计划安装时才适用。 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定自动更新会通知用户计算机将在5分钟内自动重新启动才能完成安装。|
|**Enabled**|某些更新需要重新启动计算机才能使更新生效。 如果状态设置为 "已启用"，则当用户登录到计算机时，自动更新不会自动重新启动计算机。 相反，自动更新会通知用户重新启动计算机。|
|**已禁用**|指定自动更新会通知用户计算机将在5分钟内自动重新启动才能完成安装。|

**选项：** 没有此设置的选项。

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>对计划的安装再次提示重启
指定在重新启动计划的重新启动之前自动更新等待的时间量。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT|

> [!IMPORTANT]
> 此策略仅在自动更新配置为执行更新的计划安装时才适用。 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

> [!NOTE]
> 此策略在运行 Windows RT 的计算机上不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|在关闭重启消息提示后十分钟就会进行一次计划的重新启动。 本地管理员可以通过使用本地策略来更改此设置。|
|**Enabled**|指定在上一次重新启动提示后进行了延迟，计划的重新启动将在指定的分钟数之后发生。|
|**已禁用**|在关闭重启消息提示后十分钟就会进行一次计划的重新启动。|

**选项：** 启用后，可以使用此设置选项来指定在再次提示用户计划重新启动之前所经过的时间（以分钟为单位）。

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>重新计划自动更新计划的安装
指定自动更新在计算机启动之后等待一段时间，然后再继续执行之前已丢失的计划安装。

如果将 "状态" 设置为 "**未配置**"，则在下次启动计算机后，将会出现未计划的安装一分钟。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 此策略仅在自动更新配置为执行更新的计划安装时才适用。 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定在计算机下一次启动后，错过的计划安装将在一分钟后发生。|
|**Enabled**|指定在计算机下次启动后的指定分钟数后，不会提前发生的计划安装。|
|**已禁用**|指定将在下一次计划的安装中发生缺少计划的安装。|

**选项：** 如果启用此策略设置，你可以使用它来指定在计算机下次启动后的分钟数，这将导致以前未发生的计划安装。

#### <a name="specify-intranet-microsoft-update-service-location"></a>指定 Intranet Microsoft 更新服务位置
指定用于托管来自 Microsoft 更新的更新的 Intranet 服务器。 然后，你可以使用 WSUS 自动更新网络上的计算机。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|Windows RT。|

此设置使你能够在网络上指定将充当内部更新服务的 WSUS 服务器。 WSUS 客户端将在此服务中搜索适用于应用的更新，而不是在 Internet 上使用 Microsoft 更新。

若要使用此设置，你必须设置两个服务器名称值：客户端从中检测和下载更新的服务器，以及更新的工作站上传统计信息的服务器。 如果在同一台服务器上配置两个服务，则值需要不同。

> [!NOTE]
> 如果禁用 "配置自动更新" 策略设置，则此策略不起作用。

> [!NOTE]
> Windows RT 不支持此策略。 启用此策略不会对运行 Windows RT 的计算机产生任何影响。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|如果策略或用户首选项未禁用自动更新，则此策略指定客户端直接连接到 Internet 上的 Windows 更新站点。|
|**Enabled**|指定客户端连接到指定的 WSUS 服务器，而不是 Windows 更新搜索和下载更新。 如果启用此设置，则表示组织中的最终用户无需通过防火墙即可获取更新，并使你能够在部署更新之前对其进行测试。|
|**已禁用**|如果策略或用户首选项未禁用自动更新，则此策略指定客户端直接连接到 Internet 上的 Windows 更新站点。|

**选项：** 如果启用此策略设置，则必须指定 WSUS 客户端在检测更新时将使用的 intranet 更新服务，以及更新后的 WSUS 客户端将向其上传统计信息的 Internet 统计服务器。 示例值：


|                    设置选项：                    |    示例值：    |
|-------------------------------------------------------|----------------------|
| 设置用于检测更新的 intranet 更新服务 |  http://wsus01:8530  |
|          设置 intranet 统计服务器           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>通过自动更新打开建议的更新
指定自动更新是否将从 WSUS 提供重要的和推荐的更新。

|支持：|排除|
|---------|-------|
|从 Windows Vista 开始，仍在其 Microsoft 产品中的 Windows 操作系统[支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定自动更新将继续提供重要更新（如果已将其配置为这样做）。|
|**Enabled**|指定自动更新将从 WSUS 安装推荐的更新和重要更新。|
|**已禁用**|指定自动更新将继续提供重要更新（如果已将其配置为这样做）。|

**选项：** 没有此设置的选项。

#### <a name="turn-on-software-notifications"></a>启用软件通知
此策略设置可让你控制用户是否从 Microsoft 更新服务查看有关精选软件的详细增强通知消息。 增强的通知消息会传达该值，并推广可选软件的安装和使用。 此策略设置用于在松散托管的环境中使用，你可以在这些环境中允许最终用户访问 Microsoft 更新服务。

如果使用的不是 Microsoft 更新服务，则 "软件通知" 策略设置不起作用。

如果 "配置自动更新" 策略设置已禁用或未配置，则 "软件通知" 策略设置不起作用。

|支持：|排除|
|---------|-------|
|从 Windows Server 2008 （Windows Vista）和 Windows 7 开始，仍在其 Microsoft 产品中的 Windows 操作系统[支持生命周期](https://support.microsoft.com/gp/lifeselect)。|null|

> [!NOTE]
> 默认情况下，此策略设置处于禁用状态。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|在运行 Windows 7 的计算机上的用户不会为可选应用程序提供消息。 不会为运行 Windows Vista 的计算机上的用户提供可选的应用程序或更新的消息。 本地管理员可以通过使用 "控制面板" 或本地策略来更改此设置。|
|**Enabled**|如果启用此策略设置，则当特色软件可用时，将在用户的计算机上显示一条通知消息。 用户可以单击通知打开 Windows 更新并获取有关软件的详细信息或安装该软件。 用户还可以单击 "**关闭此消息**" 或单击 "**稍后显示**" 以根据需要延迟通知。<br /><br />在 Windows 7 中，此策略设置将仅控制可选应用程序的详细通知。 在 Windows Vista 中，此策略设置控制可选的应用程序和更新的详细通知。|
|**已禁用**|指定将不为可选应用程序提供运行 Windows 7 的用户详细通知消息，并且不会为运行 Windows Vista 的用户提供可选的应用程序或可选更新的详细通知消息。|

**选项：** 没有此设置的选项。

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>计算机配置 > 维护计划程序策略设置
在 "配置自动更新" 设置中，你选择了 " **4-自动下载并计划安装**" 选项，则可以在 GPMC 中为运行 Windows 8 和 Windows RT 的计算机指定 "计划维护计划程序" 设置。 如果未在 "配置自动更新" 设置中选择选项4，则无需为自动更新配置这些设置。 维护计划程序设置位于路径中：*PolicyName* > **计算机配置** >  个**策略** > **管理模板**@no__t 个**Windows 组件** > **维护计划程序**。 组策略的维护计划程序扩展包含以下设置：

-   [自动维护激活边界](#automatic-maintenance-activation-boundary)

-   [自动维护随机延迟](#automatic-maintenance-random-delay)

-   [自动唤醒策略](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>自动维护激活边界
此策略可让你配置 "自动维护激活边界" 设置。

维护激活边界是启动自动维护的每日计划时间。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 此设置与**配置自动更新**中的选项4有关。 如果未在 "**配置自动更新**中选择选项4，则无需配置此设置。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|如果未配置此策略设置，则将应用在**操作中心**的客户端计算机上指定的每日计划时间  > **自动维护**控制面板。|
|**Enabled**|如果启用此策略设置，将覆盖 **"控制面板"**  > **操作中心** > **自动维护**（或在某些客户端版本中为**维护**）中的客户端计算机上配置的任何默认设置或修改的设置。|
|**已禁用**|如果将此策略设置设置为 "**禁用**"，则在 "控制面板" 中指定的每日计划**时间 @no__t 2** **自动维护**，将应用控制面板。|

#### <a name="automatic-maintenance-random-delay"></a>自动维护随机延迟
此策略设置允许你配置自动维护激活随机延迟。

"维护随机延迟" 是指自动维护从其激活边界开始延迟的时间量。 此设置可用于随机维护可能为性能要求的虚拟机。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 此设置与**配置自动更新**中的选项4有关。 如果未在 "**配置自动更新**中选择选项4，则无需配置此设置。

默认情况下，当启用时，定期维护随机延迟设置为**PT4H**。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|**自动**使用四个小时的随机延迟。|
|**Enabled**|自动维护将从其激活边界开始延迟最多指定的时间。|
|**已禁用**|无随机延迟适用于自动维护。|

#### <a name="automatic-wakeup-policy"></a>自动唤醒策略
此策略设置允许你配置自动维护唤醒策略。

维护唤醒策略指定自动维护是否应向操作系统发出唤醒请求，以便进行日常的计划维护。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 如果显式禁用了操作系统的 power 唤醒策略，则此设置不起作用。

> [!NOTE]
> 此设置与**配置自动更新**中的选项4有关。 如果未在 "**配置自动更新**中选择选项4，则无需配置此设置。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|如果未配置此策略设置，则会应用 "**操作中心** > **自动维护**控制面板" 中指定的唤醒设置。|
|**Enabled**|如果启用此策略设置，则在需要时，自动维护将尝试设置操作系统唤醒策略，并对每日计划时间发出唤醒请求。|
|**已禁用**|如果禁用此策略设置，则会应用 "**操作中心** > **自动维护**控制面板" 中指定的唤醒设置。|

### <a name="user-configuration--windows-update-policy-settings"></a>用户配置 > Windows 更新策略设置
本部分提供有关以下基于用户的策略设置的详细信息：

-   [不显示 "关闭 Windows" 对话框中的 "安装更新并关闭" 选项](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [不要调整 "关闭 Windows" 对话框中的 "安装更新并关闭" 默认选项](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [删除使用所有 Windows 更新功能的访问权限](#remove-access-to-use-all-windows-update-features)

在 GPMC 中，自动计算机更新的用户设置位于路径中：*PolicyName* > **用户配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **Windows 更新**。 当选择 "Windows 更新策略" 的 "**设置**" 选项卡以按字母顺序对设置进行排序时，这些设置的列出顺序与组策略中的 "计算机配置" 和 "用户配置" 扩展中显示的顺序相同。

> [!NOTE]
> 默认情况下，除非另有说明，否则不会配置这些设置。

> [!TIP]
> 对于这些设置，你可以使用以下步骤来启用、禁用或在设置之间导航：

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>不在 "关闭 Windows" 对话框中显示 "安装更新并关闭" 选项
指定是否在 "**关闭 Windows** " 对话框中显示 "**安装更新" 和 "关机**" 选项。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定当用户选择 "关闭" 选项关闭计算机时，"**安装更新" 和 "关机**" 选项将显示在 "**关闭 Windows** " 对话框中。|
|**Enabled**|如果启用此策略设置，则在 "**关闭 Windows** " 对话框中将不会显示 "**安装更新并关闭**"，即使在用户选择 "关闭" 选项关闭计算机时也可进行安装，也不会显示更新。|
|**已禁用**|指定当用户选择 "关闭" 选项关闭计算机时，"**安装更新" 和 "关机**" 选项将显示在 "**关闭 Windows** " 对话框中。|

**选项：** 没有此设置的选项。

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>不要调整 "关闭 Windows" 对话框中的 "安装更新并关闭" 默认选项
指定是否允许 "**安装更新" 和 "关机**" 选项作为 "**关闭 Windows** " 对话框中的默认选项。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

> [!NOTE]
> 如果*PolicyName* > **用户配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **Windows 更新**，则此策略设置不会产生任何影响 @no__**在启用 "关闭 Windows" 对话框中，t-sql 不显示 "安装更新并关闭" 选项**。

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|指定当用户选择 "关机" 选项关闭计算机时，"**安装更新" 和 "关机**" 选项是否将是 "**关闭 Windows** " 对话框中的默认选项。|
|**Enabled**|指定是否在 "**关闭 Windows** " 对话框中使用用户的上次关机选项（例如 "休眠" 或 "重新启动"），无论 "**安装更新" 和 "关机" 选项**是否在**希望计算机执行吗？** 下拉菜单.|
|**已禁用**|指定当用户选择 "关机" 选项关闭计算机时，"**安装更新" 和 "关机**" 选项是否将是 "**关闭 Windows** " 对话框中的默认选项。|

**选项：** 没有此设置的选项。

#### <a name="remove-access-to-use-all-windows-update-features"></a>删除使用所有 Windows 更新功能的访问权限
此设置使你可以删除 WSUS 客户端对 Windows 更新的访问权限。

|支持：|排除|
|---------|-------|
|仍处于其[Microsoft 产品支持生命周期](https://support.microsoft.com/gp/lifeselect)内的 Windows 操作系统。|null|

|||
|-|-|
|**策略设置状态**|**操作**|
|**未配置**|用户可以连接到 Windows 更新网站。|
|**Enabled**|**重要提示：** 如果启用，所有 Windows 更新功能都将被删除。 这包括阻止在 Windows 更新网站的访问权限 [https://windowsupdate.microsoft.com](https://windowsupdate.microsoft.com )，从 Windows 更新超链接在开始菜单或启动屏幕，以及**工具**在 Internet Explorer 中的菜单。 Windows 自动更新也被禁用;系统将不会向用户发送有关或从 Windows 更新收到关键更新的通知。 此设置还会阻止设备管理器从 Windows 更新网站自动安装驱动程序更新。<br /><br />启用后，可以配置以下通知选项之一：<br /><br />@no__t 0 **-不显示任何通知**<br />    此设置将删除对 Windows 更新功能的所有访问权限，并且不会显示任何通知。<br />-   **1-显示需要重新启动通知**<br />    此设置将显示完成安装所需的重新启动通知。 **注意：** 在运行 Windows 8 和 Windows RT 的计算机上，如果启用此策略，则将仅显示与重新启动和无法检测更新相关的通知。 不支持通知选项。 始终显示登录屏幕上的通知。|
|**已禁用**|用户可以连接到 Windows 更新网站。|

**选项：** 对于此设置，请参阅表中的 "**已启用**"。

## <a name="supplemental-information"></a>补充信息
本部分提供有关使用打开和保存组策略中的 WSUS 设置的其他信息，以及本指南中使用的术语的定义。 对于熟悉 WSUS 的过去版本（WSUS 3.2 和早期版本）的管理员，有一个表简要汇总了 WSUS 版本之间的差异。

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>访问组策略中的 Windows 更新设置
下面的过程介绍如何在域控制器上打开 GPMC。 然后，该过程介绍如何打开现有的域级组策略对象（GPO）进行编辑，或创建新的域级 GPO，并将其打开进行编辑。

> [!NOTE]
> 若要执行此过程，您必须是**Domain Admins**组的成员或同等身份。

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>打开或添加并打开组策略对象

1.  在域控制器上，中转到**服务器管理器**，>**工具**"，>**组策略管理**"。 组策略管理控制台打开。

2.  在左侧窗格中，展开你的林。 例如，双击 "**林： example.com**"。

3.  在左窗格中，双击 "**域**"，然后双击要为其管理组策略对象的域。 例如，双击 " **example.com**"。

4.  执行下列操作之一：

    -  **若要打开现有域级 GPO 进行编辑**，请双击包含要管理的组策略对象的域，右键单击要管理的域策略，然后单击 "**编辑**"。 组策略管理编辑器（GPME）将打开。

    -  **若要创建新的组策略对象并打开进行编辑**：
        1.  右键单击要为其创建新组策略对象的域，然后单击 "**在此域中创建 GPO 并在此处链接"** 。

        2.  在 "**新建 GPO**" 的 "**名称**" 中，键入新组策略对象的名称，然后单击 **"确定"** 。

        3.  右键单击新的组策略对象，然后单击 "**编辑**"。 GPME 将打开。

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>打开组策略的 Windows 更新或维护计划程序扩展

1.  在组策略管理编辑器 "中，执行下列操作之一：

    -   **打开组策略 > Windows 更新扩展的计算机配置**。 导航到：*PolicyName* > **计算机配置** >  个**策略** / **管理模板** >  个**Windows 组件** > **Windows 更新**。

    -   **打开组策略 > Windows 更新扩展的 "用户配置"** 。 导航到：*PolicyName* > **用户配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **Windows 更新**。

    -   **打开组策略 > 维护计划程序扩展的计算机配置**。 在 GPOE 中，导航到*PolicyName* > **台计算机配置** >  个**策略** > **管理模板** >  个**Windows 组件** > **维护计划程序**。

有关组策略的详细信息，请参阅[组策略概述](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12))。

> [!TIP]
> 打开所需组策略的扩展后，可以使用以下步骤启用、禁用或在设置之间导航：

##### <a name="to-configure-group-policy-settings"></a>配置组策略设置

1.  在*ExtensionOfGroupPolicy*中，双击要查看或修改的设置。

2.  若要配置该设置，请执行以下操作之一：

    -   若要保留设置的默认未指定状态，请选择 "**未配置**"。

    -   若要启用此设置，请选择 "**已启用**"。

    -   若要禁用此设置，请选择 "**禁用**"。

3.  在 "**选项**" 中，如果列出了任何选项，则保留默认值或根据需要进行修改。

4.  执行下列操作之一：

    -   若要保存更改并转到下一个设置，请单击 "**应用**"，然后单击 "**下一设置**"。

    -   若要保存更改并关闭对话框，请单击 **"确定"** 。

    -   若要放弃所有未保存的更改并关闭对话框，请单击 "**取消**"。

### <a name="changes-to-wsus-relevant-to-this-guide"></a>与本指南相关的 WSUS 更改
下表汇总了与本指南相关的 WSUS 的当前版本和以前版本之间的主要差异。

|Windows Server 和 WSUS 版本|描述|
|------------------|--------|
| Windows Server 2012 6.0 R2 及更早版本|从 Windows Server 2012 开始，WSUS 服务器角色与操作系统集成，默认情况下，WSUS 客户端的关联组策略设置将包含在组策略中。|
| Windows Server 2008 （以及更早版本的 Windows Server），WSUS 3.2 及更早版本|在 Windows Server 2008 （以及更早版本的 Windows Server）中，使用 WSUS 版本3.2 （及更早版本），管理 WSUS 客户端的组策略设置不包括在这些 Windows Server 操作系统中。 策略设置位于 WSUS 管理模板**wuau**中。 在这些服务器版本中，必须先将 WSUS 管理模板添加到组策略管理控制台（GPMC）中，然后才能配置 WSUS 客户端设置。|

### <a name="terms-and-definitions"></a>术语和定义
以下是本指南中使用的术语列表。

|术语|定义|
|----|-------|
|自动更新|**在 Windows 计算机上运行的服务**（自动更新）：指的是 Microsoft Windows Vista、Windows Server 2003、Windows XP 和 Windows 2000 中内置的客户端计算机组件，用于从 Microsoft 更新或 Windows 更新获取更新。<br /><br />**随意引用**（自动更新）：用于描述 Windows 更新代理何时自动计划和下载更新的术语。|
|自治服务器|用于引用管理员可以在其上管理 WSUS 组件的下游 Windows Server Update Services （WSUS）服务器。|
|下游服务器|用于引用 Windows Server Update Services （WSUS）服务器，该服务器从另一台 WSUS 服务器（而不是 Microsoft 更新或 Windows 更新）获取更新。|
|组策略扩展（和：组策略的扩展|组策略中的设置的集合，这些设置用于控制用户和计算机（应用策略）可以配置和使用各种 Windows 服务和功能的方式。 管理员可以将 WSUS 与组策略用于自动更新客户端的客户端配置，以帮助确保最终用户无法禁用或绕过公司的更新策略。<br /><br />WSUS 不需要使用 active directory 或组策略。 还可以通过使用本地组策略或通过修改 Windows 注册表来应用客户端配置。|
|内部更新服务|对使用一个或多个 WSUS 服务器来分发更新的网络基础结构的随意引用。|
|副本服务器|使用来指下游 Windows Server Update Services （WSUS）服务器，该服务器镜像其连接到的上游服务器上的审批和设置。 不能在副本服务器上管理 WSUS。|
|Microsoft 更新|**基于 Internet 的 Microsoft 下载网站：** 为 Windows 计算机（设备驱动程序）、Windows 操作系统和其他 Microsoft 软件产品存储和分发更新的 Microsoft Internet 站点。|
|软件更新服务（SUS）|SUS 是 Windows Server Update Services （WSUS）的前置产品。|
|“更新”|可以安装在计算机上的任何软件修订、修补程序、service pack、功能包和设备驱动程序的集合，以扩展功能，或提高性能和安全性。|
|更新文件|在计算机上安装更新所需的文件。|
|更新信息（也称为更新元数据）|有关更新的信息，而不是更新包中的更新二进制文件。 例如，元数据提供有关更新的属性的信息，从而使您可以了解更新的用途。 元数据还包括 Microsoft 软件许可条款。 下载到更新的元数据包通常比实际的更新文件包小得多。|
|更新源|Windows Server Update Services （WSUS）服务器同步以获取更新文件的位置。 此位置可以是 Microsoft 更新或上游 WSUS 服务器。|
|上游服务器|一种 Windows Server Update Services （WSUS）服务器，该服务器将更新文件提供给另一台 WSUS 服务器，而后者又称为下游服务器。|
|Windows Server Update Services (WSUS)|在企业网络上的一台或多台 Windows Server 计算机上运行的服务器角色程序。 WSUS 基础结构使你能够管理网络上要安装的计算机的更新。<br /><br />你可以使用 WSUS 在发布之前批准或拒绝更新，以强制按指定日期安装更新，并获取有关网络上的每台计算机所需的更新的详细报表。 你可以将 WSUS 配置为自动批准某些类别的更新（关键更新、安全更新、service pack、驱动程序等）。 WSUS 还允许你仅批准 "检测" 的更新，以便你可以查看哪些计算机将需要特定更新，而无需安装更新。<br /><br />在 WSUS 实现中，网络中必须至少有一台 WSUS 服务器能够连接到 Microsoft 更新以获取可用更新。 管理员可以根据网络安全和配置确定其他多少服务器直接连接到 Microsoft 更新。<br /><br />可以将 WSUS 服务器配置为通过 Internet 获取更新，如下所示：<br /><br />-公用 Microsoft 更新<br />-公用 Windows 更新<br />-Microsoft Store|
|Windows 更新|**基于 Internet 的 Microsoft 下载网站：** 为 Windows 计算机（设备驱动程序）和 Windows 操作系统存储和分发更新的 Microsoft Internet 站点。<br /><br />**计算机服务：** 计算机上运行的 Windows 更新服务的名称。 Windows 更新在 Windows 计算机上检测、下载和安装更新。<br /><br />根据计算机和策略配置，Windows 更新代理可以从下载更新：<br /><br />-Microsoft 更新<br />-Windows 更新<br />-Microsoft Store<br />-Internet （网络）更新服务（WSUS）<br /><br />不在基于 WSUS 的环境中管理的计算机通常会使用 Windows 更新通过 Internet 直接连接到 Windows 更新、Microsoft 更新或 Microsoft Store 以获取更新。|
|WSUS 客户端|从 WSUS intranet 更新服务接收更新的计算机。<br /><br />如果组策略设置控制最终用户与自动更新的交互： WSUS 环境中计算机的用户。|

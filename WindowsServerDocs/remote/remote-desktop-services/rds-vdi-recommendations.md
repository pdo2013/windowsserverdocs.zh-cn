---
title: VDI 桌面的建议配置
description: 建议的设置和配置适用于 Windows 10 1607 (10.0.1393) 桌面用作 VDI 映像的开销降到最低
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 24704373dedf6a44809b83f3df17bd073cee2bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818378"
---
# <a name="recommended-settings-for-vdi-desktops"></a>建议的 VDI 桌面设置

>适用于：Windows Server （半年频道），Windows Server 2016、 Windows10

Microsoft 桌面虚拟化会自动检测设备的配置和网络条件，以获取用户启动和运行更快地通过启用即时的企业应用程序和桌面设置和它提供 IT 部门提供到旧的访问权限在迁移到 Windows 10 的应用程序。

虽然 Windows 10 操作系统很好地调整在初始状态下，有机会对其进行完善进一步专用于企业的 Microsoft 虚拟桌面基础结构 (VDI) 环境。 在 VDI 环境中，从一开始禁用许多后台服务和任务。

本主题不是蓝图，但而不是指南或起始点。 某些建议可能会禁用你想要使用，因此，应考虑成本与调整你的方案中的任何特定设置的优势的功能。

这些说明和建议的设置都与 Windows 10 1607 （版本 10.0.1393） 相关。

> [!NOTE]  
> 本主题中未特别提及的任何设置可以保留在其默认值 （或每个要求和策略设置） 而无需对 VDI 功能明显影响。

在创建映像所基于的 VDI 部署，请务必使用**Current Branch**。 有关当前分支的详细信息，请参阅[Windows 10 发布信息](https://technet.microsoft.com/windows/release-info.aspx)。

## <a name="creating-the-windows-10-image"></a>创建 Windows 10 映像
第一步是物理或虚拟机上安装 Windows 10 1607 （版本 10.0.1393） 的参考映像。 安装到虚拟机非常简单，并且允许您保存版本的虚拟硬盘 (VHD) 文件，以防你想要回滚到早期版本。

在安装期间，可以选择**快速设置**或**自定义**。 设置提供期间**自定义**选项是可调整的通过使用组策略，这种方法安装基础操作系统并不重要。


如果选择了**自定义**，可以在安装过程中调整这些设置：

## <a name="in-customize-settings"></a>在"自定义设置"

您还可以调整这些安装使用组策略编辑器; 后请参阅本主题的"组策略设置"部分。

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|**个性化设置**| | |
|个性化语音，键入，并通过向 Microsoft 发送您的输入的数据输入的墨迹书写。|    开| 关闭|
|发送键入和墨迹书写给 Microsoft，以改进该平台，识别和建议的数据。|  开| 关闭|
|允许应用在应用上体验使用广告 ID。|  开| 关闭|
|请让 Skype （如果已安装） 帮助您在通讯簿中连接与朋友并验证你的手机号码。 可能会收取短信和数据的费用。|    开| 关闭|
|**位置**| | |
|打开查找我的设备并让 Windows 和应用程序请求你的位置，包括位置历史记录| 开| 关闭|
|连接性和错误报告| | |
|“自动连接到建议的开放热点”打开或关闭了 WLAN 感知。 并非所有网络都是安全的。|    开| 关闭|
|自动连接到开放热点暂时以查看是否付费的网络服务都可用。| 开| 关闭|
|向 Microsoft 发送完整的诊断和使用情况数据。 关闭此选项仅发送基本的数据。| 开| 关闭|
|**浏览器、 保护和更新**| | |
|使用 SmartScreen 联机服务来帮助防止恶意内容和下载站点加载的 Windows 的浏览器和应用商店应用程序中|    开| 上 （如果没有 Internet 访问权限，然后设置为 Off。）
|使用页面预测提高读取，加快浏览，并在 Windows 浏览器中更好地进行整体体验。 浏览数据将发送给 Microsoft。| 开| 关闭|
|若要提高应用程序和 Windows 更新下载的速度在 Internet 上获取从的更新和发送到其他电脑的更新|   开| 关闭|

安装完成后，你可以继续调整设置开头**Windows 设置**。

## <a name="in-windows-settings"></a>在 Windows 设置
若要访问 Windows 设置，请单击**启动**（Windows 上任务栏图标），然后单击**设置**（形状类似一个齿轮） 图标。

### <a name="in-the-system-area-of-windows-settings"></a>Windows 设置的"系统"区域中
在 Windows 设置区域中单击**系统**图标可让你访问数与系统相关的设置。 并非所有需要调整这些设置是最重要的最佳 VDI 使用-:

#### <a name="apps-and-features"></a>应用程序和功能

若要删除的应用，从而排除从 VDI 映像，单击应用程序中，然后单击**卸载**。 如果**卸载**就会变灰显，您不能删除它通过此方法; 您可能能够使用 Windows PowerShell 中，将它删除，或尝试以下步骤：
1. 单击**管理可选功能**(之下**应用程序和功能**同一页面上标题)。
2. 单击可选功能，然后单击**卸载**。

要考虑删除 （如果存在） 的功能包括：
- **请联系支持人员**
- **英语 （美国） 零售演示内容**
- **非特定零售演示内容**
- **快速帮助**

#### <a name="default-apps"></a>默认应用

此区域定义默认情况下的某些泛型函数，如电子邮件、 浏览 web，并映射使用的应用。 如果你想要用于特定函数的不同应用，单击当前项，然后单击想要在 VDI 映像中使用的应用。 对于非 Microsoft 应用是一个可供选择，必须安装应用之前调整此设置。

#### <a name="notifications-and-actions"></a>通知和操作

通知和 VDI 环境中的后台网络活动，将减少这些建议的值：

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|从应用程序和其他发送方获取通知| 开| 关闭|
|在锁定屏幕上显示的通知。|    开| 关闭|
|在锁定屏幕上显示警告，提醒和传入的 VoIP 呼叫。|   开| 关闭|
|使用 Windows 显示的提示、 技巧和建议。|    开| 关闭|


#### <a name="offline-maps"></a>脱机地图

此设置仅适用于安装的地图应用情况。 其默认值是**上**; 对于 VDI 使用建议的值是**关闭**。 

#### <a name="tablet-mode"></a>平板电脑模式

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|在登录时|    相应的模式用于我的硬件|   使用桌面模式|
|当此设备将自动切换模式下打开或关闭|    切换前总是询问我| 不要询问我并不切换|
|“处于平板模式时隐藏任务栏上的应用图标”|  开| 关闭|


### <a name="in-the-devices-area-of-windows-settings"></a>在"设备"区域中的 Windows 设置
在 Windows 设置区域中单击**设备**图标可让你访问数与系统相关的设置。 并非所有需要调整这些设置是最重要的最佳 VDI 使用-:

#### <a name="autoplay"></a>自动播放

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|为所有媒体和设备使用自动播放|    开| 关闭|
|可移动驱动器：|选择默认值|不执行任何操作|
|内存卡|选择默认值|不执行任何操作|

### <a name="in-the-personalization-area-of-windows-settings"></a>在"个性化"区域中的 Windows 设置
在 Windows 设置区域中单击**个性化设置**图标可让你访问数与系统相关的设置。 并非所有需要调整这些设置是最重要的最佳 VDI 使用-:

#### <a name="background"></a>后台
有时默认的黑色背景就会导致用户认为计算机没有响应。 更改背景颜色可帮助使其更清晰。 要实现这一点，请执行下列操作：
1. 在中**背景**区域中，单击下拉菜单。
2. 若要更改的背景色，请单击**纯色**，然后单击任何黑色之外的颜色。 或者，你可以单击**图片**，然后选择要用作背景的图像。

#### <a name="start"></a>开始时间

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|偶尔在启动时显示建议|    开| 关闭|
|显示最常使用的应用|开|关闭|
|显示最近添加的应用|开|关闭|
|在跳转列表上启动或任务栏中显示最近打开的项|开|关闭|

#### <a name="taskbar"></a>任务栏
默认设置是使用较大的任务栏按钮 (即，值为"关闭"对于**使用小任务栏按钮**)。 此设置会导致要使用大量的任务栏区域的 Cortana 项。 若要避免此问题，设置**使用小任务栏按钮**为"开"。 如果您愿意任务栏项保持更大，但不是愿意让 Cortana 占用得空间中，右键单击任务栏，指向**Cortana**，并在从飞出菜单，选择**Hidden**。

### <a name="in-the-privacy-area-of-windows-settings"></a>Windows 设置的"隐私"区域中
在 Windows 设置区域中单击**隐私**图标可让你访问数与系统相关的设置。 并非所有需要调整这些设置是最重要的最佳 VDI 使用-:

#### <a name="general"></a>常规
其中某些设置还设置从"自定义设置"窗口中，本主题开头所述。

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|允许应用在应用上体验使用我的广告标识符 （关闭此选项将重置你的 ID）|  开| 关闭|
|“允许网站通过访问我的语言列表来提供本地相关内容”|开|关闭|
|让的应用其他的设备上打开应用并在此设备上继续体验|开|关闭|

#### <a name="camera"></a>相机

默认值为"允许应用使用照相机"**上**; 对于 VDI 使用建议的值是**关闭**。


#### <a name="microphone"></a>麦克风

默认值为"允许应用使用我的麦克风"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="notifications"></a>通知

默认值为"允许应用访问我的通知"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="contacts"></a>联系人

默认值为"允许访问我的联系人的应用程序"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="calendar"></a>Calendar

默认值为"允许应用访问我的日历"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="call-history"></a>呼叫历史记录

默认值为"允许应用访问我的通话记录"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="email"></a>电子邮件

默认值为"允许应用访问和发送电子邮件"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="messaging"></a>消息

默认值为"允许应用读取或发送消息 （短信或彩信）"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="radios"></a>无线电收发器

默认值为"允许应用控制无线电收发器"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="other-devices"></a>其他设备

默认值为"可让您的应用程序会自动共享和同步不与 PC、 平板电脑或手机显式配对的无线设备信息"**上**; 对于 VDI 使用建议的值是**关闭**。

#### <a name="feedback-and-diagnostics"></a>反馈和诊断

默认值为"Windows 应寻求我的反馈"**自动**; 对于 VDI 使用建议的值是**从不**。

#### <a name="background-apps"></a>后台应用
列出的应用具有默认值为**上**，这使它们以接收信息、 发送通知，并更新本身是否它们的使用或不可以。 您应该禁用 (设置为**关闭**) 的任何应用不希望在 VDI 图在后台运行。

### <a name="update-and-security"></a>更新和安全
#### <a name="windows-update"></a>Windows 更新
在中**更新设置**区域中，单击**高级选项**调整这些设置：

|设置|默认值|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|为我提供更新的其他 Microsoft 产品时我更新 Windows|    清除|    选择|
|延迟功能更新|清除|选择|
|“使用我的登录信息以在更新后自动完成设备设置” |清除|取决于特定的 VDI 配置|

上**高级选项**页上，单击**选择如何传递更新**访问的设置为"从多个位置的更新"。 默认值是**上**; 对于 VDI 使用建议的值是**关闭**。

## <a name="in-control-panel-and-other-system-utilities"></a>控制面板和其他系统实用程序

在本部分中的设置是通过控制面板中导航或直接打开该实用工具可调整。

> [!NOTE]  
> 本主题中未特别提及的任何设置可以保留在其默认值 （或每个要求和策略设置） 而无需对 VDI 功能明显影响。


### <a name="task-scheduler"></a>任务计划程序
若要打开任务计划程序的最快方法是推送 Windows 按钮并键入*任务计划程序*或*taskschd.msc*。 在结果中返回的单击**任务计划程序**要打开该实用工具。 在任务计划程序中，展开**任务计划程序库**，展开**Microsoft**，然后展开**Windows**。 你现在有权访问任务集合的列表。 要更改每个计划任务的状态，请右键单击它，并单击所需的状态 (通常情况下，**禁用**用于 VDI)。

|任务集合|任务名称|默认状态|用于 VDI 的建议的状态|  
|-------------------|-------------|----------|--------------|
|客户体验改善计划||||
||合并计算器|Enabled|Disabled|
||KernelCeipTask|Enabled|Disabled|
||UsbCeip|Enabled|Disabled|
|碎片整理||||
||ScheduledDefrag|Enabled|Disabled|
|Location||||
||通知|Enabled|Disabled|
||WindowsActionDialog|Enabled|Disabled|
|维护||||
||WinSAT|Enabled|Disabled|
|地图||||
||MapsToastTask|Enabled|Disabled|
||MapsUpdateTask|Enabled|Disabled|
|移动宽带帐户||||
||M n O 元数据解析器|Enabled|Disabled|
|电源效率诊断||||
||分析系统|Enabled|Disabled|
|恢复环境||||
||VerifyWinRE|Enabled|Disabled|
|零售演示||||
||CleanupOfflineContent|Enabled|Disabled|
|壳体||||
||FamilySafetyMonitor|Enabled|Disabled|
||FamilySafetyRefreshTask|Enabled|Disabled|
|Windows 错误报告||||
||QueueReporting|Enabled|Disabled|
|Windows 媒体共享||||
||UpdateLibrary|Enabled|Disabled|

单击**Windows**再次可折叠，然后单击**XblGameSave**。 这样就可以访问的任务**XBLGameSaveTask**并**XBLGameSaveTaskLogon**; 这两个可设置为**禁用**。

### <a name="performance-monitor"></a>性能监视器
若要打开性能监视器的最快方法是推送 Windows 按钮并键入*性能监视器*或*perfmon.msc*。 在结果中返回的单击**性能监视器**。 在性能监视器中，单击**数据收集器集**，然后双击**事件跟踪会话**。 右键单击**WiFiSession**; 如果它处于默认状态**运行**，然后单击**停止**。

单击**StartupEventTraceSessions**，然后右键单击**ReadyBoot**; 如果它正在运行，单击**停止**。 单击**事件跟踪会话**，右键单击**ReadyBoot**，然后单击**属性**。 在打开的对话框，单击**跟踪会话**选项卡。清除**已启用**复选框。

### <a name="services"></a>Services
管理服务的最快方法是将推送 Windows 按钮并键入*services*。 在结果中返回的单击**Services**。 以下服务非常适合用于 VDI 方案; 禁用但是，您可能需要执行一些测试，以验证它们不需要您的要求。 若要禁用服务，在**Services**管理单元中，右键单击服务名称，然后单击**属性**。 上**常规**选项卡上，单击**启动类型**下拉菜单，然后单击**禁用**。 单击 **“确定”**。

- BranchCache
- 传递优化
- 诊断服务主机
- Windows 移动热点服务
- Xbox Live 的身份验证管理器
- Xbox Live 游戏保存
- Xbox Live 网络服务

### <a name="file-explorer-options"></a>文件资源管理器选项
推送 Windows 按钮并键入*控制面板*。 在结果中返回的单击**控制面板**。 在控制面板中，单击**文件资源管理器选项**。 在打开的对话框，单击**搜索**选项卡上，然后在**搜索未索引的位置时**区域中，清除该复选框的**包含系统目录**。 单击**确定**保存。

### <a name="flash-settings"></a>闪光灯设置
推送 Windows 按钮并键入*控制面板*。 在结果中返回的单击**控制面板**。 在控制面板中，单击**Flash Player**打开 Flash 播放器设置管理器。 上**存储**选项卡上，选择的单选按钮**阻止从将信息存储在此计算机上的所有站点**。 在打开的对话框，单击**确定**。

上**照相机和麦克风**选项卡上，在**照相机和麦克风设置**区域中，选择的单选按钮用于**阻止使用照相机和麦克风的所有站点**。

上**播放**选项卡上，在**辅助对等网络**区域中，选择的单选按钮用于**阻止从使用对等辅助网络的所有站点**。 关闭 Flash 播放器设置管理器。

### <a name="internet-options"></a>Internet 选项
推送 Windows 按钮并键入*控制面板*。 在结果中返回的单击**控制面板**。 在控制面板中，单击**Internet 选项**以打开 Internet 属性。 在中**主页上**区域中，输入希望用户看到为在浏览器中主页的网站的 URL。 这可能是用于你的公司的网站或您可以将其设置为空白的主页上输入*有关： 空白*。

在中**浏览历史记录**区域中，选中的复选框**删除浏览历史记录在退出**。

### <a name="power-options"></a>电源选项
推送 Windows 按钮并键入*控制面板*。 在结果中返回的单击**控制面板**。 在控制面板中，单击**电源选项**打开电源选项控制面板。 在中**选择或自定义电源计划**区域中，单击向下的箭头**显示附加计划**，然后选择的单选按钮**高性能**。 此设置将 VDI 主机上具有的影响非常小。

### <a name="system"></a>系统
推送 Windows 按钮并键入*控制面板*。 在结果中返回的单击**控制面板**。 在控制面板中，单击**系统**打开系统控制面板。 在左窗格中，单击**高级系统设置**。 在打开的对话框，单击**高级**选项卡。在中**性能**区域中，单击**设置**按钮，然后在**视觉效果**选项卡中打开时，选择对话框**调整为最佳性能**单选按钮。 单击**确定**保存并退出。

## <a name="group-policy-settings"></a>组策略设置

若要编辑的组策略设置，请按 Windows 按钮并键入*组策略*或*gpedit.msc*。 在结果中返回的单击**编辑组策略**打开本地组策略编辑器。

> [!NOTE]  
> 本主题中未特别提及的任何设置可以保留在其默认值 （或每个要求和策略设置） 而无需对 VDI 功能明显影响。

下**计算机配置**，展开**Windows 设置**，然后展开**安全设置**。 单击**网络列表管理器策略**，然后双击**的所有网络**。 在打开时，在的对话框**网络位置**区域中，选择的单选按钮用于**用户不能更改位置**。 单击**确定**按钮以保存。

折叠**Windows 设置**，然后展开**管理模板**。 单击或展开**网络**，然后调整每个设置，如下所示通过双击它，然后选择所指示的值的单选按钮并单击**确定**按钮：

|设置区域|设置|用于 VDI 的推荐的值|  
|-------------------|-------|----------|
|后台智能传输服务 (BITS)|||
||不允许 BITS 客户端使用 Windows 分支缓存|Enabled|
||不允许计算机充当 BITS 对等缓存客户端|Enabled|
||不允许为 BITS 对等缓存服务器的计算机|Enabled|
||允许 BITS 对等缓存|Disabled|
|BranchCache||
||打开 BranchCache|Disabled|
|热点身份验证||
||启用热点身份验证|Disabled|
|Microsoft 对等方对等网络服务||
||关闭 Microsoft 对等方对等网络服务|Enabled|
|脱机文件||
||允许或禁止使用脱机文件功能|Disabled|

折叠**网络**，然后展开**系统**。 调整每个设置，如下所示双击它，然后选择所指示的值的单选按钮并单击**确定**按钮：

|设置区域|设置|用于 VDI 的推荐的值|  
|-------------------|----------|--------------|
|设备安装||
||不发送 Windows 错误报告时在设备上安装的通用驱动程序|Enabled|
||防止在正常情况下会提示创建还原点的设备活动过程中创建系统还原点|Enabled|
||阻止来自 Internet 的设备元数据检索|Enabled|
||阻止 Windows 发送错误报告时设备驱动程序请求其他软件在安装过程|Enabled|
||在设备安装过程中关闭"发现新硬件"提示框|Enabled|

展开**Filesystem**，双击**NTFS**，双击**短名称创建选项**，选择的单选按钮**已启用**，然后使用**选项**下拉菜单中选择**上的所有卷启用**。 单击**确定**按钮以保存。

折叠**Filesystem**，然后展开**Internet 通信管理**。 单击**Internet 通信设置**。 调整每个设置，如下所示通过双击它，然后选择的单选按钮**已启用**，然后单击**确定**按钮：

- 关闭事件查看器"Events.asp"链接
- 关闭手写个性化数据共享
- 关闭手写识别错误报告
- 关闭帮助和支持中心"你知道吗？" content
- 关闭帮助和支持中心 Microsoft 知识库搜索
- 如果 URL 连接指向 Microsoft.com，则关闭 Internet 连接向导
- 关闭 web 发布和在线订购向导的 Internet 下载
- 关闭 Internet 文件关联服务
- 如果 URL 连接指向 Microsoft.com，则关闭注册
- 关闭"订购打印"照片任务
- 关闭文件和文件夹的"发布到 Web"任务
- 关闭 Windows Messenger 客户体验改善计划
- 关闭 Windows 客户体验改善计划
- 关闭 Windows 错误报告
- 关闭 Windows Update 设备驱动程序搜索

单击**电源管理**，然后双击**选择一个活动的电源计划**。 选择的单选按钮**已启用**，然后使用**选项**下拉菜单中选择**高性能**。 单击**确定**按钮以保存。

单击**恢复**，然后双击**允许还原为默认状态系统的**。 选择的单选按钮**已启用**，然后单击**确定**按钮以保存。

展开**故障排除和诊断**。 单击**计划维护**，双击**配置计划的维护行为**，然后选择的单选按钮**禁用**。 单击**确定**按钮以保存。

对于每个以下设置几个方面，请单击它，然后双击**配置方案的执行级别**，选择的单选按钮**禁用**，然后单击**确定**按钮以保存：

- Windows 启动性能诊断
- Windows 内存泄漏诊断
- Windows 资源耗尽检测和解决方法
- Windows 关机性能诊断
- Windows 待机/恢复性能诊断
- Windows 系统响应能力性能诊断

折叠**系统**，然后展开**Windows 组件**。 调整每个设置，如下所示通过双击它，然后选择所指示的值的单选按钮并单击**确定**按钮：

|设置区域|设置|用于 VDI 的推荐的值|  
|-------------------|-------|----------|
|将功能添加到 Windows 10|||
||阻止运行向导|Enabled|
|自动播放策略|||
||设置为自动运行的默认行为|已启用，然后使用**选项**下拉菜单中选择**不执行任何自动运行命令**|
|云内容|||
||不显示 Windows 提示|Enabled|
||关闭 Microsoft 消费者体验|Enabled|
|数据收集和预览版本|||
||允许遥测|已启用，然后使用**选项**下拉菜单中选择**1-基本**|
||禁用预发行版的功能或设置|     Disabled|
||请不要显示反馈通知|       Enabled|
||切换对会员版本的用户控制|      Disabled|
|桌面窗口管理器|||
||不允许 Flip3D 调用|       Enabled|
||不允许的窗口动画|       Enabled|
||使用纯色，以开始背景|     Enabled|
|边缘 UI|||
||允许边缘轻扫|     Disabled|
||禁用帮助提示|        Enabled|
|文件资源管理器|||
||不显示安装新应用程序通知|     Enabled|
|游戏资源管理器|||
||关闭下载游戏信息|     Enabled|
||关闭游戏更新|        Enabled|
||关闭游戏文件夹中的游戏的上次播放时间的跟踪|     Enabled|
|“家庭组”|||
||阻止在计算机加入家庭组|        Enabled|
|Internet Explorer|||
||允许 Microsoft 服务在用户向地址栏中键入内容时提供增强建议|        Disabled|
||禁用 Internet Explorer 软件更新的定期检查|        Enabled|
||禁用显示初始屏幕|        Enabled|
||自动安装新版本的 Internet Explorer|      Disabled|
||防止参与客户体验改善计划|     Enabled|
||正在运行的首次运行向导 Go 防止直接到主页|   已启用，然后使用**选项**下拉菜单中选择**直接转到主页**|
||设置选项卡上的进程增长|已启用，然后键入以下内容中的**选项卡上的进程增长**框：*低*。|
||指定一个新选项卡的默认行为|已启用，然后使用**选项**下拉菜单中选择**新选项卡页**|
||关闭加载项性能通知|        Enabled|
||关闭浏览器地理位置|     Enabled|
||关闭重新打开上一次浏览会话|        Enabled|
||关闭所有用户安装提供程序的建议|        Enabled|
||开启建议站点|       Disabled|

为同一级别**Internet Explorer**只是上表中进行调整的设置，请注意另一个级别的范围从文件夹**加速器**到**工具栏**。 换而言之，你目前尚在本地计算机策略 > 计算机配置 > 管理模板 > Windows 组件 > Internet 资源管理器。 

打开**删除浏览历史记录**文件夹中，双击**允许删除浏览历史记录在退出**，选择**启用**，然后单击**确定**保存并退出。

使用右上角的后退箭头左上的本地组策略编辑器若要回到**Internet Explorer**级别。 双击**Internet 设置**，双击**高级设置**，然后调整子文件夹中的设置，如下所示：

|设置文件夹下的**高级设置**|设置|用于 VDI 的推荐的值|  
|-------------------|-------|----------|
|**浏览**|||
||关闭电话号码检测|Enabled|
|**多媒体**|||
||允许 Internet Explorer 播放媒体文件使用其他编解码器|Disabled|

备份到的级别转**Internet Explorer**，然后双击**Internet 设置**。 在此文件夹中，将设置下的这两个设置**记忆式键入功能**到**已启用**:

- 关闭 URL 的建议
- 关闭 Windows Search 记忆式键入功能

备份到四个级别转**Windows 组件**，双击**位置和传感器**，然后将这三种设置设置为**已启用**(每个，请单击**好了**保存并退出):

- 关闭位置
- 关闭脚本位置
- 关闭传感器

级别的段**位置和传感器**，双击**Windows 位置提供程序**并设置**关闭 Windows 位置提供程序**到**已启用**. 单击**确定**保存并退出。

在左窗格中，单击**Maps**，将这些设置设置为**已启用**; 对于每个，然后单击**确定**保存并退出：

- 关闭地图数据的自动下载和更新
- 在“脱机地图”设置页面关闭未经请求的网络流量

使用左窗格中，输入以下设置子文件夹的每个并调整各个设置，如下所示：

|设置文件夹下的**Windows 组件**|设置|用于 VDI 的推荐的值|  
|-------------------|-------|----------|
|**OneDrive**|||
||禁止使用 OneDrive 进行文件存储|Enabled|
||默认情况下将文档保存到 OneDrive|Disabled|
|**RSS 源**|||
||阻止自动发现的源和网页快讯|Enabled|
|**搜索**|||
||允许使用 Cortana|        Disabled|
||锁定屏幕上方允许 Cortana|      Disabled|
||允许搜索和 Cortana 使用位置|     Disabled|
||不允许 Web 搜索|      Enabled|
||不要在 web 上搜索或在搜索中显示 web 结果|        Enabled|
||阻止添加 UNC 位置，以便从控制面板编制索引|     Enabled|
||防止脱机文件缓存中的文件编制索引|        Enabled|
|**应用商店**|||
||关闭到最新版本的 Windows 更新的产品/服务|Enabled|
|**Windows 错误报告**|||
||自动发送 OS 生成错误报告的内存的转储|       Disabled|
||禁用 Windows 错误报告|      Enabled|
|**Windows 安装程序**|||
||控件的基线文件缓存的最大大小|  已启用，然后使用在 spinbox**选项**区域设置**基线文件缓存最大大小**到*5*。|
||关闭系统还原检查点的创建|      Enabled|
|**Windows Mail**|||
||关闭社区功能|Enabled|
|**Windows Media Player**|||
||不显示首次使用对话框|       Enabled|
||防止媒体共享|        Enabled|
|**Windows 移动中心**|||
||关闭 Windows 移动中心|Enabled|
|**Windows 可靠性分析**|||
||配置可靠性 WMI 提供程序|Disabled|
|**Windows 更新**|||
||允许自动更新立即安装|       Enabled|
||删除所有 Windows 更新功能的访问权限|     Enabled|
|在中**Windows Update**文件夹中，打开**延迟 Windows 更新**|||
||选择时收到功能更新|已启用，然后在**选项**区域中，使用**选择你想要接收功能更新分支就绪级别**下拉菜单以选择**Current Branch for Business**. 设置**发布功能更新后，将推迟接收此天数**到 spinbox *180 天*。
||选择时收到质量更新|已启用，然后在**选项**区域中，设置**质量更新发布后，将推迟接收此天数**到 spinbox *30 天*并选中复选框**暂停质量更新**。

在本地组策略编辑器的左窗格中，单击**用户配置**。 使用左窗格中，单击**管理模板**然后输入以下设置子文件夹的每个并调整各个设置，如下所示：

|设置文件夹下的**管理模板**|设置|用于 VDI 的推荐的值|  
|-------------------|-------|----------|
|**桌面**|||
||不将共享的最近打开的文档添加到网络位置|Enabled|
|在中**桌面**文件夹中，打开**Active Directory**|||
||Active Directory 搜索的最大大小|已启用，然后在**选项**区域中，使用 spinbox 设置**返回的对象数**到*5000*。|
|**启动 Manu 和任务栏**|||
||清除最近的程序列表的新用户|     Enabled|
||不在跳转列表中显示或跟踪来自远程位置的项目|        Enabled|
||关闭功能广告气球通知|     Enabled|
||关闭用户跟踪|       Enabled|
|在中**开始菜单和任务栏**文件夹中，打开**通知**|||
||关闭 toast 通知|Enabled|
|在中**Windows 组件**文件夹中，打开：|||
|**云内容**|||
||关闭所有的 Windows 聚焦功能|Enabled|
|**文件资源管理器**|||
||关闭缓存的缩略图的图片|       Enabled|
||关闭文件资源管理器搜索框中的新搜索条目显示|        Enabled|
||关闭隐藏的 thumbs.db 文件中的缩略图缓存|      Enabled|

## <a name="microsoft-store-apps"></a>Microsoft Store 应用
有多种可能想要从 VDI 映像; 中删除的 Microsoft Store 应用删除它们将减少 CPU 使用情况和节省磁盘空间。 删除的良好候选项包括：

- 获取 Office
- Skype （预览版）
- 获取启动 （尤其是如果没有 Internet 连接）
- 反馈中心
- Microsoft 纸牌集合
- 付费的 Wi-fi 和移动电话

若要自定义用于创建 VDI 映像使用的默认用户配置文件，请使用内置管理员帐户。 如果尚未启用，请在计算机管理中使用本地用户和组。 然后登录到管理员帐户来完成以下步骤。

> [!NOTE]  
> 不删除系统应用程序，如应用商店应用程序。 人们很难重新安装。 其他应用程序从应用商店是轻松地 reinstallable。

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>从管理员用户配置文件中删除不需要的应用
1. 在 Windows PowerShell 中，运行`Get-AppxPackage | ft PackageFamilyName`若要查看已安装的应用列表。
2. 对于每个你想要卸载的应用包生成工具运行 cmdlet 的此示例格式：

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>删除不需要的应用商店应用的有效负载
这将防止重新安装应用。
1. 列表应用商店应用和其他项的已预配存储使用此 cmdlet 中的数据： `Get-AppxProvisionedPackage -Online`。
2. 删除与给定的包`Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`，使用相应 MyAppPackage 返回步骤 1 中。 例如，若要删除的 Zune 相关的包，将运行`Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`。

## <a name="removing-other-items"></a>删除其他项
可以删除 OneDrive 图标和应用程序、 关闭系统图标，并删除已下载的更新。

### <a name="remove-onedrive-icon-and-app"></a>删除 OneDrive 图标和应用程序
1. 单击**启动**并滚动到**OneDrive**图标。
2. 右键单击**OneDrive**图标，指向**详细**，然后单击**打开文件位置**。
3. 右键单击**OneDrive**中的文件位置和单击图标**删除**。

若要删除的 OneDrive 应用：
1. 单击**启动**并滚动到**OneDrive**图标。
2. 右键单击**OneDrive**图标，，然后单击**卸载**。 程序和功能随即打开。
3. 在程序和功能中，右键单击**Microsoft OneDrive**然后单击**卸载**。

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>程序和功能 （从早期版本的控制面板中）
1. 推送**启动**按钮，键入*控制*，然后按 ENTER。
2. 点击或双击**程序和功能**。
3. 最左侧下**控制面板主页**，点击或单击**打开或关闭打开的 Windows 功能**。 将打开一个新用户界面。
4. 清除不想或不需要在基本映像，例如任何项对应的复选框：**SMB 1.0/CIFS 文件共享支持**。

### <a name="turn-system-icons-off"></a>关闭系统图标
1. 推送或单击**启动**，然后单击**设置**（齿轮图标）。
2. 在中**查找设置**文本区域中，键入*任务栏*，然后单击**任务栏设置**。
3. 下**任务栏**部分中，向下滚动或向下轻扫**通知区域**部分。
4. 单击或点击**打开或关闭系统图标**，并重新打开每个系统图标打开或关闭以您选择的映像。

### <a name="delete-downloaded-updates"></a>删除下载的更新
1. 使用文件资源管理器，导航到**C:\Windows\Software Distribution\Download**。
2. 删除所有文件和文件夹在该目录中。













 













---
title: "配置其他 LSA 保护"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>配置其他 LSA 保护

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员说明如何配置本地安全颁发机构 (LSA) 进程阻止代码可能损害凭据额外的保护。

LSA，其中包括本地安全颁发机构服务器服务 (LSASS) 的过程，验证本地边远登录的用户，并执行本地安全策略。 Windows 8.1 操作系统提供了更多保护以防止阅读内存和代码由不受保护的进程插入 LSA。 这提供 LSA 存储，并管理的凭据增加的安全性。 可以在 Windows 8.1 配置 LSA 的受保护的进程设置，但它无法配置 Windows RT 8.1 中。 安全启动的结合使用时此设置，因为禁用 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 注册表项不产生任何影响实现额外的保护。

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>保护的进程要求插件或驱动程序
LSA 插件或驱动程序来保护进程作为可以成功加载，必须满足以下条件：

1.  签名验证

    保护的模式要求，任何插件载入 LSA 使用进行数字签名 Microsoft 签名。 因此，任何插件未签名或与 Microsoft 签名未签名将无法在 LSA 中加载。 这些插件的示例包括智能卡驱动程序、 加密插件和密码筛选器。

    LSA 插件是驱动程序，如智能卡驱动程序，需要使用 WHQL 证书登录。 有关详细信息，请参阅[WHQL 发布签名 （Windows 驱动程序）](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx)。

    LSA 插件不具有 WHQL 认证进程，必须登录使用[文件签名服务 LSA](https://go.microsoft.com/fwlink/?LinkId=392590)。

2.  遵守 Microsoft 安全开发生命周期 (SDL) 过程指南

    所有插件必须遵守适用 SDL 过程指南。 有关详细信息，请参阅[Microsoft 安全开发生命周期 (SDL) 附录](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx)。

    即使 Microsoft 签名正确签名插件，非遵守 SDL 过程可能导致失败加载插件。

#### <a name="recommended-practices"></a>建议的最佳实践
使用下面的列表，若要全面测试该 LSA 保护已启用之前广泛地部署功能：

-   确定的所有 LSA 插件和中使用你的组织的驱动程序。 
    这包括非 Microsoft 驱动程序或插件如智能卡驱动程序和加密插件和任何内部开发了用于强制密码筛选器或密码更改通知的软件。

-   确保所有 LSA 插件都进行数字签名与 Microsoft 证书，以便插件将不无法加载。

-   确保，所有正确签名插件可以成功加载到 LSA 和它们按预期执行。

-   使用审核日志识别 LSA 插件和失败作为受保护的流程来运行的驱动程序。

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>如何识别 LSA 插件和失败作为受保护的流程来运行的驱动程序
在此部分中所述的事件位于运营下的应用程序和服务 Logs\Microsoft\Windows\CodeIntegrity 日志。 他们可以帮助你确定 LSA 插件和加载由于登录的原因失败的驱动程序。 若要管理这些事件，你可以使用**wevtutil**命令行工具。 有关此工具的信息，请参阅[Wevtutil](../../administration/windows-commands/Wevtutil.md)。

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>之前中选择： 如何确定插件和加载 lsass.exe 由驱动程序
你可以使用审核模式找出 LSA 插件和驱动程序将无法加载 LSA 保护模式中。 在审核模式时，系统将生成事件日志、 识别的所有插件和驱动程序将无法加载 LSA 下，如果 LSA 保护已启用。 电子邮件内容将记录不会阻止的插件或驱动程序。

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>若要启用审核模式 Lsass.exe 为一台计算机上编辑注册表

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于的注册表项： HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 文件执行 Options\LSASS.exe。

2.  设置的值的注册表项到**AuditLevel = dword:00000008**。

3.  重启计算机。

分析的结果事件 3065 和 3066 事件。

-   **事件 3065**： 此事件录制的代码完整性检查确定进程 (通常 lsass.exe) 试图加载的特定的驱动程序不符合共享部分的安全要求。 但是，设置系统策略，由于图像已允许加载。

-   **事件 3066**： 此事件录制，决定尝试进程 (通常 lsass.exe) 加载不符合登录级别要求 Microsoft 的特定驱动程序的代码完整性检查。 但是，设置系统策略，由于图像已允许加载。

> [!IMPORTANT]
> 在内核调试程序已连接，并在系统上启用时，不会生成这些操作的事件。
> 
> 如果的插件或驱动程序包含共享部分、 事件 3066 将记录事件 3065 与。 删除共享部分应防止这两个事件发生除非插件未满足 Microsoft 登录级别要求。

若要启用某个域中的多台计算机的审核模式，你可以使用组策略注册表客户端扩展部署 Lsass.exe 审核级别注册表值。 你需要进行修改 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 文件执行 Options\LSASS.exe 注册表项。

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>若要在 GPO 创建 AuditLevel 值设置

1.  打开组策略管理控制台 (GPMC)。

2.  创建一个新组策略对象 (GPO) 链接域级别，或与部门包含你的帐户。 或者，你可以选择已经部署 GPO。

3.  GPO，右键单击，然后单击**编辑**组策略管理编辑器中打开。

4.  展开**计算机配置**，展开**首选项**，然后展开**Windows 设置**。

5.  右键单击**注册表**，指向**新建**，然后单击**注册表项**。 **新注册表属性**显示对话框。

6.  在**配置单元**列表中，单击**HKEY_LOCAL_MACHINE。**

7.  在**关键路径**列表中，浏览到**SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image 文件执行 Options\LSASS.exe**。

8.  在**值名称**框中，键入**AuditLevel**。

9. 在**值类型**框中，单击以选择**REG_DWORD**。

10. 在**值数据**框中，键入**00000008**。

11. 单击**确定**。

> [!NOTE]
> Gpo 生效，必须对所有域控制器在域中复制 GPO 更改。

若要在多台计算机上的其他 LSA 保护，你可以使用注册表客户端扩展组策略为通过修改 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。 有关如何执行此操作的步骤，请参阅[如何配置额外的凭据 LSA 保护](#BKMK_HowToConfigure)本主题中。

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>后中选择： 如何确定插件和加载 lsass.exe 由驱动程序
你可以使用事件日志识别 LSA 插件和加载 LSA 保护模式失败的驱动程序。 启用 LSA 受保护的过程后，系统将生成识别所有插件和加载下 LSA 失败的驱动程序的事件日志。

分析的结果事件 3033 和事件 3063。

-   **事件 3033**： 此事件录制，决定尝试进程 (通常 lsass.exe) 加载不符合登录级别要求的 Microsoft 驱动程序的代码完整性检查。

-   **事件 3063**： 此事件录制的代码完整性检查确定进程 (通常 lsass.exe) 试图加载的驱动程序不符合共享部分的安全要求。

共享的部分通常是编程允许实例使用相同的安全上下文其他进程与进行交互的数据的技术的结果。 这可以创建安全漏洞。

## <a name="BKMK_HowToConfigure"></a>如何配置额外的凭据 LSA 保护
在设备上运行 Windows 8.1 （或不安全启动或 UEFI），配置是可能通过执行此部分中所述的过程。 对于运行 Windows RT 8.1 的设备，始终启用 lsass.exe 保护，，并且无法关闭它。

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>基于 x86 或基于 x64 的设备上使用安全启动和 UEFI 或不
基于 x86 或基于 x64 的使用安全启动和 UEFI 的设备，UEFI 变量设置 UEFI 固件中启用 LSA 保护后使用注册表项。 当设置固件中存储时，无法删除或更改注册表项中 UEFI 变量。 必须重置 UEFI 变量。

基于 x86 的或者基于 x64 的设备不支持 UEFI 或安全启动都已禁用，不能的固件中存储的配置 LSA 保护和依靠注册表项中是否存在。 在此情况下，就可以通过使用远程设备访问，禁用 LSA 保护。

你可以使用下面的过程启用或禁用 LSA 保护：

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>若要启用 LSA 保护一台计算机上

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于的注册表项： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  设置的注册表项到值:"RunAsPPL"= dword:00000001。

3.  重启计算机。

##### <a name="to-enable-lsa-protection-using-group-policy"></a>若要启用 LSA 保护使用组策略

1.  打开组策略管理控制台 (GPMC)。

2.  创建一个新 GPO 链接域级别，或与部门包含你的帐户。 或者，你可以选择已经部署 GPO。

3.  GPO，右键单击，然后单击**编辑**组策略管理编辑器中打开。

4.  展开**计算机配置**，展开**首选项**，然后展开**Windows 设置**。

5.  右键单击**注册表**，指向**新建**，然后单击**注册表项**。 **新注册表属性**显示对话框。

6.  在**配置单元**列表中，单击**HKEY_LOCAL_MACHINE**。

7.  在**关键路径**列表中，浏览到**SYSTEM\CurrentControlSet\Control\Lsa**。

8.  在**值名称**框中，键入**RunAsPPL**。

9. 在**值类型**框中，单击**REG_DWORD**。

10. 在**值数据**框中，键入**00000001**。

11. 单击**确定**。

##### <a name="to-disable-lsa-protection"></a>若要禁用 LSA 保护

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于的注册表项： HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  从的注册表项删除的以下值:"RunAsPPL"= dword:00000001。

3.  使用本地安全颁发机构 (LSA) 保护的进程选择退出工具删除 UEFI 变量，如果设备在使用安全启动。

    有关选择退出工具的详细信息，请参阅[下载的本地安全颁发机构 (LSA) 保护的过程退出从 Microsoft 官方下载中心](https://www.microsoft.com/download/details.aspx?id=40897)。

    有关管理安全启动的详细信息，请参阅[UEFI 固件](https://technet.microsoft.com/library/hh824898.aspx)。

    > [!WARNING]
    > 安全启动处于关闭状态，当重的安全启动和 UEFI 相关的配置。 仅在所有其他方式来禁用 LSA 保护已失败时，你应关闭安全启动。

### <a name="verifying-lsa-protection"></a>验证 LSA 保护
发现如果 LSA 启动在保护模式下启动 Windows，搜索中的以下 WinInit 事件**系统**下登录**Windows 日志**:

-   12: LSASS.exe 启动作为受保护的进程与级别： 4

## <a name="additional-resources"></a>更多资源
[凭据保护和管理](credentials-protection-and-management.md)

[文件 LSA 签名服务](https://go.microsoft.com/fwlink/?LinkId=392590)



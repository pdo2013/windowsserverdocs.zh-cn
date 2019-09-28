---
title: 配置其他 LSA 保护
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: eaebac19119525b659c09b5506c497afdbd9a263
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386989"
---
# <a name="configuring-additional-lsa-protection"></a>配置其他 LSA 保护

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题面向 IT 专业人员，介绍如何为本地安全机构 (LSA) 进程配置附加保护，以防止发生损害凭据安全性的代码注入。

LSA 包含本地安全机构服务器服务 (LSASS) 进程，可以验证用户的本地和远程登录，并强制本地安全策略。 Windows 8.1 操作系统为 LSA 提供附加保护，以防止未受保护的进程读取内存和代码注入。 这为 LSA 存储和管理的凭据提供了更高的安全性。 LSA 的受保护进程设置可以在 Windows 8.1 中配置，但不能在 Windows RT 8.1 中进行配置。 将此设置与安全启动结合使用时，便可以实现附加保护，因为禁用 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 注册表项不起作用。

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>插件或驱动程序的受保护进程要求
要使 LSA 插件或驱动程序以受保护进程的形式成功加载，它必须符合以下条件：

1.  签名验证

    保护模式要求加载到 LSA 中的任何插件都已使用 Microsoft 签名进行数字签名。 因此，未签名的或者未使用 Microsoft 签名进行签名的任何插件都无法加载到 LSA 中。 这些插件的示例包括智能卡驱动程序、加密插件和密码筛选器。

    用作驱动程序（例如智能卡驱动程序）的 LSA 插件需要使用 WHQL 认证进行签名。 有关详细信息，请参阅[WHQL 发行版签名](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx)。

    不需要经历 WHQL 认证过程的 LSA 插件必须使用 [LSA 的文件签名服务](https://go.microsoft.com/fwlink/?LinkId=392590)进行签名。

2.  遵守 Microsoft 安全开发生命周期 (SDL) 过程指导

    所有插件必须符合适用的 SDL 过程指导。 有关详细信息，请参阅 [Microsoft 安全开发生命周期 (SDL) 附录](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx)。

    即使插件已使用 Microsoft 签名正确地进行签名，但如果不符合 SDL 过程，也可能会导致加载插件失败。

#### <a name="recommended-practices"></a>建议的做法
在广泛部署该功能之前，请使用以下列表来全面测试是否已启用 LSA 保护：

-   识别组织中使用的所有 LSA 插件和驱动程序。 
    这包括非 Microsoft 驱动程序或插件（例如智能卡驱动程序和加密插件），以及内部开发的、用于强制密码筛选器或密码更改通知的所有软件。

-   确保使用 Microsoft 证书对所有 LSA 插件进行数字签名，以防止插件加载失败。

-   确保正确签名的所有插件都能成功加载到 LSA 中，并且能按预期工作。

-   使用审核日志来识别无法以受保护进程运行的 LSA 插件和驱动程序。

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>启用 LSA 保护引入的限制

如果启用了 LSA 保护，则无法调试自定义 LSA 插件。
如果调试程序是受保护的进程，则不能将调试程序附加到它。
一般情况下，不支持调试正在运行的受保护进程。

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>如何识别无法以受保护进程运行的 LSA 插件和驱动程序
本部分所述的事件位于 Applications and Services Logs\Microsoft\Windows\CodeIntegrity 下的运行日志中。 这些事件可帮助你识别由于签名方面的原因而无法加载的 LSA 插件和驱动程序。 若要管理这些事件，可使用 **wevtutil** 命令行工具。 有关此工具的信息，请参阅 [Wevtutil](../../administration/windows-commands/Wevtutil.md)。

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>在选择加入之前：如何识别 lsass.exe 加载的插件和驱动程序
可以使用审核模式来识别 LSA 保护模式下无法加载的 LSA 插件和驱动程序。 在审核模式下，系统将生成事件日志，标识在启用 LSA 保护的情况下无法在 LSA 下加载的所有插件和驱动程序。 将会记录消息，而不阻止这些插件或驱动程序。

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>在一台计算机上通过编辑注册表为 Lsass.exe 启用审核模式的步骤

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于以下位置的注册表项：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe。

2.  将该注册表项的值设置为 **AuditLevel=dword:00000008**。

3.  重新启动计算机。

分析事件 3065 和事件 3066 的结果。

完成此操作后，你可能会在事件查看器中看到这些事件：Microsoft Codeintegrity/操作：

-   **事件 3065**：此事件记录：代码完整性检查已确定某个进程（通常为 lsass.exe）尝试加载特定的驱动程序，但该驱动程序不符合共享区域的安全要求。 但是，由于所设置的系统策略的原因，允许加载相应的映像。

-   **事件 3066**:此事件记录：代码完整性检查已确定某个进程（通常为 lsass.exe）尝试加载特定的驱动程序，但该驱动程序不符合 Microsoft 签名级别要求。 但是，由于所设置的系统策略的原因，允许加载相应的映像。

> [!IMPORTANT]
> 如果在系统上附加并启用了内核调试程序，则不生成这些操作事件。
> 
> 如果插件或驱动程序包含共享区域，则会同时记录事件 3066 和事件 3065。 除非插件不符合 Microsoft 签名级别要求，否则，删除共享区域应可防止发生这两个事件。

若要为域中的多台计算机启用审核模式，可以使用组策略的注册表客户端扩展来部署 Lsass.exe 审核级别注册表值。 需要修改 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe 注册表项。

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>在 GPO 中创建 AuditLevel 值设置的步骤

1.  打开组策略管理控制台 (GPMC)。

2.  创建一个新的组策略对象 (GPO)，该对象在域级别链接，或者链接到你的计算机帐户所在的组织单位。 你也可以选择已部署的 GPO。

3.  右键单击该 GPO，然后单击“编辑”打开组策略管理编辑器 。

4.  依次展开“计算机配置”、“首选项”和“Windows 设置”。

5.  右键单击 **“注册表”** ，指向 **“新建”** ，然后单击 **“注册表项”** 。 此时将出现“新建注册表属性”对话框 。

6.  在 **“配置单元”** 列表中单击 **HKEY_LOCAL_MACHINE.**

7.  在 **“注册表项路径”** 列表中浏览到 **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**。

8.  在 **“值名称”** 框中键入 **AuditLevel**。

9. 在 **“值类型”** 框中，通过单击选择 **“REG_DWORD”** 。

10. 在 **值数据** 框中，键入 **00000008**。

11. 单击 **“确定”** 。

> [!NOTE]
> 要使该 GPO 生效，必须将 GPO 更改复制到域中的所有域控制器。

若要在多台计算机上选择加入附加 LSA 保护，可以通过修改 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa 来使用组策略的注册表客户端扩展。 有关如何执行此操作的步骤，请参阅本主题的 [如何配置凭据的附加 LSA 保护](#BKMK_HowToConfigure) 。

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>选择加入之后：如何识别 lsass.exe 加载的插件和驱动程序
可以使用事件日志来识别 LSA 保护模式下无法加载的 LSA 插件和驱动程序。 启用 LSA 受保护进程后，系统将生成事件日志，标识无法在 LSA 下加载的所有插件和驱动程序。

分析事件 3033 和事件 3063 的结果。

完成此操作后，你可能会在事件查看器中看到这些事件：Microsoft Codeintegrity/操作：

-   **事件 3033**:此事件记录：代码完整性检查已确定某个进程（通常为 lsass.exe）尝试加载某个驱动程序，该驱动程序不符合 Microsoft 签名级别要求。

-   **事件 3063**:此事件记录：代码完整性检查已确定某个进程（通常为 lsass.exe）尝试加载某个驱动程序，该驱动程序不符合共享区域的安全要求。

共享区域通常是运用某些编程技术的后果，这些技术允许实例数据与使用相同安全上下文的其他进程交互。 这可能会造成安全漏洞。

## <a name="BKMK_HowToConfigure"></a>如何配置凭据的附加 LSA 保护
在运行 Windows 8.1 的设备（有或没有安全启动或 UEFI）上，可以通过执行本部分中所述的过程来进行配置。 对于运行 Windows RT 8.1 的设备，lsass.exe 保护始终处于启用状态，并且不能关闭。

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>在使用或不使用安全启动和 UEFI 的基于 x86 或基于 x64 的设备上
在使用安全启动和 UEFI 的基于 x86 或基于 x64 的设备上，使用注册表项启用 LSA 保护后，将在 UEFI 固件中设置一个 UEFI 变量。 在固件中存储设置后，无法在注册表项中删除或更改该 UEFI 变量， 而只能重新设置它。

不支持 UEFI 或安全启动的基于 x86 或 x64 的设备将被禁用，无法在固件中存储 LSA 保护的配置，并且完全依赖于注册表项的存在状态。 在此情况下，可以使用对设备的远程访问权限来禁用 LSA 保护。

可以使用以下过程来启用或禁用 LSA 保护：

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>在一台计算机上启用 LSA 保护的步骤

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于以下位置的注册表项：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  将该注册表项的值设置为："RunAsPPL"=dword:00000001。

3.  重新启动计算机。

##### <a name="to-enable-lsa-protection-using-group-policy"></a>使用组策略启用 LSA 保护的步骤

1.  打开组策略管理控制台 (GPMC)。

2.  创建一个新 GPO，该 GPO 在域级别链接，或者链接到你的计算机帐户所在的组织单位。 你也可以选择已部署的 GPO。

3.  右键单击该 GPO，然后单击“编辑”打开组策略管理编辑器 。

4.  依次展开“计算机配置”、“首选项”和“Windows 设置”。

5.  右键单击 **“注册表”** ，指向 **“新建”** ，然后单击 **“注册表项”** 。 此时将出现“新建注册表属性”对话框 。

6.  在“配置单元” 列表中，单击 **HKEY_LOCAL_MACHINE**。

7.  在“注册表项路径” 列表中，浏览到 **SYSTEM\CurrentControlSet\Control\Lsa**。

8.  在“值名称” 框中，键入 **RunAsPPL**。

9. 在“值类型”框中，单击“REG_DWORD”。

10. 在“值数据” 框中，键入 **00000001**。

11. 单击 **“确定”** 。

##### <a name="to-disable-lsa-protection"></a>禁用 LSA 保护的步骤

1.  打开注册表编辑器 (RegEdit.exe)，然后导航到位于以下位置的注册表项：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa。

2.  从注册表项删除以下值："RunAsPPL"=dword:00000001。

3.  如果设备使用了安全启动，请使用本地安全机构 (LSA) 受保护进程选择退出工具来删除 UFEI 变量。

    有关选择退出工具的详细信息，请参阅 [从官方 Microsoft 下载中心下载本地安全机构 (LSA) 受保护进程退出](https://www.microsoft.com/download/details.aspx?id=40897)。

    有关管理安全启动的详细信息，请参阅 [UEFI 固件](https://technet.microsoft.com/library/hh824898.aspx)。

    > [!WARNING]
    > 关闭安全启动后，将会重置所有与安全启动和 UEFI 相关的配置。 仅当禁用 LSA 保护的所有其他方法均已失败时，才应关闭安全启动。

### <a name="verifying-lsa-protection"></a>验证 LSA 保护
若要发现 Windows 启动时是否在保护模式下启动 LSA，请搜索“系统”日志下的“Windows 日志”中的以下 WinInit 事件:

-   12：LSASS.exe 已作为具有以下级别的受保护进程启动：4

## <a name="additional-resources"></a>其他资源
[凭据保护和管理](credentials-protection-and-management.md)

[LSA 的文件签名服务](https://go.microsoft.com/fwlink/?LinkId=392590)



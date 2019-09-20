---
title: 将 Windows Server 2008 R2 升级到 Windows Server 2012 R2 |Microsoft Docs
description: 了解如何执行就地升级，从 Windows Server 2008 R2 升级到 Windows Server 2012 R2。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: d5051239f7269eb4b6361187121ac960e06f6d9e
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125077"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>将 Windows Server 2008 R2 升级到 Windows Server 2012 R2

如果要保留相同的硬件和已设置的所有服务器角色而不平展服务器，则需要执行就地升级。 就地升级允许从较旧的操作系统切换到较新的操作系统，同时使设置、服务器角色和数据保持不变。 本文可帮助你从 Windows Server 2008 R2 迁移到 Windows Server 2012 R2。

若要升级到 Windows Server 2019，请先使用本主题升级到 Windows Server 2012 R2，然后[从 Windows server 2012 r2 升级到 Windows server 2019](upgrade-2012r2-to-2019.md)。

## <a name="before-you-begin-your-in-place-upgrade"></a>开始就地升级之前

在开始 Windows Server 升级之前，建议你从设备收集一些信息，以便进行诊断和故障排除。 因为此信息仅供升级失败时使用，所以必须确保将信息存储在可从设备访问的某个位置。

### <a name="to-collect-your-info"></a>收集信息

1. 打开命令提示符，中转到`c:\Windows\system32`，然后键入**systeminfo.exe**。

2. 将生成的系统信息复制、粘贴并存储在设备之外的某个位置。

3. 在命令提示符下键入**ipconfig/all** ，然后将生成的配置信息复制并粘贴到上述位置。

4. 打开注册表编辑器，中转到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然后将 Windows Server **BuildLabEx** （版本）和**EditionID** （edition）复制并粘贴到上述位置。

收集了所有与 Windows Server 相关的信息后，我们强烈建议你备份操作系统、应用程序和虚拟机。 你还必须**关闭**、**快速迁移**或**实时迁移**当前正在服务器上运行的所有虚拟机。 在就地升级过程中，不能运行任何虚拟机。

## <a name="to-perform-the-upgrade"></a>执行升级

1. 请确保**BuildLabEx**值表明你正在运行 Windows Server 2008 R2。

2. 找到 Windows Server 2012 R2 安装媒体，**然后选择 "setup.exe"** 。

    ![显示 setup.exe 文件的 Windows 资源管理器](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. 选择 **"是"** 以启动安装过程。

    ![要求用户提供权限来启动安装程序的用户帐户控制](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. 在 Windows Server 2012 R2 屏幕上，选择 "**立即安装**"。

5. 对于连接到 internet 的设备，选择 "**联机" 以立即安装更新（推荐）** 。

    ![用于选择联机以获取重要 Windows 更新的屏幕](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. 选择要安装的 Windows Server 2012 R2 版本，然后选择 "**下一步**"。

    ![用于选择要安装的 Windows Server 2012 R2 edition 的屏幕](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. 选择 "**我接受许可条款**" 以接受许可协议条款，基于分发渠道（例如零售、批量许可、OEM、ODM 等），然后选择 "**下一步**"。

    ![接受许可协议的屏幕](media/upgrade-2008r2-2012r2/license-terms.png)

8. 选择 **"升级"：安装 Windows 并保留文件、设置和应用程序** ，以选择执行就地升级。

    ![用于选择安装类型的屏幕](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. 安装程序将提醒你使用[Windows server 安装和升级](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade)一文中的信息，确保你的应用程序与 windows Server 2012 R2 兼容，然后选择 "**下一步**"。

    ![屏幕提醒你检查应用程序兼容性](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. 如果你看到通知你不推荐升级的页面，则可以将其忽略，然后选择 "**确认**"。 它已设置为提示进行全新安装，但不是必需的。

    ![显示升级进度的屏幕](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    就地升级将开始，其中显示了 "**升级 Windows** " 屏幕的进度。 升级完成后，服务器将重新启动。

## <a name="after-your-upgrade-is-done"></a>升级完成后

升级完成后，必须确保升级到 Windows Server 2012 R2 成功。

### <a name="to-make-sure-your-upgrade-was-successful"></a>确保升级成功

1. 打开注册表编辑器，中转到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，查看**产品名称**。 应会看到 windows Server 2012 R2 版本，例如**Windows server 2012 R2 Datacenter**。

2. 请确保所有应用程序都正在运行，并且客户端与应用程序的连接已成功。

如果你认为在升级过程中出现问题，请复制并压缩（通常`%SystemRoot%\Panther` `C:\Windows\Panther`为）目录，并与 Microsoft 支持部门联系。

## <a name="next-steps"></a>后续步骤

你可以从 Windows Server 2012 R2 升级到 Windows Server 2019。 有关详细说明，请参阅[将 Windows server 2012 R2 升级到 Windows server 2019](upgrade-2012r2-to-2019.md)。

## <a name="related-articles"></a>相关文章

- 有关 Windows Server 2012 R2 的详细信息和信息，请参阅[Windows server 2012 r2 和 Windows server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11))。

---
title: 将 Windows Server 2016 升级到 Windows Server 2019 |Microsoft Docs
description: 了解如何执行就地升级，从 Windows Server 2016 到 Windows Server 2019。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 99133f2c582b180f240740fc2f39e99527bc0cf8
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124817"
---
# <a name="upgrade-windows-server-2016-to-windows-server-2019"></a>将 Windows Server 2016 升级到 Windows Server 2019

如果要保留相同的硬件和已设置的所有服务器角色而不平展服务器，则需要执行就地升级。 就地升级允许从较旧的操作系统切换到较新的操作系统，同时使设置、服务器角色和数据保持不变。 本文可帮助你从 Windows Server 2016 迁移到 Windows Server 2019。

## <a name="before-you-begin-your-in-place-upgrade"></a>开始就地升级之前

在开始 Windows Server 升级之前，建议你从设备收集一些信息，以便进行诊断和故障排除。 因为此信息仅供升级失败时使用，所以必须确保将信息存储在可从设备访问的某个位置。

### <a name="to-collect-your-info"></a>收集信息

1. 打开命令提示符，中转到`c:\Windows\system32`，然后键入**systeminfo.exe**。

2. 将生成的系统信息复制、粘贴并存储在设备之外的某个位置。

3. 在命令提示符下键入**ipconfig/all** ，然后将生成的配置信息复制并粘贴到上述位置。

4. 打开注册表编辑器，中转到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然后将 Windows Server **BuildLabEx** （版本）和**EditionID** （edition）复制并粘贴到上述位置。

收集了所有与 Windows Server 相关的信息后，我们强烈建议你备份操作系统、应用程序和虚拟机。 你还必须**关闭**、**快速迁移**或**实时迁移**当前正在服务器上运行的所有虚拟机。 在就地升级过程中，不能运行任何虚拟机。

## <a name="to-perform-the-upgrade"></a>执行升级

1. 请确保**BuildLabEx**值表明你正在运行 Windows Server 2016。

2. 找到 Windows Server 2019 安装程序介质，**然后选择 "setup.exe"** 。

    ![显示 setup.exe 文件的 Windows 资源管理器](media/upgrade-2016-2019/setup-2019.png)

3. 选择 **"是"** 以启动安装过程。

    ![要求用户提供权限来启动安装程序的用户帐户控制](media/upgrade-2016-2019/start-setup-uac-box.png)

4. 对于连接到 internet 的设备，请选择 "**下载更新、驱动程序和可选功能（推荐）** " 选项，然后选择 "**下一步**"。

    ![用于选择联机以获取重要 Windows 更新的屏幕](media/upgrade-2016-2019/online-updates-win-setup.png)

5. 安装程序将检查你的设备配置，你必须等待它完成，然后选择 "**下一步**"。

6. 根据接收到的 Windows Server media 来源（零售、批量许可、OEM、ODM 等）和服务器许可证，系统可能会提示输入授权密钥以继续。

7. 选择要安装的 Windows Server 2019 edition，然后选择 "**下一步**"。

    ![用于选择要安装的 Windows Server 2016 edition 的屏幕](media/upgrade-2016-2019/select-os-edition.png)

8. 选择 "**接受**" 以接受许可协议的条款，基于分发渠道（例如零售、批量许可、OEM、ODM 等）。

    ![接受许可协议的屏幕](media/upgrade-2016-2019/license-terms.png)

9. 选择 "**保留个人文件和应用**" 以选择进行就地升级，然后选择 "**下一步**"。

    ![用于选择安装类型的屏幕](media/upgrade-2016-2019/choose-install-upgrade.png)

10. 安装程序分析设备后，请选择 "**安装**" 以继续进行升级。

    ![屏幕显示已准备好开始升级](media/upgrade-2016-2019/ready-to-install.png)

    就地升级将开始，其中显示了 "**升级 Windows** " 屏幕的进度。 升级完成后，服务器将重新启动。

    ![显示升级进度的屏幕](media/upgrade-2016-2019/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>升级完成后

升级完成后，必须确保已成功升级到 Windows Server 2019。

### <a name="to-make-sure-your-upgrade-was-successful"></a>确保升级成功

1. 打开注册表编辑器，中转到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，查看**产品名称**。 应会看到 windows Server 2019 的版本，例如**Windows server 2019 Datacenter**。

2. 请确保所有应用程序都正在运行，并且客户端与应用程序的连接已成功。

如果你认为在升级过程中出现问题，请复制并压缩（通常`%SystemRoot%\Panther` `C:\Windows\Panther`为）目录，并与 Microsoft 支持部门联系。

## <a name="related-articles"></a>相关文章

- 有关 Windows Server 2019 的详细信息和信息，请参阅[Windows server 2019 入门](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)。
---
title: 为用户设置远程桌面 Web 客户端
description: 介绍管理员如何设置远程桌面 Web 客户端。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 09/19/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 38f54548e8e68a0ee693c5d8ec80e67057b3d5b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387657"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>为用户设置远程桌面 Web 客户端

远程桌面 Web 客户端使用户可以通过兼容的 Web 浏览器访问组织的远程桌面基础结构。 无论他们身在何处，都能够如同使用本地电脑一样与远程应用或桌面进行交互。 设置了远程桌面 Web 客户端后，用户只需要可以用于访问客户端的 URL、其凭据以及受支持的 Web 浏览器，便可开始使用。

>[!IMPORTANT]
>Web 客户端当前不支持使用 Azure 应用程序代理，并且根本不支持 Web 应用程序代理。 有关详细信息，请参阅[将 RDS 与应用程序代理服务结合使用](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services)。

## <a name="what-youll-need-to-set-up-the-web-client"></a>设置 Web 客户端所需的条件

开始之前，请记住以下事项：

* 确保[远程桌面部署](../rds-deploy-infrastructure.md)具有在 Windows Server 2016 或 2019年上运行的 RD 网关、RD 连接代理和 RD Web 访问。
* 确保部署针对[每用户客户端访问许可证](../rds-client-access-license.md) (CAL)（而不是每设备）进行配置，否则会使用所有许可证。
* 在 RD 网关上安装 [Windows 10 KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)。 更高版本的累积更新可能已包含此 KB。
* 确保为 RD 网关和 RD Web 访问角色配置了公共受信任证书。
* 确保用户将连接到的任何计算机都在运行以下操作系统版本之一：
  * Windows 10
  * Windows Server 2008R2 或更高版本

用户在连接到 Windows Server 2016（或更高版本）和 Windows 10（版本 1611 或更高版本）时会获得更好的性能。

>[!IMPORTANT]
>如果在预览期间使用了 Web 客户端并安装了低于 1.0.0 的版本，则必须先卸载旧客户端，然后再迁移到新版本。 如果收到指示“Web 客户端在安装时使用了较旧版本的 RDWebClientManagement，必须先删除再部署新版本”的错误，请按照以下步骤操作：
>
>1. 打开提升的 PowerShell 提示符。
>2. 运行 Uninstall-Module RDWebClientManagement  以卸载新模块。
>3. 关闭并重新打开提升的 PowerShell 提示符。
>4. 运行 Install-Module RDWebClientManagement -RequiredVersion \<旧版本> 以安装旧模块。 
>5. 运行 Uninstall-RDWebClient  以卸载旧 Web 客户端。
>6. 运行 Uninstall-Module RDWebClientManagement  以卸载旧模块。
>7. 关闭并重新打开提升的 PowerShell 提示符。
>8. 按如下所示继续执行正常安装步骤。

## <a name="how-to-publish-the-remote-desktop-web-client"></a>如何发布远程桌面 Web 客户端

若要首次安装 Web 客户端，请按照以下步骤操作：

1. 在 RD 连接代理服务器上，获取用于远程桌面连接的证书并将它导出为 .cer 文件。 将 .cer 文件从 RD 连接代理复制到运行 RD Web 角色的服务器。
2. 在 RD Web 访问服务器上，打开提升的 PowerShell 提示符。
3. 在 Windows Server 2016 上，更新 PowerShellGet 模块，因为收件箱版本不支持安装 Web 客户端管理模块。 若要更新 PowerShellGet，请运行以下 cmdlet：
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >需要先重启 PowerShell，然后更新才能生效，否则模块可能无法正常工作。

4. 使用此 cmdlet 从 PowerShell 库安装远程桌面 Web 客户端管理 PowerShell 模块：
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 在此之后，运行以下 cmdlet，以下载远程桌面 Web 客户端的最新版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接下来，运行此 cmdlet，将括号括起来的值替换为从 RD 代理复制的 .cer 文件的路径：
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最后，运行此 cmdlet 以发布远程桌面 Web 客户端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    确保可以使用服务器名称，通过 Web 客户端 URL 访问 Web 客户端，格式为 <https://server_FQDN/RDWeb/webclient/index.html>。 务必在 URL 中使用与 RD Web 访问公共证书匹配的服务器名称（通常是服务器 FQDN）。

    >[!NOTE]
    >运行 Publish-RDWebClientPackage  cmdlet 时，可能会看到一个警告，指出不支持每设备 CAL，即使是针对每用户 CAL 配置部署也是如此。 如果部署使用每用户 CAL，则可以忽略此警告。 我们显示它是为了确保你了解配置限制。
8. 准备好让用户访问 Web 客户端时，只需向他们发送创建的 Web 客户端 URL。

>[!NOTE]
>若要查看 RDWebClientManagement 模块支持的所有 cmdlet 的列表，请在 PowerShell 中运行以下 cmdlet：
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>如何更新远程桌面 Web 客户端

有新版本的远程桌面 Web 客户端可用时，请按照以下这些步骤使用新客户端更新部署：

1. 在 RD Web 访问服务器上打开提升的 PowerShell 提示符并运行以下 cmdlet，以下载最新的可用 Web 客户端版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. （可选）可以通过运行此 cmdlet，在正式版本之前发布客户端进行测试：
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    客户端应出现在与 Web 客户端 URL 对应的测试 URL 上（例如，<https://server_FQDN/RDWeb/webclient-test/index.html>）。
3. 通过运行以下 cmdlet 来为用户发布客户端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    这会在所有用户重新启动网页时替换其客户端。

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>如何卸载远程桌面 Web 客户端

若要删除 Web 客户端的所有踪迹，请按照以下步骤操作：

1. 在 RD Web 访问服务器上，打开提升的 PowerShell 提示符。
2. 取消发布测试和生产客户端，卸载所有本地程序包并删除 Web 客户端设置：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 卸载远程桌面 Web 客户端管理 PowerShell 模块：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>如何在没有 Internet 连接的情况下安装远程桌面 Web 客户端

请按照以下步骤将 Web 客户端部署到没有 Internet 连接的 RD Web 访问服务器。

> [!NOTE]
> 可以在版本 1.0.1 及更高版本的 RDWebClientManagement PowerShell 模块中进行安装而不需要 Internet 连接。

> [!NOTE]
> 仍然需要具有 Internet 访问权限的管理员电脑以下载所需文件，然后将它们传输到脱机服务器。

> [!NOTE]
> 最终用户电脑目前需要 Internet 连接。 在未来版本的客户端中会解决此问题以提供完全脱机方案。

### <a name="from-a-device-with-internet-access"></a>通过具有 Internet 访问权限的设备

1. 打开 PowerShell 提示符。

2. 从 PowerShell 库导入远程桌面 Web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 下载最新版本的远程桌面 Web 客户端以便在另一个设备上安装：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下载最新版本的 RDWebClientManagement PowerShell 模块：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. 将 "C:\WebClient\" 的内容复制到 RD Web 访问服务器。

### <a name="from-the-rd-web-access-server"></a>通过 RD Web 访问服务器

按照[如何发布远程桌面 Web 客户端](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)下的说明操作（将步骤 4 和 5 替换为以下内容）。

4. 从本地文件夹导入远程桌面 Web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 从本地文件夹部署最新版本的远程桌面 Web 客户端（替换为相应的 zip 文件）：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>在 Windows Server 2019 中连接到 RD 代理而不使用 RD 网关
此部分介绍如何在 Windows Server 2019 中实现与 RD 代理的 Web 客户端连接而不使用 RD 网关。

### <a name="setting-up-the-rd-broker-server"></a>设置 RD 代理服务器

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>如果没有证书绑定到 RD 代理服务器，请按照以下这些步骤操作

1. 打开“服务器管理器”   > “远程桌面服务”  。

2. 在“部署概述”  部分中，选择“任务”  下拉菜单。

3. 选择“编辑部署属性”  ，一个标题为“部署属性”  的新窗口会打开。

4. 在“部署属性”  窗口中，选择左侧菜单中的“证书”  。

5. 在证书级别列表中，选择“RD 连接代理 – 启用单一登录”  。 有两个选项：(1) 创建新证书或 (2) 使用现有证书。

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>如果以前有证书绑定到 RD 代理服务器，请按照以下这些步骤操作

1. 打开绑定到代理的证书，然后复制“指纹”  值。

2. 若要将此证书绑定到安全端口 3392，请打开提升的 PowerShell 窗口并运行以下命令（将“< thumbprint >”  替换为上一步中复制的值）：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要检查证书是否已正确绑定，请运行以下命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在 SSL 证书绑定列表中，确保正确的证书已绑定到端口 3392。

3. 打开 Windows 注册表 (regedit)，导航到 ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` 并找到键 WebSocketURI  。 值必须设置为 <strong>https://+:3392/rdp/</strong>。

### <a name="setting-up-the-rd-session-host"></a>设置 RD 会话主机
如果 RD 会话主机服务器与 RD 代理服务器不同，请按照以下这些步骤操作：

1. 为 RD 会话主机计算机创建证书，打开它并复制“指纹”  值。

2. 若要将此证书绑定到安全端口 3392，请打开提升的 PowerShell 窗口并运行以下命令（将“< thumbprint >”  替换为上一步中复制的值）：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要检查证书是否已正确绑定，请运行以下命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在 SSL 证书绑定列表中，确保正确的证书已绑定到端口 3392。

3. 打开 Windows 注册表 (regedit)，导航到 ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` 并找到键 WebSocketURI  。 值必须设置为 <https://+:3392/rdp/>。

### <a name="general-observations"></a>常规观察

* 确保 RD 会话主机和 RD 代理服务器在运行 Windows Server 2019。

* 确保为 RD 会话主机和 RD 代理服务器配置了公共受信任证书。
    > [!NOTE]
    > 如果 RD 会话主机和 RD 代理服务器共享相同计算机，则仅设置 RD 代理服务器证书。 如果 RD 会话主机和 RD 代理服务器使用不同计算机，则必须使用唯一证书配置两者。

* 每个证书的使用者可选名称 (SAN)  都必须设置为计算机完全限定的域名 (FQDN)  。 每个证书的公用名 (CN)  必须与 SAN 匹配。

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>如何为远程桌面 Web 客户端用户预配置设置
此部分告知如何使用 PowerShell 为远程桌面 Web 客户端部署配置设置。 这些 PowerShell cmdlet 基于组织的安全问题或者工作流控制用户是否能够更改设置。 以下设置全都位于 Web 客户端的“设置”  侧面板中。

### <a name="suppress-telemetry"></a>禁止遥测
默认情况下，用户可以选择启用或禁用收集向 Microsoft 发送的遥测数据。 有关 Microsoft 收集的遥测数据的信息，请通过“关于”  侧面板中的链接参阅我们的隐私声明。

作为管理员，可以使用以下 PowerShell cmdlet 选择对部署禁止遥测收集：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

默认情况下，用户可以选择启用或禁用遥测。 布尔值 $false  会与默认客户端行为匹配。 布尔值 $true  会禁用遥测，并限制用户启用遥测。

### <a name="remote-resource-launch-method"></a>远程资源启动方法

>[!NOTE]
>此设置目前仅适用于 RDS Web 客户端，不适用于 Windows 虚拟桌面 Web 客户端。

默认情况下，用户可以选择通过以下方法启动远程资源：(1) 在浏览器中或 (2) 下载 .rdp 文件以处理安装在其计算机上的另一个客户端。 作为管理员，可以使用以下 Powershell 命令选择对部署限制远程资源启动方法：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 默认情况下，用户可以选择任一启动方法。 布尔值 $true  会强制用户在浏览器中启动资源。 布尔值 $false  会强制用户通过下载 .rdp 文件以处理本地安装的 RDP 客户端来启动资源。

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>将 RDWebClientDeploymentSetting 配置重置为默认值

若要将部署级 Web 客户端设置重置为默认配置，请运行以下 PowerShell cmdlet，并使用 -name 参数指定要重置的设置：

   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>疑难解答

如果用户在首次打开 web 客户端时报告以下任何问题，则以下各部分会告知执行哪些操作来修复它们。

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>如果在尝试访问 Web 客户端时用户的浏览器显示安全警告，该怎么办

RD Web 访问角色可能未使用受信任的证书。 确保使用公开受信任的证书配置 RD Web 访问角色。

如果这不起作用，则 Web 客户端 URL 中的服务器名称可能与 RD Web 证书提供的名称不匹配。 确保 URL 使用承载 RD Web 角色的服务器的 FQDN。

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>如果用户无法使用 Web 客户端连接到资源，即使他们可以查看“所有资源”下的项目，该怎么办

如果用户报告他们无法使用 Web 客户端进行连接，即使他们可以看到列出的资源，请检查以下事项：

* RD 网关角色是否正确配置为使用受信任的公共证书？
* RD 网关服务器是否安装了所需更新？ 确保服务器安装了 [KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)。

如果用户在尝试连接时收到“收到了一个意外的服务器身份验证证书”错误消息，则该消息会显示证书的指纹。 使用该指纹搜索 RD 代理服务器的证书管理器，以找到正确的证书。 在远程桌面部署属性页面中验证该证书是否配置为用于 RD 代理角色。 确保该证书尚未过期之后，将 .cer 文件格式的证书复制到 RD Web 访问服务器并在 RD Web 访问服务器上运行以下命令（将括号括起来的值替换为该证书的文件路径）：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>使用控制台日志诊断问题

如果无法基于本文中的故障排除说明解决问题，则可以尝试通过在浏览器中观察控制台日志来自行诊断问题来源。 Web 客户端提供了一种方法，可在使用 Web 客户端期间记录浏览器控制台日志活动以帮助诊断问题。

* 选择右上角的省略号，然后在下拉菜单中导航到“关于”  页面。
* 在“捕获支持信息”  下，选择“开始记录”  按钮。
* 在 Web 客户端中执行生成尝试诊断的问题的操作。
* 导航到“关于”  页面，然后选择“停止记录”  。
* 浏览器会自动下载一个标题为 RD Console Logs.txt  的 .txt 文件。 此文件包含重现目标问题时生成的完整控制台日志活动。

还可以直接通过浏览器访问控制台。 控制台通常位于开发人员工具下。 例如，可以通过按 F12  键，或是通过选择省略号，然后导航到“更多工具”   > “开发人员工具”  ，在 Microsoft Edge 中访问日志。

## <a name="get-help-with-the-web-client"></a>获取有关 Web 客户端的帮助

如果遇到无法通过使用本文中的信息来解决的问题，可以在[技术社区](https://aka.ms/wvdtc)中报告。 还可以通过我们的[建议箱](https://remotedesktop.uservoice.com/forums/911494-remote-desktop-web-client)对新功能进行请求或投票。

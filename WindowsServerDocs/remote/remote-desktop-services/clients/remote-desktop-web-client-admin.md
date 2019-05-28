---
title: 为用户设置远程桌面 Web 客户端
description: 介绍管理员如何设置远程桌面 web 客户端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: bf10f7f7444967247e51065bc6138fc0afd5ed1a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976783"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>为用户设置远程桌面 Web 客户端

远程桌面 web 客户端允许用户通过兼容的 web 浏览器访问组织的远程桌面基础结构。 他们将能够与远程应用程序或桌面类似的方式与无论身在何处本地 PC 进行交互。 一旦设置了远程桌面 web 客户端时，用户需要开始是 URL 它们可以访问客户端、 其凭据和受支持的 web 浏览器的位置。

>[!IMPORTANT]
>Web 客户端当前不支持使用 Azure 应用程序代理，并不完全支持 Web 应用程序代理。 请参阅[使用应用程序代理服务使用 RDS](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services)有关详细信息。

## <a name="what-youll-need-to-set-up-the-web-client"></a>你将需要设置 web 客户端

开始之前，请记住以下事项：

* 请确保你[远程桌面部署](../rds-deploy-infrastructure.md)具有 RD 网关、 RD 连接代理和 RD Web 访问在 Windows Server 2016 或 2019年上运行。
* 请确保你的部署已针对[每用户客户端访问许可证](../rds-client-access-license.md)(Cal) 而不是每个设备，否则所有许可证都都会被。
* 安装[Windows 10 KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)对 RD 网关。 更高版本的累积更新可能已包含此知识库文章。
* 请确保公共受信任的证书配置为 RD 网关和 RD Web 访问角色。
* 请确保你的用户将连接到任何计算机正在运行下列操作系统版本之一：
  * Windows 10
  * Windows Server 2008R2 或更高版本

用户将看到更好的性能连接到 Windows Server 2016 （或更高版本） 和 Windows 10 （版本 1611年或更高版本）。

>[!IMPORTANT]
>如果在预览期间使用的 web 客户端安装的版本低于 1.0.0，必须先将移动到新版本之前卸载旧的客户端。 如果收到错误，指出"web 客户端使用较旧版本的 RDWebClientManagement 安装和部署新版本之前必须先删除"，请按照下列步骤：
>
>1. 打开提升的 PowerShell 提示符。
>2. 运行**Uninstall-module RDWebClientManagement**卸载新模块。
>3. 关闭并重新打开提升的 PowerShell 提示符。
>4. 运行**Install-module RDWebClientManagement-RequiredVersion\<旧版本 > 若要安装旧模块。**
>5. 运行**卸载 RDWebClient**卸载旧的 web 客户端。
>6. 运行**Uninstall-module RDWebClientManagement**卸载旧的模块。
>7. 关闭并重新打开提升的 PowerShell 提示符。
>8. 继续执行正常安装步骤，如下所示。

## <a name="how-to-publish-the-remote-desktop-web-client"></a>如何发布远程桌面 web 客户端

若要安装第一次 web 客户端，请按照下列步骤：

1. 在 RD 连接代理服务器上，获取用于远程桌面连接的证书并将其导出为.cer 文件。 将.cer 文件从 RD 连接代理复制到运行 RD Web 角色的服务器。
2. 在 RD Web 访问服务器上打开提升的 PowerShell 提示符。
3. Windows Server 2016 上更新 PowerShellGet 模块，因为收件箱版本不支持安装 web 客户端管理模块。 若要更新 PowerShellGet，请运行以下 cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >你将需要更新才能生效，否则为该模块可能无法重新启动 PowerShell。

4. 从 PowerShell 库使用此 cmdlet 安装远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 然后，运行以下 cmdlet，以下载远程桌面 web 客户端的最新版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接下来，运行此 cmdlet，用括号括起来的值替换为从 RD 代理复制的.cer 文件的路径：
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最后，运行此 cmdlet 可将远程桌面 web 客户端发布：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    请确保你可以使用您的服务器名称，格式为访问 web 客户端的 web 客户端 url <https://server_FQDN/RDWeb/webclient/index.html>。 请务必使用与匹配的 URL （通常服务器 FQDN） 中的 RD Web 访问公共证书的服务器名称。

    >[!NOTE]
    >运行时**发布 RDWebClientPackage** cmdlet，你可能会看到一个警告，指出每设备 Cal 不受支持，即使你的部署配置的每个用户 Cal。 如果你的部署使用每用户 Cal，可以忽略此警告。 我们显示，以确保您知道的配置限制。
8. 用户便可以访问 web 客户端时，只需向他们发送您创建的 web 客户端 URL。

>[!NOTE]
>若要查看所有支持的 cmdlet 为 RDWebClientManagement 模块的列表，请在 PowerShell 中运行以下 cmdlet:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>如何更新远程桌面 web 客户端

可用的远程桌面 web 客户端的新版本时，请按照下列步骤来更新具有新的客户端部署：

1. 打开 RD Web 访问服务器上提升的 PowerShell 提示符并运行以下 cmdlet，以下载最新的可用 web 客户端版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. （可选） 可以将客户端发布的官方发布之前测试通过运行此 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    客户端应出现在对应于你的 web 客户端 URL 测试 URL (例如， <https://server_FQDN/RDWeb/webclient-test/index.html>)。
3. 通过运行以下 cmdlet 来发布用户的客户端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    当他们重新启动 web 页时，这将替换所有用户的客户端。

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>如何卸载远程桌面 web 客户端

若要删除的 web 客户端的所有跟踪，请按照下列步骤：

1. 在 RD Web 访问服务器上打开提升的 PowerShell 提示符。
2. 取消发布的测试和生产客户端、 卸载所有本地包和删除 web 客户端设置：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 卸载远程桌面 web 客户端管理 PowerShell 模块：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>如何安装远程桌面 web 客户端未连接到 internet

请按照下列步骤以将 web 客户端部署到没有 internet 连接的 RD Web 访问服务器。

> [!NOTE]
> 安装没有 internet 连接是版本 1.0.1 中及更高版本的 RDWebClientManagement PowerShell 模块。

> [!NOTE]
> 您仍然需要管理员 PC 具有 internet 访问权限之前将它们传输到该脱机服务器下载所需的文件。

> [!NOTE]
> 现在，最终用户 PC 需要 internet 连接。 客户端提供完整的脱机方案的未来版本将解决此问题。

### <a name="from-a-device-with-internet-access"></a>从具有 internet 访问权限的设备

1. 打开 PowerShell 提示符。

2. 从 PowerShell 库导入的远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 下载用于在不同的设备上安装远程桌面 web 客户端的最新版本：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下载最新版本的 RDWebClientManagement PowerShell 模块：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. 复制的内容"C:\WebClient\"到 RD Web 访问服务器。

### <a name="from-the-rd-web-access-server"></a>从 RD Web 访问服务器

请按照下面的说明[如何将远程桌面 web 客户端发布](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)，使用以下替换步骤 4 和 5。

4. 从本地文件夹中导入的远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 部署远程桌面 web 客户端从本地文件夹 （替换为相应的 zip 文件） 的最新版本：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>连接到 RD 代理，而无需在 Windows Server 2019 RD 网关
本部分介绍如何启用 web 客户端连接到 RD Broker 而无需在 Windows Server 2019 RD 网关。

### <a name="setting-up-the-rd-broker-server"></a>将 RD 代理服务器设置

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>如果没有证书绑定到 RD 代理服务器，请执行以下步骤

1. 打开**服务器管理器** > **远程桌面服务**。

2. 在中**部署概述**部分中，选择**任务**下拉列表菜单。

3. 选择**编辑部署属性**，新的窗口标题为**部署属性**将打开。

4. 在中**部署属性**窗口中，选择**证书**在左侧菜单中。

5. 在证书级别的列表中选择**RD 连接代理-启用单一登录**。 有两个选项：（1） 创建新的证书或 (2) 的现有证书。

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>如果以前绑定到 RD 代理服务器的证书，请执行以下步骤

1. 打开该证书绑定到的 Broker 和复制**指纹**值。

2. 若要将此证书绑定到安全端口 3392，打开提升的 PowerShell 窗口并运行以下命令，用 **"< 指纹 >"** 使用上一步中复制的值：

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
    > 在列表中的 SSL 证书绑定，请确保正确的证书绑定到端口 3392。

3. 打开 Windows 注册表 (regedit) 和到 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```并找到该密钥**WebSocketURI**。 值必须设置为**https://+:3392/rdp/**。

### <a name="setting-up-the-rd-session-host"></a>设置 RD 会话主机
如果在 RD 会话主机服务器不同于 RD 代理服务器，请执行以下步骤：

1. 创建用于 RD 会话主机计算机的证书、 将其打开和复制**指纹**值。

2. 若要将此证书绑定到安全端口 3392，打开提升的 PowerShell 窗口并运行以下命令，用 **"< 指纹 >"** 使用上一步中复制的值：

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
    > 在列表中的 SSL 证书绑定，请确保正确的证书绑定到端口 3392。

3. 打开 Windows 注册表 (regedit) 和到 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```并找到该密钥**WebSocketURI**。 值必须设置为**https://+:3392/rdp/**。

### <a name="general-observations"></a>问题的一般意见

* 确保 RD 会话主机和 RD 代理服务器正在运行 Windows Server 2019。

* 请确保该公共受信任的证书配置 RD 会话主机和 RD 代理服务器。
    > [!NOTE]
    > 如果 RD 会话主机和 RD 代理服务器都共享同一台计算机，设置 RD 代理服务器证书。 如果 RD 会话主机和 RD 代理服务器使用不同的计算机，都必须使用唯一的证书配置。

* **使用者可选名称 (SAN)** 每个证书必须设置为计算机的**完全限定域名 (FQDN)** 。 **公用名 (CN)** 必须匹配的每个证书 SAN。

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>如何预配置远程桌面 web 客户端用户设置
本部分将告诉您如何使用 PowerShell 配置远程桌面 web 客户端部署的设置。 这些 PowerShell cmdlet 控制用户能够更改设置，根据组织的安全问题或者工作流。 以下设置均位于**设置**的 web 客户端侧面板。 

### <a name="suppress-telemetry"></a>禁止显示遥测数据
默认情况下，用户可以选择启用或禁用向 Microsoft 发送的遥测数据的集合。 有关 Microsoft 收集的遥测数据的信息，请参阅中的链接，通过我们的隐私声明**有关**侧面板。

作为管理员，你可以选择要取消你的部署使用以下 PowerShell cmdlet 的遥测数据收集：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -SuppressTelemetry $true
   ```

默认情况下，用户可以选择启用或禁用遥测数据。 一个布尔值 **$false**将匹配的默认客户端行为。 一个布尔值 **$true**禁用遥测数据，并将用户限制从启用遥测。

### <a name="remote-resource-launch-method"></a>远程资源启动方法
默认情况下，用户可以选择以启动远程资源 （1） 在浏览器中或 （2） 通过下载.rdp 文件，以处理与他们的计算机上安装的另一个客户端。 作为管理员，你可以选择限制你使用以下 Powershell 命令的部署的远程资源启动方法：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -LaunchResourceInBrowser ($true|$false)
   ```
 默认情况下，用户可以选择任何一种启动方法。 一个布尔值 **$true**将强制用户启动浏览器中的资源。 一个布尔值 **$false**将强制用户通过下载.rdp 文件来处理使用本地安装 RDP 客户端启动的资源。

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>重置 RDWebClientDeploymentSetting 配置为默认值
若要重置为默认配置的所有部署级别 web 客户端设置，请运行以下 PowerShell cmdlet:

   ```PowerShell
    Reset-RDWebClientDeploymentSetting 
   ```

## <a name="troubleshooting"></a>疑难解答

如果用户报告的任何以下问题第一次打开 web 客户端时，以下各节将告诉您要执行的操作来修复它们。

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>如果用户的浏览器中显示安全警告，在尝试访问 web 客户端时，怎么办

RD Web 访问角色可能没有使用受信任的证书。 请确保使用公开受信任的证书配置 RD Web 访问角色。

如果不起作用，你的服务器名称在 web 客户端 URL 可能不匹配 RD Web 证书所提供的名称。 请确保你的 URL 使用承载 RD Web 角色的服务器的 FQDN。

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>要执行的操作如果用户不能连接到资源与 web 客户端即使他们可以查看在所有资源下的项

如果用户报告，它们不能使用 web 客户端连接即使他们可以看到列出的资源，请检查以下操作：

* RD 网关角色正确配置为使用受信任的公共证书？
* RD 网关服务器是否已安装所需的更新？ 请确保你的服务器具有[KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)安装。

如果用户会收到"已收到意外的服务器身份验证证书"的错误消息时在尝试连接，则该消息将显示证书的指纹。 搜索 RD 代理服务器的证书管理器使用该指纹来查找正确的证书。 验证证书配置用于远程桌面部署属性页中的 RD 代理角色。 确保该证书尚未过期后，将.cer 文件格式的证书复制到 RD Web 访问服务器和 RD Web 访问服务器上运行以下命令，用括号括起来的值替换为证书的文件路径：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>诊断控制台日志的问题

如果你不能解决根据这篇文章中的故障排除说明此问题，可以尝试自行诊断问题的根源通过观察控制台浏览器中的日志。 Web 客户端提供用于在使用 web 客户端帮助诊断问题时记录浏览器控制台日志活动的方法。

* 在右上角选择省略号，然后导航到**有关**下拉列表菜单中的页。
* 下**捕获支持信息**选择**开始录制**按钮。
* 在生成要诊断此问题的 web 客户端中执行操作。
* 导航到**有关**页，然后选择**停止录制**。
* 在浏览器会自动将下载一个.txt 文件，名为**RD 控制台 Logs.txt**。 此文件将包含重现该目标问题时生成完整的控制台日志活动。

此外可以直接通过浏览器访问控制台。 在控制台通常位于下的开发人员工具。 例如，可以通过按访问 Microsoft Edge 中的日志**F12**键，或通过选择省略号，然后导航到**更多工具** > **开发人员工具**.

## <a name="get-help-with-the-web-client"></a>获取有关 web 客户端的帮助

如果您遇到不能通过这篇文章中的信息来解决了问题，可以[发送电子邮件](mailto:rdwbclnt@microsoft.com)进行报告。 此外可以请求或新功能在我们[常开](https://aka.ms/rdwebfbk)。

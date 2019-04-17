---
title: 为用户设置远程桌面 Web 客户端
description: 描述了如何管理员可以设置远程桌面 web 客户端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099180"
---
# 为用户设置远程桌面 Web 客户端

远程桌面 web 客户端允许用户通过兼容的 web 浏览器访问你的组织的远程桌面基础结构。 他们将能够与远程应用或类似的方式与本地电脑无需考虑所在位置的桌面进行交互。 一旦你设置远程桌面 web 客户端，所有用户需要开始是 URL 它们可以访问客户端、 其凭据和受支持的 web 浏览器的位置。

>[!IMPORTANT]
>Web 客户端当前不支持使用 Azure 应用程序代理和根本不支持 Web 应用程序代理。 有关详细信息，请参阅[使用 RDS 与应用程序代理服务](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services)。

## 你将需要设置 web 客户端

在开始之前，请记住以下事项：

* 请确保[远程桌面部署](../rds-deploy-infrastructure.md)具有 RD 网关、 RD 连接代理和 Windows Server 2016 或 2019年上运行的 RD Web 访问。
* 请确保你的部署配置为[每个用户客户端访问许可证](../rds-client-access-license.md)(Cal) 而不是每个设备，否则为将使用的所有许可证。
* 在 RD 网关上安装[Windows 10 KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)。 更高版本的累积更新可能已包含此 KB。
* 请确保为 RD 网关和 RD Web 访问角色配置公共受信任的证书。
* 请确保你的用户将连接到任何计算机正在运行下列操作系统版本之一：
  * Windows 10
  * Windows Server 2008R2 或更高版本

你的用户将看到更好的性能连接到 Windows Server 2016 （或更高版本） 和 Windows 10 （版本 1611年或更高版本）。

>[!IMPORTANT]
>如果你在预览阶段使用 web 客户端，并安装的版本 1.0.0 之前，你必须先移动到新版本之前卸载旧的客户端。 如果你收到错误，指出"web 客户端使用较早版本的 RDWebClientManagement 安装和部署新版本之前必须先删除"，请按照下列步骤：
>
>1. 打开提升的 PowerShell 提示符。
>2. 运行**卸载模块 RDWebClientManagement**卸载新模块。
>3. 关闭并重新打开提升的 PowerShell 提示符。
>4. 运行**安装模块 RDWebClientManagement-RequiredVersion \<old version> 安装旧的模块。**
>5. 运行**卸载 RDWebClient**卸载旧 web 客户端。
>6. 运行**卸载模块 RDWebClientManagement**卸载旧的模块。
>7. 关闭并重新打开提升的 PowerShell 提示符。
>8. 继续执行正常安装步骤，如下所示。

## 如何发布远程桌面 web 客户端

若要安装 web 客户端第一次，请按照下列步骤：

1. 在 RD 连接代理服务器上，获取用于远程桌面连接的证书并将其导出为.cer 文件。 从 RD 连接代理的.cer 文件复制到运行 RD Web 角色的服务器。
2. 在 RD Web 访问服务器上，打开提升的 PowerShell 提示符。
3. Windows Server 2016 上更新 PowerShellGet 模块，因为收件箱版本不支持安装 web 客户端管理模块。 若要更新 PowerShellGet，运行以下 cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >你将需要重新启动 PowerShell 才能更新可以生效，否则为该模块可能不起作用。

4. 从使用此 cmdlet 在 PowerShell 库安装远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 在此之后，请运行以下 cmdlet 以下载最新版本的远程桌面 web 客户端：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接下来，使用括号中的值替换为从 RD 代理复制的.cer 文件的路径中运行此 cmdlet:
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最后，运行此 cmdlet 来进行发布远程桌面 web 客户端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    请确保你可以访问 web 客户端在 web 客户端 URL 与你的服务器名称，格式为<https://server_FQDN/RDWeb/webclient/index.html>。 请务必使用相匹配的 URL （通常服务器 FQDN） 中的 RD Web 访问公用证书的服务器名称。

    >[!NOTE]
    >当运行**发布 RDWebClientPackage** cmdlet，你可能会看到一条警告，显示每个设备 Cal 不受支持，即使你的部署配置为每个用户 Cal。 如果你的部署使用每个用户 Cal，则可以忽略此警告。 我们会显示它以确保你可以配置限制意识到。
8. 当你准备为用户访问 web 客户端时，只需向他们发送你创建的 web 客户端 URL。

>[!NOTE]
>若要查看所有受支持的 cmdlet RDWebClientManagement 模块的列表，请在 PowerShell 中运行以下 cmdlet:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## 如何更新远程桌面 web 客户端

远程桌面 web 客户端的新版本可用时，请按照以下步骤更新部署与新客户端：

1. 打开 RD Web 访问服务器上提升的 PowerShell 提示符并运行以下 cmdlet 以下载最新版本 web 客户端：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. （可选），你可以将客户端发布正式发布之前测试通过运行此 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    客户端应出现在与你的 web 客户端 URL 对应的测试 URL (例如， <https://server_FQDN/RDWeb/webclient-test/index.html>)。
3. 将用户的客户端发布通过运行以下 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    当他们重新启动网页时，这将替换为所有用户的客户端。

## 如何卸载远程桌面 web 客户端

若要删除的 web 客户端的所有跟踪，请按照下列步骤：

1. 在 RD Web 访问服务器上，打开提升的 PowerShell 提示符。
2. 取消发布的测试和生产客户端、 卸载所有本地包和删除 web 客户端设置：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 卸载远程桌面 web 客户端管理 PowerShell 模块：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## 如何安装远程桌面 web 客户端未连接到 internet

请按照以下步骤将 web 客户端部署到没有 internet 连接 RD Web 访问服务器。

> [!NOTE]
> 安装没有 internet 连接不可在版本 1.0.1 和更高版本的 RDWebClientManagement PowerShell 模块。

> [!NOTE]
> 你仍然需要具有 internet 访问权限之前将其传输到脱机的服务器下载必要的文件的管理员电脑。

> [!NOTE]
> 最终用户电脑现在需要 internet 连接。 这将在客户端提供完整的脱机情形的未来版本中得到解决。

### 从 internet 访问权限的设备

1. 打开 PowerShell 提示符。

2. 从 PowerShell 库导入远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 下载远程桌面 web 客户端在另一台设备上安装最新版本：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下载最新版本的 RDWebClientManagement PowerShell 模块：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. "C:\WebClient\"的内容复制到 RD Web 访问服务器。

### 从 RD Web 访问服务器

请按照下[如何发布远程桌面 web 客户端](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)，步骤 4 和 5 替换以下说明。

4. 从本地文件夹导入远程桌面 web 客户端管理 PowerShell 模块：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 部署远程桌面 web 客户端从本地文件夹 （替换为相应的 zip 文件） 的最新版本：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## 连接到 RD 代理，而无需在 Windows Server 2019 RD 网关
本部分介绍如何启用 web 客户端连接到 Windows Server 2019 中 RD 网关不 RD 代理。

### 设置 RD 代理服务器

#### 如果没有绑定到 RD 代理服务器的证书，请按照下列步骤

1. 打开**服务器管理器** > **远程桌面服务**。

2. 在**部署概述**部分中，选择**任务**下拉菜单。

3. 选择**编辑部署属性**，标题为**部署属性**在新窗口将会打开。

4. 在**部署属性**窗口中，在左侧菜单中选择**证书**。

5. 在证书级别的列表中，选择**RD 连接代理-启用单一登录**。 有两个选项: （1） 创建一个新的证书或 (2) 现有证书。

#### 如果以前绑定到 RD 代理服务器的证书，请按照下列步骤

1. 打开证书绑定到的代理和复制**指纹**值。

2. 若要将此证书绑定到安全端口 3392，打开提升的 PowerShell 窗口并运行以下命令， **"< 指纹 >"** 替换为从上一步中复制的值：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要查看证书是否已正确绑定，运行以下命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 中的 SSL 证书绑定的列表，请确保正确的证书绑定到端口 3392。

3. 打开 Windows 注册表 (regedit) 和 nagivate 到```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```并找到密钥**WebSocketURI**。 值必须设置为**https://+:3392/rdp/**。

### 设置 RD 会话主机
如果 RD 会话主机服务器不同于 RD 代理服务器，请按照下列步骤：

1. 为 RD 会话主机计算机创建证书、 打开它并复制**指纹**值。

2. 若要将此证书绑定到安全端口 3392，打开提升的 PowerShell 窗口并运行以下命令， **"< 指纹 >"** 替换为从上一步中复制的值：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要查看证书是否已正确绑定，运行以下命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 中的 SSL 证书绑定的列表，请确保正确的证书绑定到端口 3392。

3. 打开 Windows 注册表 (regedit) 和 nagivate 到```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```并找到密钥**WebSocketURI**。 值必须设置为**https://+:3392/rdp/**。

### 常规观察

* 确保 RD 会话主机和 RD 代理服务器正在运行 Windows Server 2019。

* 确保该公钥受信任的证书配置为 RD 会话主机和 RD 代理服务器。
    > [!NOTE]
    > 如果 RD 会话主机和 RD 代理服务器共享同一台计算机，设置 RD 代理服务器证书。 如果 RD 会话主机和 RD 代理服务器使用不同的计算机，都必须使用唯一证书配置。

* 每个证书**使用者备用名称 (SAN)** 必须设置为计算机的**完全限定域名 (FQDN)**。 **公用名 (CN)** 必须匹配的每个证书 SAN。

## 疑难解答

如果用户报告任何以下问题首次打开 web 客户端时，以下部分将告诉你要执行的操作来解决这些问题。

### 如果用户的浏览器中显示安全警告，在尝试访问 web 客户端时，怎么办

RD Web 访问角色可能不会使用受信任的证书。 请确保 RD Web 访问角色配置使用公共可信证书。

如果它不起作用，在 web 服务器名称客户端 URL 可能不匹配提供 RD Web 证书的名称。 请确保你的 URL 使用承载 RD Web 角色的服务器的 FQDN。

### 如果用户无法连接到资源与 web 客户端即使他们能够查看下的所有资源的项目，怎么办

如果用户报告，它们不能与联系 web 客户端即使他们可以看到列出的资源，请检查以下内容：

* RD 网关角色正确配置为使用受信任的公共证书？
* RD 网关服务器是否有安装所需的更新？ 请确保你的服务器具有[KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)安装。

如果用户获取"意外的服务器身份验证证书已收到"错误消息时在尝试连接，则该消息将显示证书的指纹。 搜索 RD 代理服务器的证书管理器使用的指纹来查找正确的证书。 验证证书配置为可用于远程桌面部署属性页中 RD 代理角色。 确保证书没有过期后，将.cer 文件格式的证书复制到 RD Web 访问服务器，并带有括在括号中的值替换为证书的文件路径 RD Web 访问服务器上运行以下命令：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### 诊断问题的控制台日志

如果你无法解决此问题，具体取决于本文章中的疑难解答说明进行操作，你可以尝试通过观看控制台自行诊断问题的来源记录在浏览器中。 Web 客户端提供了一种方法来使用 web 客户端来帮助诊断问题时记录的浏览器控制台日志活动。

* 选择右上角的省略号，然后导航到在下拉菜单中的**关于**页面。
* 在**捕获支持信息**下，选择**录制开始菜单**按钮。
* 在生成要诊断此问题的 web 客户端执行的操作。
* 导航到**关于**页并选择**停止录制**。
* 你的浏览器将自动下载标题为**RD 控制台 Logs.txt**.txt 文件。 此文件将包含重现该目标问题时生成的完整控制台日志活动。

也可以直接通过你的浏览器访问控制台。 在控制台通常位于下的开发人员工具。 例如，你可以访问 Microsoft Edge 中的日志，通过按**F12**键，或选择省略号，然后导航到**更多的工具** > **开发人员工具**。

## 获取有关帮助 web 客户端

如果已遇到，通过此文章中的信息不能解决的问题，你可以[向我们发送电子邮件](mailto:rdwbclnt@microsoft.com)报告它。 你还可以请求或对我们[建议框中](https://aka.ms/rdwebfbk)的新增功能进行投票。
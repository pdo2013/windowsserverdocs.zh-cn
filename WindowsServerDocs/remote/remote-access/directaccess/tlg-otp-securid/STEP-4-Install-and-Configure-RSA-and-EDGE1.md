---
title: 步骤 4： 安装和配置 RSA 和 EDGE1
description: 本主题是一部分的测试实验室指南-使用 OTP 身份验证和 Windows Server 2016 的 RSA SecurID 演示 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5bb28ff6131c371e4b2f668fd20ec0a6133a0099
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859998"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>步骤 4： 安装和配置 RSA 和 EDGE1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

RSA 是 RADIUS 和 OTP 服务器，并配置 RADIUS 和 OTP 之前安装。  
  
将执行以下步骤以配置 RSA 部署：  
  
1. RSA 服务器上安装操作系统。 RSA 在服务器上安装 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
2. 在 RSA 上配置 TCP/IP。 RSA 服务器上配置 TCP/IP 设置。  
  
3. 将身份验证管理器安装文件复制到 RSA 服务器。 在安装后在 RSA，操作系统将身份验证管理器文件复制到 RSA 计算机。  
  
4. 将 RSA 服务器加入到 CORP 域。 将 RSA 加入到 CORP 域。  
  
5. 禁用 Windows 防火墙在 RSA 上。 禁用 RSA 服务器上的 Windows 防火墙。  
  
6. RSA 服务器上安装 RSA 身份验证管理器。 安装 RSA 身份验证管理器。  
  
7. 配置 RSA 身份验证管理器。 配置身份验证管理器。  
  
8. 创建 DAProbeUser。 创建用于探测目的的用户帐户。  
  
9. 在 CLIENT1 上安装 RSA SecurID 软件令牌。 在 CLIENT1 上安装 RSA SecurID 软件令牌。  
  
10. 将 EDGE1 配置为 RSA 身份验证代理。 EDGE1 上配置 RSA 身份验证代理。  
  
11. 支持 OTP 身份验证配置 EDGE1。 对于 DirectAccess，配置 OTP，并验证配置。  
  
## <a name="InstallOS"></a>RSA 服务器上安装操作系统  
  
1.  在 RSA 上启动 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的安装。  
  
2.  按照说明完成安装过程中，指定 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完全安装） 和本地管理员帐户的强密码。 使用本地管理员账户登录。  
  
3.  连接到具有 Internet 访问权限的网络的 RSA 并运行 Windows 更新，即可安装最新的更新适用于 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，然后从 Internet 断开连接。  
  
4.  连接到公司网络子网的 RSA。  
  
## <a name="TCP"></a>RSA 上配置 TCP/IP  
  
1.  在初始配置任务中，单击**配置网络**。  
  
2.  在中**网络连接**，右键单击**本地区域连接**，然后单击**属性**。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”**，然后单击 **“属性”**。  
  
4.  单击 **“使用下面的 IP 地址”**。 在“IP 地址” 中，键入“10.0.0.5” 。 在 **“子网掩码”** 框中，键入 **255.255.255.0**。 在中**默认网关**，类型**10.0.0.2**。 单击**使用以下 DNS 服务器地址**，在**首选 DNS 服务器**，类型**10.0.0.1**。  
  
5.  单击 **“高级”**，然后单击 **“DNS”** 选项卡。  
  
6.  在中**此连接的 DNS 后缀**，类型**corp.contoso.com**，然后单击**确定**两次。  
  
7.  上**本地连接属性**对话框中，单击**关闭**。  
  
8.  关闭 **“网络连接”** 窗口。  
  
## <a name="copyinstfiles"></a>将身份验证管理器安装文件复制到 RSA 服务器  
  
1.  RSA 服务器上创建文件夹 C:\RSA 安装。  
  
2.  将 RSA 身份验证管理器 7.1 SP4 媒体的内容复制到 C:\RSA 安装文件夹。  
  
3.  创建子文件夹 C:\RSA Installation\License 和令牌。  
  
4.  将 RSA 许可证文件复制到 C:\RSA Installation\License 和令牌。  
  
## <a name="JoinDomain"></a>将 RSA 服务器加入到 CORP 域  
  
1.  右键单击**我的电脑**，然后单击**属性**。  
  
2.  在 **“系统属性”** 对话框中的 **“计算机名”** 选项卡上，单击 **“更改”**。  
  
3.  在中**计算机名**，类型**RSA**。 在中**的成员**，单击**域**，类型**corp.contoso.com**，然后单击**确定**。  
  
4.  当系统提示输入用户名和密码的值时，键入**User1**和其密码，然后单击**确定**。  
  
5.  域欢迎对话框上单击**确定**。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定” 。  
  
7.  在“系统属性”  对话框中单击“关闭” 。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动” 。  
  
9. 重新启动计算机后，键入**User1**和密码，选择在 CORP**登录到：** 下拉列表中，然后单击**确定**。  
  
## <a name="BKMK_Firewall"></a>禁用在 RSA 上的 Windows 防火墙  
  
1.  单击**启动**，单击**控制面板**，单击**系统和安全**，然后单击**Windows 防火墙**。  
  
2.  单击**打开或关闭 Windows 防火墙**。  
  
3.  **关闭 Windows 防火墙**对于的所有设置。  
  
4.  单击**确定**并关闭 Windows 防火墙。  
  
## <a name="install"></a>RSA 服务器上安装 RSA 身份验证管理器  
  
1.  如果在此过程中随时会出现安全警告消息，请单击**运行**以继续。  
  
2.  打开 C:\RSA 安装文件夹，然后双击**autorun.exe**。  
  
3.  单击**立即安装**，单击**下一步**，选择最上面的选项美洲地区，然后单击**下一步**。  
  
4.  选择**我接受许可协议条款**，然后单击**下一步**。  
  
5.  选择**主实例**，然后单击**下一步**。  
  
6.  在中**目录名称：** 字段中，键入**C:\RSA**，然后单击**下一步**。  
  
7.  验证服务器名称 (RSA.corp.contoso.com) 和 IP 地址是否正确，然后单击**下一步**。  
  
8.  浏览到 C:\RSA Installation\License 和令牌，然后单击**下一步**。  
  
9. 上**验证许可证文件**页上，单击**下一步**。  
  
10. 在中**用户 ID**字段中，键入**管理员**，然后在**密码**并**确认密码**字段中键入一个强密码。 单击“下一步” 。  
  
11. 在日志选择屏幕上，接受默认值，然后单击**下一步**。  
  
12. 在摘要屏幕上单击**安装**。  
  
13. 安装完成后，单击**完成**。  
  
## <a name="confiauthmgr"></a>配置 RSA 身份验证管理器  
  
1.  如果在 RSA Security 控制台不会自动打开，然后在 RSA 计算机桌面上双击"RSA 安全控制台"。  
  
2.  如果安全证书警告 / 安全警报出现时，单击**继续浏览此网站**或单击**是**来继续操作，将此站点添加到受信任的站点，如果请求。  
  
3.  在中**用户 ID**字段中，键入**管理员**然后单击**确定**。  
  
4.  在中**密码**字段中，键入管理员帐户的密码，然后单击**Log On**。  
  
5.  插入令牌信息。  
  
    1.  在中**RSA Security Console**单击**身份验证**然后单击**SecurID 令牌**。  
  
    2.  单击**导入令牌作业**，然后单击**添加新**。  
  
    3.  在中**导入选项**部分中，单击**浏览**。 浏览到并选择在 C:\ 中的令牌 XML 文件RSA Installation\License 和令牌文件夹，然后单击**打开**。  
  
    4.  单击**提交作业**页的底部。  
  
6.  创建 OTP 新用户。  
  
    1.  在**RSA Security Console**单击**标识**选项卡上，单击**用户**，然后单击**添加新**。  
  
    2.  在中**Last Name:** 部分类型**用户**，然后在**用户 ID:** 部分类型**User1** （用户 Id 必须与用于的 AD 用户名相同此实验室）。  在中**密码：** 并**确认密码：** 部分键入一个强密码。 清除**要求用户在下次登录时更改密码**复选框，然后单击**保存**。  
  
7.  将 User1 分配给一个导入的令牌。  
  
    1.  上**用户**页上，单击**User1**然后单击**SecurID 令牌**。  
  
    2.  单击**SecurID 令牌**然后单击**令牌分配**。  
  
    3.  下**序列号**标题单击第一个数字列，然后单击**分配**。  
  
    4.  单击已分配的令牌，然后单击**编辑**。 在中**SecurID PIN 管理**部分，了解**用户身份验证要求**，选择**不需要 PIN （仅代码后面）**。  
  
    5.  单击**保存和分发标记**。  
  
    6.  上**分发软件令牌**页面**基础知识**部分中，单击**问题标记文件 (SDTID)**。  
  
    7.  上**分发软件令牌**页面**令牌文件选项**部分中，清除**启用复制保护**复选框。 单击**没有密码**并**下一步**。  
  
    8.  上**分发软件令牌**页面**下载的文件**部分中，单击**立即下载**。 单击“保存” 。 浏览到 C:\RSA 安装，然后单击**保存**并**关闭**。  
  
    9. 最大程度减少**RSA 安全控制台**供以后使用。  
  
8.  将身份验证管理器配置为 RADIUS 服务器。  
  
    1.  在 RSA 计算机桌面双击 **"RSA 安全操作控制台"**。  
  
    2.  如果安全证书警告 / 安全警报出现时，单击**继续浏览此网站**或单击**是**来继续操作，如果请求将此站点添加到受信任的站点。  
  
    3.  输入用户 ID 和密码，然后单击**Log On**。  
  
    4.  单击**部署配置的 RADIUS-将服务器配置**。  
  
    5.  上**所需的其他凭据**页上输入的管理员用户 ID 和密码，然后单击**确定**。  
  
    6.  上**配置 RADIUS 服务器**页上，输入管理员用户使用的相同密码**机密**并**主密码**。 输入管理员用户 ID 和密码，然后单击**配置**。  
  
    7.  验证消息**已成功配置的 RADIUS 服务器**显示。 单击 **“完成”**。 关闭**RSA 操作控制台**。  
  
    8.  切换回 **"RSA 安全控制台"**。  
  
    9. 上**RADIUS**选项卡上单击**RADIUS 服务器**。 验证列出该 rsa.corp.contoso.com。  
  
9. 将 RSA 服务器配置为 RSA 身份验证客户端。  
  
    1.  上**RADIUS**选项卡上，单击**RADIUS 客户端**并**添加新**。  
  
    2.  单击**ANY RADIUS 客户端**复选框。  
  
    3.  键入在所选的一个强密码**共享机密**字段。 更高版本为 OTP 配置 EDGE1 时，将使用此相同的密码。  
  
    4.  将保留**IP 地址**字段留空，并**使 / Model**项作为**标准 RADIUS**。  
  
    5.  单击**RSA 代理保存**。  
  
10. 创建所需的配置 EDGE1 RSA 身份验证代理所在的文件。  
  
    1.  上**访问**选项卡上，突出显示**身份验证代理**，然后单击**添加新**。  
  
    2.  类型**EDGE1**中**主机名**字段，然后单击**解决 IP**。  
  
    3.  请注意，在现在显示 EDGE1 的 IP 地址**IP 地址**字段。 单击“保存” 。  
  
11. 生成 EDGE1 服务器 (AM_Config.zip) 的配置文件。  
  
    1.  上**访问**选项卡上，突出显示**身份验证代理**，然后单击**生成配置文件**。  
  
    2.  上**生成配置文件**页上，单击**生成配置文件**，然后单击**立即下载**。  
  
    3.  单击**保存**，浏览到 C:\RSA 安装，然后单击**保存**。  
  
    4.  单击**关闭**上**下载完整**对话框。  
  
12. 生成 EDGE1 服务器 (EDGE1_NodeSecret.zip) 的节点密钥文件。  
  
    1.  上**访问**选项卡上，突出显示**身份验证代理**，然后单击**管理现有**。  
  
    2.  单击当前配置的节点 EDGE1，然后单击**管理节点密钥**。  
  
    3.  检查**创建一个新的随机节点机密，并导出到文件的节点密钥**复选框。  
  
    4.  输入管理员用户使用同一密码**加密密码**并**确认加密密码**字段，然后单击**保存**。  
  
    5.  上**生成的节点的机密文件**页上，单击**立即下载**。  
  
    6.  上**文件下载**对话框中，单击**保存**，浏览到 C:\RSA 安装，然后单击**保存**。 单击**关闭**上**下载完整**对话框。  
  
    7.  从 RSA 身份验证管理器媒体副本 \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe 到 C:\RSA 安装。  
  
## <a name="BKMK_DAProbeUser"></a>创建 DAProbeUser  
  
1.  在**RSA Security Console**单击**标识**选项卡上，单击**用户**，然后单击**添加新**。  
  
2.  在中**Last Name:** 部分类型**探测**，然后在**用户 ID:** 部分类型**DAProbeUser**。 在中**密码：** 并**确认密码：** 部分键入一个强密码。 清除**要求用户在下次登录时更改密码**复选框，然后单击**保存**。  
  
## <a name="InstToken"></a>在 CLIENT1 上安装 RSA SecurID 软件令牌  
使用此过程在 CLIENT1 上安装 SecurID 软件令牌。  
  
#### <a name="install-securid-software-token"></a>安装 SecurID 软件令牌  
  
1.  在 CLIENT1 计算机上创建文件夹 C:\RSA 文件。 在将文件复制 Software_Tokens.zip 从 C:\RSA 安装 RSA 计算机到 C:\RSA 文件。 提取文件 User1_000031701832.SDTID C:\RSA 在 CLIENT1 上的文件。  
  
2.  访问 RSA SecurID 软件令牌媒体源，然后双击中的 RSASECURIDTOKEN410 **SecurID SoftwareToken 客户端应用**要从中启动 RSA SecurID 安装文件夹。 如果**打开的文件-安全警告**将显示消息，然后单击**运行**。  
  
3.  上**RSA SecurID 软件令牌-InstallShield 向导**对话框中，单击**下一步**两次。  
  
4.  接受许可协议，然后单击**下一步**。  
  
5.  上**安装类型**对话框中，选择**典型**，单击**下一步**，然后单击**安装**。  
  
6.  如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
7.  选择**启动 RSA SecurID 软件令牌**复选框，然后单击**完成**。  
  
8.  单击**从文件导入**。  
  
9. 单击**浏览**，选择 C:\RSA Files\User1_000031701832.SDTID，然后单击**打开**。  
  
10. 单击“确定”两次  。  
  
## <a name="configAuthAgt"></a>将 EDGE1 配置为 RSA 身份验证代理  
使用此过程来配置 EDGE1 执行 RSA 身份验证。  
  
#### <a name="configure-the-rsa-authentication-agent"></a>配置 RSA 身份验证代理  
  
1.  EDGE1 上打开 Windows 资源管理器并创建 C:\RSA 文件的文件夹。 浏览到 RSA ACE 安装介质。  
  
2.  复制文件 agent_nsload.exe，AM_Config.zip 和从 RSA 媒体 EDGE1_NodeSecret.zip C:\RSA 文件。  
  
3.  提取到以下位置这两个 zip 文件的内容：  
  
    1.  C:\Windows\system32\  
  
    2.  C:\Windows\SysWOW64\  
  
4.  将 agent_nsload.exe 复制到 C:\Windows\SysWOW64\\。  
  
5.  打开提升的命令提示符并导航到 C:\Windows\SysWOW64。  
  
6.  类型**agent_nsload.exe-f nodesecret.rec-p <password>** 其中<password>RSA 进行初始配置期间创建的强密码。 按 Enter。  
  
7.  将 C:\Windows\SysWOW64\securid 复制到 C:\Windows\System32。  
  
## <a name="configOTP"></a>将 EDGE1 配置为支持 OTP 身份验证  
使用此过程对于 DirectAccess，配置 OTP，并验证配置。  
  
#### <a name="configure-otp-for-directaccess"></a>为 DirectAccess 配置 OTP  
  
1.  在 EDGE1，打开服务器管理器，然后单击**远程访问**的左窗格中。  
  
2.  右键单击**EDGE1**服务器窗格中，然后选择**远程访问管理**。  
  
3.  单击**配置**。  
  
4.  在中**DirectAccess 安装**窗口下**步骤 2-远程访问服务器**，单击**编辑**。  
  
5.  单击**下一步**三次，然后在**身份验证**部分中，选择**双因素身份验证**并**使用 OTP**，并确保**使用计算机证书**检查。 验证根 CA 设置为**CN = corp-APP1-CA**。 单击“下一步” 。  
  
6.  在中**OTP RADIUS 服务器**部分中，双击空白**服务器名称**字段。  
  
7.  在中**添加 RADIUS 服务器**对话框中，键入**RSA**中**服务器名称**字段。 单击**更改**旁边**共享机密**字段，并键入中的 RSA 服务器上配置 RADIUS 客户端时使用的相同密码**新机密**并**确认新的机密**字段。 单击**确定**两次，然后单击**下一步**。  
  
    > [!NOTE]  
    > 如果 RADIUS 服务器位于域不同于远程访问服务器，则**服务器名称**字段必须指定 RADIUS 服务器的 FQDN。  
  
8.  在中**OTP CA 服务器**部分选择 APP1.corp.contoso.com，然后单击**添加**。 单击“下一步” 。  
  
9. 上**OTP 证书模板**页上，单击**浏览**若要选择用于进行 OTP 身份验证，并在颁发的证书的注册证书模板**证书模板**对话框中，选择**DAOTPLogon**。 单击 **“确定”**。 单击**浏览**若要选择用于注册的远程访问服务器登录 OTP 证书注册请求，并在使用的证书的证书模板**证书模板**对话框选择**DAOTPRA**。 单击“确定”。 单击“下一步” 。  
  
10. 上**远程访问服务器设置**页上，单击**完成**，然后单击**完成**上**DirectAccess 方面的专家向导**。  
  
11. 上**远程访问评审**对话框中，单击**应用**，等待 DirectAccess 策略更新，然后单击**关闭**。  
  
12. 上**启动**屏幕上，键入**powershell.exe**，右键单击**powershell**，单击**高级**，然后单击**以运行管理员**。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
13. 在 Windows PowerShell 窗口中，键入**gpupdate /force**然后按 ENTER。  
  
14. 关闭并重新打开远程访问管理控制台并验证所有 OTP 设置都正确。  
  



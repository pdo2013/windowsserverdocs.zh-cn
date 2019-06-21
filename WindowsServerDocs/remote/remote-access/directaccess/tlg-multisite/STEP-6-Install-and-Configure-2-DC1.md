---
title: 步骤 6 安装和配置 2 DC1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c3e12f9d64619ce00e578c7a8096c764e68ff7d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283161"
---
# <a name="step-6-install-and-configure-2-dc1"></a>步骤 6 安装和配置 2 DC1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

2-DC1 提供了以下服务：  
  
-   Corp2.corp.contoso.com Active Directory 域服务 (AD DS) 域的域控制器。  
  
-   Corp2.corp.contoso.com DNS 域的 DNS 服务器。  
  
2-DC1 配置由以下内容组成：  
  
- 2-DC1 上安装操作系统
  
- 配置 TCP/IP 属性

- 2-DC1 配置为域控制器和 DNS 服务器

- 为 CORP\User1 提供组策略权限
  
- 允许 CORP2 计算机获取计算机证书
  
- 强制 DC1 和 2 DC1 之间的复制
  
## <a name="install-the-operating-system-on-2-dc1"></a>2-DC1 上安装操作系统  
首先，安装 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>2-DC1 上安装操作系统  
  
1.  启动 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的安装。  
  
2.  按照说明完成安装过程中，指定 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完全安装） 和本地管理员帐户的强密码。 使用本地管理员账户登录。  
  
3.  连接到具有 Internet 访问权限的网络的 2-DC1 并运行 Windows 更新，即可安装最新的更新适用于 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，然后从 Internet 断开连接。  
  
4.  连接到 2 Corpnet 子网 2-DC1。  
  
## <a name="configure-tcpip-properties"></a>配置 TCP/IP 属性  
使用静态 IP 地址配置 TCP/IP 协议。  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>2-DC1 上配置 TCP/IP  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**有线以太网连接**，单击的链接。  
  
2.  在 **“网络连接”** 中，右键单击 **“有线以太网连接”** ，然后单击 **“属性”** 。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  单击 **“使用下面的 IP 地址”** 。 在中**IP 地址**，类型**10.2.0.1**。 在 **“子网掩码”** 框中，键入 **255.255.255.0**。 在中**默认网关**，类型**10.2.0.254**。 单击**使用以下 DNS 服务器地址**，在**首选 DNS 服务器**，类型**10.2.0.1**，然后在**备用 DNS 服务器**，类型**10.0.0.1**。  
  
5.  单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
6.  在中**此连接的 DNS 后缀**，类型**corp2.corp.contoso.com**，然后单击**确定**两次。  
  
7.  单击“Internet 协议版本 6 (TCP/IPv6)”  ，然后单击“属性”  。  
  
8.  单击**使用下列 IPv6 地址**。 在中**IPv6 地址**，类型**2001:db8:2::1**。 在中**子网前缀长度**，类型**64**。 在中**默认网关**，类型**2001:db8:2::fe**。 单击**使用以下 DNS 服务器地址**，在**首选 DNS 服务器**，类型**2001:db8:2::1**，然后在**备用 DNS 服务器**，类型**2001:db8:1::1**。  
  
9. 单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
10. 在中**此连接的 DNS 后缀**，类型**corp2.corp.contoso.com**，然后单击**确定**两次。  
  
11. 上**有线以太网连接属性**对话框中，单击**关闭**。  
  
12. 关闭 **“网络连接”** 窗口。  
  
13. 在服务器管理器控制台中，在**本地服务器**，在**属性**区域中，为下的一步**计算机名称**，单击的链接。  
  
14. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
15. 上**计算机名/域更改**对话框中**计算机名**，类型**2 DC1**，然后单击**确定**。  
  
16. 当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
17. 在“系统属性”  对话框中单击“关闭”  。  
  
18. 当系统提示你重新启动计算机时，请单击“立即重新启动”  。  
  
19. 重启后，使用本地管理员帐户登录。  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>2-DC1 配置为域控制器和 DNS 服务器  
配置 2-DC1 用作 corp2.corp.contoso.com 域的域控制器以及 corp2.corp.contoso.com DNS 域的 DNS 服务器。  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>2-DC1 配置为域控制器和 DNS 服务器  
  
1.  在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击**下一步**三次以转到服务器角色选择屏幕  
  
3.  上**选择服务器角色**页上，选择**Active Directory 域服务**。 单击**添加功能**当系统提示，然后单击**下一步**三次。  
  
4.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  
  
5.  安装成功完成后，单击**提升此服务器的域控制器到**。  
  
6.  在 Active Directory 域服务配置向导中，在**部署配置**页上，单击**将新域添加到现有林**。  
  
7.  在中**父域名称**，类型**corp.contoso.com**，在**新域名**，类型**corp2**。  
  
8.  下**提供凭据以执行此操作**，单击**更改**。 上**Windows 安全性**对话框中**用户名**，类型**corp.contoso.com\Administrator**，然后在**密码**，输入 corp\管理员密码，单击**确定**，然后单击**下一步**。  
  
9. 上**域控制器选项**页上，请确保**站点名称**是**第二个站点**。 下**键入目录服务还原模式 (DSRM) 密码**，在**密码**并**确认密码**两次，键入强密码，然后单击**下一步**五次。  
  
10. 上**必备项检查**页上之后验证系统必备组件后，单击**安装**。  
  
11. 等待，直到向导完成 Active Directory 和 DNS 服务的配置，然后单击**关闭**。  
  
12. 在计算机重新启动后，登录到 CORP2 域使用管理员帐户。  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>为 CORP\User1 提供组策略权限  
使用此过程使用 CORP\User1 用户提供创建和更改 corp2 组策略对象的完全权限。  
  
### <a name="to-provide-group-policy-permissions"></a>若要提供组策略的权限  
  
1.  上**启动**屏幕上，键入**gpmc.msc**，然后按 ENTER。  
  
2.  在组策略管理控制台中，打开**林： corp.contoso.com/Domains/corp2.corp.contoso.com**。  
  
3.  在详细信息窗格中，单击**委派**选项卡。在中**权限**下拉列表中，单击**链接 Gpo**。  
  
4.  单击**外**，和的新**选择用户、 计算机或组**对话框中，单击**位置**。  
  
5.  上**位置**对话框中**位置**树中，单击**corp.contoso.com**，然后单击**确定**。  
  
6.  在中**输入要选择的对象名称**类型**User1**，单击**确定**，然后在**添加组或用户**对话框中，单击**确定**.  
  
7.  在组策略管理控制台中，在树中，单击**组策略对象**，并在详细信息窗格中单击**委派**选项卡。  
  
8.  单击**外**，和的新**选择用户、 计算机或组**对话框中，单击**位置**。  
  
9. 上**位置**对话框中**位置**树中，单击**corp.contoso.com**，然后单击**确定**。  
  
10. 在中**输入要选择的对象名称**类型**User1**，单击**确定**。  
  
11. 在组策略管理控制台中，在树中，单击**WMI 筛选器**，并在详细信息窗格中单击**委派**选项卡。  
  
12. 单击**外**，和的新**选择用户、 计算机或组**对话框中，单击**位置**。  
  
13. 上**位置**对话框中**位置**树中，单击**corp.contoso.com**，然后单击**确定**。  
  
14. 在中**输入要选择的对象名称**类型**User1**，单击**确定**。 上**添加组或用户**对话框框中，请确保**权限**设置为**完全控制**，然后单击**确定**。  
  
15. 关闭“组策略管理控制台”。  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>允许 CORP2 计算机获取计算机证书 

CORP2 域中的计算机必须从在 APP1 上的证书颁发机构获取计算机证书。 在 APP1 上执行此过程。  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>若要允许 CORP2 计算机自动获取计算机证书  
  
1.  在 APP1 上，单击**启动**，类型**certtmpl.msc**，然后按 ENTER。  
  
2.  在中**证书模板控制台**，在中间窗格中，双击**客户端-服务器身份验证**。  
  
3.  上**客户端-服务器身份验证属性**对话框中，单击**安全**选项卡。  
  
4.  单击**外**，然后在**选择用户、 计算机、 服务帐户或组**对话框中，单击**位置**。  
  
5.  上**位置**对话框中**位置**，展开**corp.contoso.com**，单击**corp2.corp.contoso.com**，然后单击**确定**。  
  
6.  在中**输入要选择的对象名称**，类型**Domain Admins;域计算机**，然后单击**确定**。  
  
7.  上**客户端-服务器身份验证属性**对话框中**组或用户名**，单击**域管理员 （CORP2\Domain 管理员）** ，然后在**域管理员的权限**，请在**允许**列中，选择**编写**并**注册**。  
  
8.  在**组或用户名**，单击**域计算机 （CORP2\Domain 计算机）** ，然后在**域的计算机的权限**，在**允许**列中，选择**注册**并**自动注册**，然后单击**确定**。  
  
9. 关闭“证书模板控制台”。  
  
## <a name="replication"></a>强制 DC1 和 2 DC1 之间的复制  
可以为 2 EDGE1 上的证书注册之前，必须强制设置从 DC1 到 2 DC1 的复制。 在 DC1 上应进行此操作。  
  
### <a name="to-force-replication"></a>若要强制进行复制  
  
1.  在 DC1 上，单击**启动**，然后单击**Active Directory 站点和服务**。  
  
2.  在 Active Directory 站点和服务控制台中，在树中，展开**站点间的传输**，然后单击**IP**。  
  
3.  在详细信息窗格中，双击**DEFAULTIPSITELINK**。  
  
4.  上**DEFAULTIPSITELINK 属性**对话框中**成本**，类型**1**，在**复制每个**，类型**15**，然后单击**确定**。 等待 15 分钟的复制操作完成。  
  
5.  若要强制复制现在在控制台树中，展开**Sites\Default 第一个站点 name\Servers\DC1\NTDS 设置**，在详细信息窗格中，右键单击 **<automatically generated>** ，单击**立即复制副本**，然后在**立即复制副本**对话框中，单击**确定**。  
  
6.  若要确保复制已成功完成执行以下操作：  
  
    1.  上**启动**屏幕上，键入**cmd.exe**，然后按 ENTER。  
  
    2.  输入以下命令，然后按 ENTER。  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  请确保同步且未出错的所有分区。 如果没有，然后重新运行该命令，直到在继续之前未不报告任何错误。  
  
7.  关闭命令提示符窗口。  
  



---
title: 步骤6安装和配置 2-DC1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c7a8243922f58f9705a85cd30b2a68cf4d876c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404772"
---
# <a name="step-6-install-and-configure-2-dc1"></a>步骤6安装和配置 2-DC1

>适用于：Windows Server（半年频道）、Windows Server 2016

2-DC1 提供以下服务：  
  
-   Corp2.corp.contoso.com Active Directory 域服务（AD DS）域的域控制器。  
  
-   Corp2.corp.contoso.com DNS 域的 DNS 服务器。  
  
2-DC1 配置由以下内容组成：  
  
- 在 2-DC1 上安装操作系统
  
- 配置 TCP/IP 属性

- 将 2-DC1 配置为域控制器和 DNS 服务器

- 向 CORP\User1 提供组策略权限
  
- 允许 CORP2 计算机获取计算机证书
  
- 在 DC1 和2之间强制复制-DC1
  
## <a name="install-the-operating-system-on-2-dc1"></a>在 2-DC1 上安装操作系统  
首先，安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>在 2-DC1 上安装操作系统  
  
1.  开始安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
2.  按照说明完成安装，为本地管理员帐户指定 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完全安装）和强密码。 使用本地管理员账户登录。  
  
3.  将 2-DC1 连接到可访问 Internet 的网络，并运行 Windows 更新以安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然后从 Internet 断开连接。  
  
4.  将 2-DC1 连接到2公司网络子网。  
  
## <a name="configure-tcpip-properties"></a>配置 TCP/IP 属性  
配置具有静态 IP 地址的 TCP/IP 协议。  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>在 2-DC1 上配置 TCP/IP  
  
1.  在服务器管理器控制台中，单击 "**本地服务器**"，然后在 "**有线以太网连接**" 旁边的 "**属性**" 区域中，单击链接。  
  
2.  在 **“网络连接”** 中，右键单击 **“有线以太网连接”** ，然后单击 **“属性”** 。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  单击 **“使用下面的 IP 地址”** 。 在 " **IP 地址**" 中，键入**10.2.0.1**。 在 **“子网掩码”** 框中，键入 **255.255.255.0**。 在 "**默认网关**" 中，键入**10.2.0.254**。 单击 "**使用以下 dns 服务器地址**"，在 "**首选 dns 服务器**" 中，键入**10.2.0.1**，在 "**备用 dns 服务器**" 中键入**10.0.0.1**。  
  
5.  单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
6.  在 "**此连接的 DNS 后缀**" 中，键入**corp2.corp.contoso.com**，然后单击 **"确定"** 两次。  
  
7.  单击“Internet 协议版本 6 (TCP/IPv6)”，然后单击“属性”。  
  
8.  单击 **"使用下列 IPv6 地址"** 。 在 " **IPv6 地址**" 中，键入 " **2001： db8：2：： 1**"。 在 "**子网前缀长度**" 中，键入**64**。 在 "**默认网关**" 中，键入 " **2001： db8：2：： fe**"。 单击 "**使用以下 dns 服务器地址**"，在 "**首选 dns 服务器**" 中 **，键入 "** **2001： db8：2：： 1**"，然后在 "**备用 dns 服务器**" 中，键入 ""。  
  
9. 单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
10. 在 "**此连接的 DNS 后缀**" 中，键入**corp2.corp.contoso.com**，然后单击 **"确定"** 两次。  
  
11. 在 "**有线以太网连接属性**" 对话框中，单击 "**关闭**"。  
  
12. 关闭 **“网络连接”** 窗口。  
  
13. 在服务器管理器控制台中，在 "**本地服务器**" 的 "**属性**" 区域中，单击 "**计算机名**" 旁边的链接。  
  
14. 在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
15. 在 "**计算机名/域更改**" 对话框的 "**计算机名**" 中，键入 " **2-DC1**"，然后单击 **"确定"** 。  
  
16. 当系统提示你必须重新启动计算机时，请单击“确定”。  
  
17. 在“系统属性” 对话框中单击“关闭”。  
  
18. 当系统提示你重新启动计算机时，请单击“立即重新启动”。  
  
19. 重新启动后，使用本地管理员帐户登录。  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>将 2-DC1 配置为域控制器和 DNS 服务器  
将 2-DC1 配置为 corp2.corp.contoso.com 域的域控制器，并将其配置为 corp2.corp.contoso.com DNS 域的 DNS 服务器。  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>将 2-DC1 配置为域控制器和 DNS 服务器  
  
1.  在服务器管理器控制台的 "**仪表板**" 上，单击 "**添加角色和功能**"。  
  
2.  单击 "**下一步**" 三次以转到服务器角色选择屏幕  
  
3.  在 "**选择服务器角色**" 页上，选择**Active Directory 域服务**。 在出现提示时单击 "**添加功能**"，然后单击 "**下一步**" 三次。  
  
4.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  
  
5.  安装成功完成后，单击 "**将此服务器提升为域控制器**"。  
  
6.  在 Active Directory 域服务配置向导的 "**部署配置**" 页上，单击 "**将新域添加到现有林**"。  
  
7.  在 "**父域名**" 中，在 "**新域名**" 中键入**corp.contoso.com**，键入**corp2**。  
  
8.  在 "**提供用于执行此操作的凭据**" 下，单击 "**更改**"。 在 " **Windows 安全性**" 对话框中，在 "**用户名**" 中键入 " **Com\Administrator**"，在 "**密码**" 中输入 corp\ 管理员密码，单击 **"确定**"，然后单击 "**下一步**"。  
  
9. 在 "**域控制器选项**" 页上，确保**站点名称**为**第二个站点**。 在 **"键入目录服务还原模式（DSRM）密码**" 下的 "**密码**" 和 "**确认密码**" 中，键入强密码两次，然后单击 "**下一步**" 五次。  
  
10. 在 "**先决条件检查**" 页上，验证必备项后，单击 "**安装**"。  
  
11. 等待向导完成 Active Directory 和 DNS 服务的配置，然后单击 "**关闭**"。  
  
12. 计算机重启后，使用管理员帐户登录到 CORP2 域。  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>向 CORP\User1 提供组策略权限  
使用此过程向 CORP\User1 用户提供创建和更改 corp2 组策略对象的完全权限。  
  
### <a name="to-provide-group-policy-permissions"></a>提供组策略权限  
  
1.  在 "**开始**" 屏幕上，键入**gpmc**，然后按 enter。  
  
2.  在组策略管理控制台中，打开 "**林： corp.contoso.com/Domains/corp2.corp.contoso.com**"。  
  
3.  在详细信息窗格中，单击 "**委派**" 选项卡。在 "**权限**" 下拉列表中，单击 "**链接 gpo**"。  
  
4.  单击 "**添加**"，然后在 "新建**选择用户、计算机或组**" 对话框中，单击 "**位置**"。  
  
5.  在 "**位置**" 对话框的 "**位置**" 树中，单击 " **Corp.contoso.com**"，然后单击 **"确定"** 。  
  
6.  在 "**输入要选择的对象名称"** 中，单击 **"确定** **"，然后**在 "**添加组或用户**" 对话框中，单击 **"确定"** 。  
  
7.  在组策略管理控制台中，在树中单击 "**组策略对象**"，然后在详细信息窗格中单击 "**委派**" 选项卡。  
  
8.  单击 "**添加**"，然后在 "新建**选择用户、计算机或组**" 对话框中，单击 "**位置**"。  
  
9. 在 "**位置**" 对话框的 "**位置**" 树中，单击 " **Corp.contoso.com**"，然后单击 **"确定"** 。  
  
10. 在 "**输入要选择的对象名称"** **中，单击** **"确定"** 。  
  
11. 在组策略管理控制台中，单击 " **WMI 筛选器**"，然后在详细信息窗格中单击 "**委派**" 选项卡。  
  
12. 单击 "**添加**"，然后在 "新建**选择用户、计算机或组**" 对话框中，单击 "**位置**"。  
  
13. 在 "**位置**" 对话框的 "**位置**" 树中，单击 " **Corp.contoso.com**"，然后单击 **"确定"** 。  
  
14. 在 "**输入要选择的对象名称"** **中，单击** **"确定"** 。 在 "**添加组或用户**" 对话框中，确保 "**权限**" 设置为 "**完全控制**"，然后单击 **"确定"** 。  
  
15. 关闭“组策略管理控制台”。  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>允许 CORP2 计算机获取计算机证书 

CORP2 域中的计算机必须从 APP1 上的证书颁发机构获取计算机证书。 在 APP1 上执行此过程。  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>允许 CORP2 计算机自动获取计算机证书  
  
1.  在 APP1 上，单击 "**开始**"，键入**certtmpl**，然后按 enter。  
  
2.  在**证书模板控制台**的中间窗格中，双击 "**客户端-服务器身份验证**"。  
  
3.  在 "**客户端-服务器身份验证属性**" 对话框中，单击 "**安全**" 选项卡。  
  
4.  单击 "**添加**"，然后在 "**选择用户、计算机、服务帐户或组**" 对话框中，单击 "**位置**"。  
  
5.  在 "**位置**" 对话框的 "**位置**" 中，展开 " **corp.contoso.com**"，单击 " **Corp2.corp.contoso.com**"，然后单击 **"确定"** 。  
  
6.  在 "**输入要选择的对象名称**" 中，键入**Domain Admins;域计算机**，然后单击 **"确定"** 。  
  
7.  在 "**客户端-服务器身份验证属性**" 对话框的 "**组或用户名**" 中，单击 "**域管理员" （CORP2\Domain Admins）** ，并在 "**域管理员**" 的 "**允许**" 列中，选择 "**写入**"和**注册**。  
  
8.  在 **"组或用户名**" 中，单击 "**域计算机" （CORP2\Domain 计算机）** ，并在 "**域计算机的权限**" 中的 "**允许**" 列中，选择 "**注册**和**自动注册**"，然后单击 **"确定"** 。  
  
9. 关闭“证书模板控制台”。  
  
## <a name="replication"></a>在 DC1 和2之间强制复制-DC1  
在 EDGE1 上注册证书之前，必须强制从 DC1 到 2-DC1 的设置复制。 应在 DC1 上完成此操作。  
  
### <a name="to-force-replication"></a>强制复制  
  
1.  在 DC1 上，单击 "**开始**"，然后单击 " **Active Directory 站点和服务**"。  
  
2.  在 Active Directory 站点和服务控制台中，在树中展开 "**站点间传输**"，然后单击 " **IP**"。  
  
3.  在详细信息窗格中，双击 " **DEFAULTIPSITELINK**"。  
  
4.  在 " **DEFAULTIPSITELINK 属性**" 对话框的 "**开销**" 中，键入**1**，在 "**复制频率**" 中，键入**15**，然后单击 **"确定"** 。 等待15分钟，以完成复制。  
  
5.  若要立即在控制台树中强制复制，请展开 " **Sites\Default-First-Site-name\Servers\DC1\NTDS 设置**"，在详细信息窗格中，右键单击 **<automatically generated>** ，单击 "**立即复制**"，然后在 "**立即复制**" 对话框中，单击 **"确定"** 。  
  
6.  若要确保复制已成功完成，请执行以下操作：  
  
    1.  在 "**开始**" 屏幕上，键入 "**cmd.exe**"，然后按 enter。  
  
    2.  输入以下命令，然后按 ENTER。  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  请确保所有分区都同步且没有错误。 如果未报告任何错误，请重新运行该命令，然后继续。  
  
7.  关闭命令提示符窗口。  
  



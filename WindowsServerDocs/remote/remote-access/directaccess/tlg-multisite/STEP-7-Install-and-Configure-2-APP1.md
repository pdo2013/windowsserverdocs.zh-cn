---
title: 步骤7安装和配置 2-APP1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c5a316e1230692fb800c088d752c26ec4a0f3349
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388264"
---
# <a name="step-7-install-and-configure-2-app1"></a>步骤7安装和配置 2-APP1

>适用于：Windows Server（半年频道）、Windows Server 2016

APP1 提供 web 和文件共享服务。 2-APP1 配置由以下内容组成：  
  
- 在 2-APP1 上安装操作系统  
  
- 配置 TCP/IP 属性  
  
- 将 APP1 加入 CORP2 域  
  
- 在 APP1 上安装 Web 服务器（IIS）角色  
  
- 在 2-APP1 上创建共享文件夹 
  
## <a name="bkmk_InstallOS"></a>在 2-APP1 上安装操作系统  
首先，安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>在 2-APP1 上安装操作系统  
  
1.  开始安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完全安装）。  
  
2.  按照说明完成安装，指定本地管理员账户的密码（强）。 使用本地管理员账户登录。  
  
3.  将 APP1 连接到具有 Internet 访问权限的网络，并运行 Windows 更新以安装 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然后从 Internet 断开连接。  
  
4.  将 2-APP1 连接到2公司网络子网。  
  
## <a name="bkmk_TCP"></a>配置 TCP/IP 属性  
配置 APP1 上的 TCP/IP 属性。  
  
#### <a name="to-configure-tcpip-properties"></a>配置 TCP/IP 属性的步骤  
  
1.  在服务器管理器控制台中，单击 "**本地服务器**"，然后在 "**有线以太网连接**" 旁边的 "**属性**" 区域中，单击链接。  
  
2.  在 "**网络连接**" 窗口中，右键单击 "**有线以太网连接**"，然后单击 "**属性**"。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  单击 **“使用下面的 IP 地址”** 。 在 " **IP 地址**" 中，键入**10.2.0.3**。 在 **“子网掩码”** 框中，键入 **255.255.255.0**。 在 "**默认网关**" 中，键入**10.2.0.254**。  
  
5.  单击 **“使用下面的 DNS 服务器地址”** 。 在 "**首选 DNS 服务器**" 中，键入**10.2.0.1**。  
  
6.  单击 **“高级”** ，然后单击 **“DNS”** 选项卡。在 "**此连接的 DNS 后缀**" 中，键入**corp2.corp.contoso.com**，并单击 **"确定"** 两次。  
  
7.  单击“Internet 协议版本 6 (TCP/IPv6)”，然后单击“属性”。  
  
8.  单击 **"使用下列 IPv6 地址"** 。 在 " **IPv6 地址**" 中，键入 " **2001： db8：2：： 3**"。 在 "**子网前缀长度**" 中，键入**64**。 在 "**默认网关**" 中，键入 " **2001： db8：2：： fe**"。 单击 "**使用以下 dns 服务器地址**"，并在 "**首选 dns 服务器**" 中，键入 " **2001：**  
  
9. 单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
10. 在 "**此连接的 DNS 后缀**" 中，键入**corp2.corp.contoso.com**，然后单击 **"确定"** 两次。  
  
11. 在 "**有线以太网连接属性**" 对话框中，单击 "**关闭**"。  
  
12. 关闭 **“网络连接”** 窗口。  
  
## <a name="bkmk_JoinDomain"></a>将 APP1 加入 CORP2 域  
将 APP1 加入到 corp2.corp.contoso.com 域。  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>将 APP1 加入 CORP2 域  
  
1.  在服务器管理器控制台中，在 "**本地服务器**" 的 "**属性**" 区域中，单击 "**计算机名**" 旁边的链接。  
  
2.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
3.  在 "**计算机名**" 中，键入**2-APP1**。 在 "**成员**" 中，单击 "**域**"，键入**Corp2.corp.contoso.com**，然后单击 **"确定"** 。  
  
4.  当系统提示你输入用户名和密码时，请键入**Administrator**及其密码，然后单击 **"确定"** 。  
  
5.  出现欢迎使用 corp2.corp.contoso.com 域的对话框时，请单击 **"确定"** 。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”。  
  
7.  在“系统属性” 对话框中单击“关闭”。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”。  
  
9. 计算机重启后，单击 "**切换用户**"，然后单击 "**其他用户**"，然后用管理员帐户登录到 CORP2 域。  
  
## <a name="bkmk_IIS"></a>在 APP1 上安装 Web 服务器（IIS）角色  
安装 Web 服务器（IIS）角色，使 web 服务器 APP1。  
  
#### <a name="to-install-the-web-server-iis-role"></a>安装 Web 服务器（IIS）角色  
  
1.  在服务器管理器控制台的 "**仪表板**" 上，单击 "**添加角色和功能**"。  
  
2.  单击 "**下一步**" 三次以转到服务器角色选择屏幕  
  
3.  在 "**选择服务器角色**" 页上，选择 " **WEB 服务器（IIS）** "，然后单击 "**下一步**" 四次。  
  
4.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  
  
5.  验证安装是否成功，然后单击 "**关闭**"。  
  
## <a name="bkmk_Share"></a>在 2-APP1 上创建共享文件夹  
在 APP1 上的文件夹中创建共享文件夹和文本文件。  
  
#### <a name="to-create-a-shared-folder"></a>创建共享文件夹  
  
1.  在 "**开始**" 屏幕上，键入 "**资源管理器**"，然后按 enter。  
  
2.  单击 "**计算机**"，然后双击 "**本地磁盘（C：）** "。  
  
3.  单击 "**新建文件夹**"，键入**文件**，然后按 enter。 使 "**本地磁盘**" 窗口保持打开状态。  
  
4.  在 "**开始**" 屏幕上，键入**notepad.exe**，右键单击 "**记事本**"，单击 "**高级**"，然后单击 "以**管理员身份运行**"。  
  
5.  在 "**无标题-记事本**" 窗口中，键入**这是 APP1 上的共享文件**。  
  
6.  依次单击 "**文件**"、"**保存**"、"**计算机**"，双击 "**本地磁盘（C：）** "，然后双击 "**文件**" 文件夹。  
  
7.  在 **"文件名"** 中，键入**example .txt**，然后单击 "**保存**"。 关闭记事本。  
  
8.  在 "**本地磁盘**" 窗口中，右键单击**Files**文件夹，指向 "**共享**"，然后单击 "**特定用户**"。  
  
9. 在 "**文件共享**" 对话框的下拉列表中，单击 "**所有人**"，然后单击 "**添加**"。 在 "适用于**每个人**的**权限级别**" 中，单击 "**读/写**"。  
  
10. 单击 "**共享**"，然后单击 "**完成**"。  
  
11. 关闭 "**本地磁盘**" 窗口。  
  



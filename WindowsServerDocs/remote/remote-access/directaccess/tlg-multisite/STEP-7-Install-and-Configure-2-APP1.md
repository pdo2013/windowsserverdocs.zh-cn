---
title: 步骤 7 安装和配置 2-APP1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4746ff5118814506d20983d3570881366297322f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283183"
---
# <a name="step-7-install-and-configure-2-app1"></a>步骤 7 安装和配置 2-APP1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

2-APP1 提供 web 和文件共享服务。 2-APP1 配置包括以下：  
  
- 2-APP1 上安装操作系统  
  
- 配置 TCP/IP 属性  
  
- 2-APP1 加入 CORP2 域  
  
- 在 2-APP1 上安装 Web 服务器 (IIS) 角色  
  
- 2-APP1 上创建一个共享的文件夹 
  
## <a name="bkmk_InstallOS"></a>2-APP1 上安装操作系统  
首先，安装 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>若要在 2 APP1 上安装操作系统  
  
1.  启动 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完全安装） 安装。  
  
2.  按照说明完成安装，指定本地管理员账户的密码（强）。 使用本地管理员账户登录。  
  
3.  2-APP1 连接到具有 Internet 访问权限的网络并运行 Windows 更新，即可安装最新的更新适用于 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，然后从 Internet 断开连接。  
  
4.  连接到 2 Corpnet 子网 2-APP1。  
  
## <a name="bkmk_TCP"></a>配置 TCP/IP 属性  
2-APP1 上配置 TCP/IP 属性。  
  
#### <a name="to-configure-tcpip-properties"></a>配置 TCP/IP 属性的步骤  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**有线以太网连接**，单击的链接。  
  
2.  在中**网络连接**窗口中，右键单击**有线以太网连接**，然后单击**属性**。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  单击 **“使用下面的 IP 地址”** 。 在中**IP 地址**，类型**10.2.0.3**。 在 **“子网掩码”** 框中，键入 **255.255.255.0**。 在中**默认网关**，类型**10.2.0.254**。  
  
5.  单击 **“使用下面的 DNS 服务器地址”** 。 在中**首选 DNS 服务器**，类型**10.2.0.1**。  
  
6.  单击 **“高级”** ，然后单击 **“DNS”** 选项卡。在中**此连接的 DNS 后缀**，类型**corp2.corp.contoso.com**，然后单击**确定**两次。  
  
7.  单击“Internet 协议版本 6 (TCP/IPv6)”  ，然后单击“属性”  。  
  
8.  单击**使用下列 IPv6 地址**。 在中**IPv6 地址**，类型**2001:db8:2::3**。 在中**子网前缀长度**，类型**64**。 在中**默认网关**，类型**2001:db8:2::fe**。 单击**使用以下 DNS 服务器地址**，然后在**首选 DNS 服务器**，类型**2001:db8:2::1**。  
  
9. 单击 **“高级”** ，然后单击 **“DNS”** 选项卡。  
  
10. 在中**此连接的 DNS 后缀**，类型**corp2.corp.contoso.com**，然后单击**确定**两次。  
  
11. 上**有线以太网连接属性**对话框中，单击**关闭**。  
  
12. 关闭 **“网络连接”** 窗口。  
  
## <a name="bkmk_JoinDomain"></a>2-APP1 加入 CORP2 域  
2-APP1 加入 corp2.corp.contoso.com 域。  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>若要加入到 CORP2 域 2-APP1  
  
1.  在服务器管理器控制台中，在**本地服务器**，在**属性**区域中，为下的一步**计算机名称**，单击的链接。  
  
2.  在 **“系统属性”** 对话框上的 **“计算机名称”** 选项卡上，单击 **“更改”** 。  
  
3.  在中**计算机名**，类型**2-APP1**。 在中**的成员**，单击**域**，类型**corp2.corp.contoso.com**，然后单击**确定**。  
  
4.  当系统提示输入用户名和密码的值时，键入**管理员**其和密码，然后单击**确定**。  
  
5.  当看到一个对话框，欢迎您进入 corp2.corp.contoso.com 域时，请单击**确定**。  
  
6.  当系统提示你必须重新启动计算机时，请单击“确定”  。  
  
7.  在“系统属性”  对话框中单击“关闭”  。  
  
8.  当系统提示你重新启动计算机时，请单击“立即重新启动”  。  
  
9. 在计算机重新启动后，单击**切换用户**，然后单击**其他用户**和登录到 CORP2 域管理员帐户。  
  
## <a name="bkmk_IIS"></a>在 2-APP1 上安装 Web 服务器 (IIS) 角色  
安装 Web 服务器 (IIS) 角色，以使 web 服务器的 2-APP1。  
  
#### <a name="to-install-the-web-server-iis-role"></a>若要安装 Web 服务器 (IIS) 角色  
  
1.  在服务器管理器控制台中，在**仪表板**，单击**添加角色和功能**。  
  
2.  单击**下一步**三次以转到服务器角色选择屏幕  
  
3.  上**选择服务器角色**页上，选择**Web 服务器 (IIS)** ，然后单击**下一步**四次。  
  
4.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  
  
5.  验证安装是否成功，然后依次**关闭**。  
  
## <a name="bkmk_Share"></a>2-APP1 上创建一个共享的文件夹  
2-APP1 上创建共享的文件夹和文件夹中的文本文件。  
  
#### <a name="to-create-a-shared-folder"></a>若要创建的共享的文件夹  
  
1.  上**启动**屏幕上，键入**explorer.exe**，然后按 ENTER。  
  
2.  单击**计算机**，然后双击**本地磁盘 （c:）** 。  
  
3.  单击**新文件夹**，类型**文件**，然后按 ENTER。 将保留**本地磁盘**窗口处于打开状态。  
  
4.  上**启动**屏幕上，键入**notepad.exe**，右键单击**记事本**，单击**高级**，然后单击**以运行管理员**。  
  
5.  在中**无标题-记事本**窗口中，键入**这是 2-APP1 上的共享的文件**。  
  
6.  单击**文件**，单击**保存**，单击**计算机**，双击**本地磁盘 （c:）** ，然后双击**文件**文件夹。  
  
7.  在中**文件名**，类型**example.txt**，然后单击**保存**。 关闭记事本。  
  
8.  在中**本地磁盘**窗口中，右键单击**文件**文件夹，指向**与共享**，然后单击**特定人员**。  
  
9. 上**文件共享**对话框中，在下拉列表中，单击**Everyone**，然后单击**添加**。 在中**权限级别**有关**Everyone**，单击**读/写**。  
  
10. 单击**共享**，然后单击**完成**。  
  
11. 关闭**本地磁盘**窗口。  
  



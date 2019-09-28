---
title: 配置 WEB1 以分发证书吊销列表（Crl）
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5d53cbba37699346db110f0748a9c3e0c834c18e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356293"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>配置 WEB1 以分发证书吊销列表（Crl）

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程来配置 web 服务器 WEB1 以分发 Crl。  
  
在根 CA 的扩展中，已声明可通过 https://pki.corp.contoso.com/pki 获取根 CA 的 CRL。 目前，WEB1 上没有 PKI 虚拟目录，所以必须创建一个。  
  
若要执行此过程，您必须是**Domain Admins**的成员。  
  
> [!NOTE]  
> 在下面的过程中，将用户帐户名称、Web 服务器名称、文件夹名称和位置以及适用于你的部署的其他值替换为其他值。  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>配置 WEB1 以分发证书和 Crl  
  
1.  在 WEB1 上，以管理员身份运行 Windows PowerShell，键入 `explorer c:\`，然后按 ENTER。 Windows 资源管理器将打开驱动器 C。   
  
2.  在 C：驱动器上创建一个名为 "PKI" 的新文件夹。 为此，请单击 "**主页**"，然后单击 "**新建文件夹**"。 将创建一个新文件夹，其中突出显示了临时名称。 键入 " **pki** "，然后按 enter。  
  
3.  在 Windows 资源管理器中，右键单击刚创建的文件夹，将鼠标光标悬停在 "**共享**" 上，然后单击 "**特定用户**"。 此时将打开 "**文件共享**" 对话框。  
  
4.  在 "**文件共享**" 中键入 " **Cert 发行者**"，然后单击 "**添加**"。 "证书发布者" 组将添加到列表中。 在列表的 "**权限级别**" 中，单击 "**证书发布者**" 旁边的箭头，然后单击 "**读/写**"。 单击 "**共享**"，然后单击 "**完成**"。  
  
5.  关闭 Windows 资源管理器。  
  
6.  打开 IIS 控制台。 在服务器管理器中，单击“工具”，然后单击“Internet 信息服务 (IIS) 管理器”。  
  
7.  在 "Internet Information Services （IIS）管理器" 控制台树中，展开**WEB1**。 如果你受邀从 Microsoft Web 平台开始，请单击 **“取消”** 。  
  
8.  展开 **“Sites”** ，然后右键单击 **“Default Web Site”** ，并单击 **“添加虚拟目录”** 。  
  
9. 在 "**别名**" 中，键入 " **pki**"。 在 "**物理路径**" 中键入**C:\pki**，然后单击 **"确定"** 。  
  
10. 启用对 pki 虚拟目录的匿名访问，以便任何客户端都可以检查 CA 证书和 Crl 的有效性。 为此，请执行以下操作：  
  
    1.  在 **“连接”** 窗格中，确保选中该 **“pki”** 。  
  
    2.  在 **pki 主页**上，单击 **“身份验证”** 。  
  
    3.  在 **“操作”** 窗格中，单击 **“编辑权限”** 。  
  
    4.  在 **“安全’** 选项卡上，单击 **“编辑”**  
  
    5.  在 **“pki 权限”** 对话框中，单击 **“添加”** 。  
  
    6.  在 "**选择用户、计算机、服务帐户或组**" 中，键入 "**匿名登录";"所有人**"，然后单击 "**检查名称**"。 单击 **“确定”** 。  
  
    7.  在 "**选择用户、计算机、服务帐户或组**" 对话框中单击 **"确定"** 。  
  
    8.  在 " **Pki 权限**" 对话框中单击 **"确定"** 。  
  
11. 在 " **Pki 属性**" 对话框中单击 **"确定"** 。  
  
12. 在 **“pki 主页”** 窗格中，双击 **“请求过滤”** 。  
  
13. 默认情况下，在 **“请求过滤”** 窗格中选择 **“文件扩展名”** 选项卡。 在 **“操作”** 窗格中，单击 **“编辑功能设置”** 。  
  
14. 在 **“编辑请求过滤设置”** 中，选择 **“允许双重转义”** ，然后单击 **“确定”** 。  
  
15. 在 Internet Information Services （IIS）管理器 MMC 中，单击你的 Web 服务器名称。 例如，如果你的 Web 服务器名为 WEB1，则单击**WEB1**。  
  
16. 在 "**操作**" 中，单击 "**重新启动**"。 Internet 服务已停止，然后重新启动。  
  


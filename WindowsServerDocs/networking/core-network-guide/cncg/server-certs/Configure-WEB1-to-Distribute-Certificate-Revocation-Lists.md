---
title: 配置 WEB1 分发证书吊销列表 (Crl)
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839188"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>配置 WEB1 分发证书吊销列表 (Crl)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于配置 WEB1 分发 Crl 的 web 服务器。  
  
在根 CA 的扩展，曾提到，会通过从根 CA CRL https://pki.corp.contoso.com/pki。 目前，没有 PKI 虚拟目录在 WEB1 上，因此必须创建一个。  
  
若要执行此过程，您必须**Domain Admins**。  
  
> [!NOTE]  
> 在下面的过程中，将为与适用于你的部署的用户帐户名称、 Web 服务器名称、 文件夹名称和位置和其他值。  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>若要配置 WEB1 分发证书和 Crl  
  
1.  在 WEB1 上，以管理员身份运行 Windows PowerShell 中，键入`explorer c:\`，然后按 ENTER。 Windows 资源管理器将打开到驱动器 c。   
  
2.  创建新文件夹 c： 驱动器上名为 PKI。 若要执行此操作，请单击**主页**，然后单击**新文件夹**。 突出显示的临时名称创建新文件夹。 类型**pki** ，然后按 ENTER。  
  
3.  在 Windows 资源管理器中，右键单击刚创建的文件夹，将鼠标光标悬停**与共享**，然后单击**特定人员**。 **文件共享**对话框随即打开。  
  
4.  在中**文件共享**，类型**Cert Publishers**，然后单击**添加**。 Cert Publishers 组添加到列表。 在列表中，在**权限级别**，单击旁边的箭头**Cert Publishers**，然后单击**读/写**。 单击**共享**，然后单击**完成**。  
  
5.  关闭 Windows 资源管理器。  
  
6.  打开 IIS 控制台。 在服务器管理器中，单击“工具”，然后单击“Internet 信息服务 (IIS) 管理器”。  
  
7.  在 Internet 信息服务 (IIS) 管理器控制台树中，展开**WEB1**。 如果你受邀从 Microsoft Web 平台开始，请单击 **“取消”**。  
  
8.  展开 **“Sites”**，然后右键单击 **“Default Web Site”**，并单击 **“添加虚拟目录”**。  
  
9. 在中**别名**，类型**pki**。 在中**物理路径**类型**C:\pki**，然后单击**确定**。  
  
10. 启用匿名访问 pki 虚拟目录，以便任何客户端可以检查 CA 证书和 Crl 的有效性。 为此，请执行以下操作：  
  
    1.  在 **“连接”** 窗格中，确保选中该 **“pki”**。  
  
    2.  在 **pki 主页**上，单击 **“身份验证”**。  
  
    3.  在 **“操作”** 窗格中，单击 **“编辑权限”**。  
  
    4.  在 **“安全’** 选项卡上，单击 **“编辑”**  
  
    5.  在 **“pki 权限”** 对话框中，单击 **“添加”**。  
  
    6.  在中**选择用户、 计算机、 服务帐户或组**，类型**ANONYMOUS LOGON;每个人都**，然后单击**检查名称**。 单击 **“确定”**。  
  
    7.  单击**确定**上**选择用户、 计算机、 服务帐户或组**对话框。  
  
    8.  单击**确定**上**pki 权限**对话框。  
  
11. 单击**确定**上**pki 属性**对话框。  
  
12. 在 **“pki 主页”** 窗格中，双击 **“请求过滤”**。  
  
13. 默认情况下，在 **“请求过滤”** 窗格中选择 **“文件扩展名”** 选项卡。 在 **“操作”** 窗格中，单击 **“编辑功能设置”**。  
  
14. 在 **“编辑请求过滤设置”** 中，选择 **“允许双重转义”** ，然后单击 **“确定”**。  
  
15. 在 Internet 信息服务 (IIS) 管理器 MMC 中，单击 Web 服务器名称。 例如，如果你的 Web 服务器名为 WEB1，单击**WEB1**。  
  
16. 在中**操作**，单击**重新启动**。 Internet 服务被停止并重新启动。  
  


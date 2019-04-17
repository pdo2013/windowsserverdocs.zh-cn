---
title: 配置 WEB1 分配证书吊销列表 (Crl)
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>配置 WEB1 分配证书吊销列表 (Crl)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程配置 web 服务器 WEB1 分配 Crl。  
  
在根扩展，曾提到从根 CRL 将可通过http://pki.corp.contoso.com/pki。 当前，PKI 虚拟目录上没有 WEB1，因此必须创建一个。  
  
若要执行此过程，你必须**域管理员**。  
  
> [!NOTE]  
> 在下面的过程，替换用户帐户名、 Web 服务器的名称，文件夹名称和位置和其他值这些适用于你的部署。  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>若要配置 WEB1 分发证书和 Crl  
  
1.  在 WEB1，以 administrator 身份，运行 Windows PowerShell 键入`explorer c:\`，然后按 ENTER。 Windows 资源管理器打开到驱动器 c。   
  
2.  创建命名 PKI 驱动器 c： 打开一个新文件夹。 若要执行此操作，请单击**家庭**，然后单击**新文件夹**。 突出显示的临时名称创建一个新文件夹。 键入**pki** ，然后按 ENTER。  
  
3.  Windows 资源管理器、 右键单击你刚创建的文件夹、 将鼠标指针悬停在**与共享**，然后单击**特定用户**。 **文件共享**对话框中打开。  
  
4.  在**文件共享**，类型**证书发行者**，然后单击**添加**。 证书发行者组添加到列表中。 在列表中，在**权限级别**，单击旁边的箭头**证书发行者**，然后单击**读取/写入**。 单击**共享**，然后单击**完成**。  
  
5.  关闭 Windows 资源管理器。  
  
6.  打开 IIS 主机。 在服务器管理器中，单击**工具**，然后单击**Internet 信息服务 (IIS) 管理器**。  
  
7.  在 Internet 信息服务 (IIS) 管理控制台树中，展开**WEB1**。 如果您受邀 Microsoft Web 平台入门，请单击**取消**。  
  
8.  展开**站点**，然后右键单击**默认网站**，然后单击**添加虚拟目录**。  
  
9. 在**别名**，类型**pki**。 在**物理路径**类型**C:\pki**，然后单击**确定**。  
  
10. 启用匿名访问 pki 虚拟目录，以便任何客户端可以检查 CA 证书和 Crl 的有效性。 若要执行此操作：  
  
    1.  在**连接**窗格中，确保**pki**选择。  
  
    2.  在**pki 家庭**单击**身份验证**。  
  
    3.  在**操作**窗格中，单击**编辑权限**。  
  
    4.  在**安全**选项卡上，单击**编辑**  
  
    5.  在**pki 权限**对话框中，单击**添加**。  
  
    6.  在**选择用户、 计算机、 服务帐户、 或组**，类型**匿名登录;每个人都**，然后单击**检查名称**。 单击**确定**。  
  
    7.  单击**确定**上**选择用户、 计算机、 服务帐户或组**对话框。  
  
    8.  单击**确定**上**pki 权限**对话框。  
  
11. 单击**确定**上**pki 属性**对话框。  
  
12. 在**pki 家庭**窗格中，双击**请求筛选**。  
  
13. **文件扩展名**默认情况下在选择选项卡**请求筛选**窗格。 在**操作**窗格中，单击**编辑功能设置**。  
  
14. 在**编辑请求筛选设置**、 选择**允许双转义**，然后单击**确定**。  
  
15. 在 MMC Internet 信息服务 (IIS) 管理器中，单击你的 Web 服务器名称。 例如，如果你的 Web 服务器 WEB1 命名，请单击**WEB1**。  
  
16. 在**操作**，单击**重启**。 Internet 服务会停止，然后重新启动。  
  


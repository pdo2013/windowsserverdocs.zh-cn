---
title: 步骤5配置 DC1
description: 本主题是测试实验室指南-演示适用于 Windows Server 2016 的 DirectAccess 多站点部署的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa251ccc0cc48e3805667a247047711c2ae4fcf6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388300"
---
# <a name="step-5-configure-dc1"></a>步骤5配置 DC1

>适用于：Windows Server（半年频道）、Windows Server 2016

DC1 充当 corp.contoso.com 域的域控制器、DNS 服务器和 DHCP 服务器。  
  
若要将远程访问配置为使用多站点拓扑，需要为第二个域控制器 2-DC1 添加其他 Active Directory 域服务（AD DS）站点，并配置子网之间的路由。  
  
1. 在域控制器上配置默认网关。 在 DC1 上配置默认网关。  
  
2. 在 DC1 上创建适用于 Windows 7 DirectAccess 客户端的安全组。 配置 DirectAccess 时，它会自动创建应用于 DirectAccess 客户端和服务器组策略对象（Gpo）和 GPO 设置。 DirectAccess 客户端 GPO 应用于特定 Active Directory 安全组。  
  
3. 添加新的 AD DS 站点。 创建第二个 AD DS 站点。  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>在域控制器上配置默认网关  
  
1.  在服务器管理器控制台中，单击 "**本地服务器**"，然后在 "**有线以太网连接**" 旁边的 "**属性**" 区域中，单击链接。  
  
2.  在 "网络连接" 窗口中，右键单击 "**有线以太网连接**"，然后单击 "**属性**"。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  在 "**默认网关**" 中，键入**10.0.0.254**，在 "**备用 DNS 服务器**" 中键入**10.2.0.1**，然后单击 **"确定"** 。  
  
5.  单击“Internet 协议版本 6 (TCP/IPv6)”，然后单击“属性”。  
  
6.  在 "**默认网关**" 中，键入**2001： db8：1：： Fe**，在**备用 DNS 服务器**中，键入 " **2001： db8：2：： 1**"，然后单击 **"确定"** 。  
  
7.  在 "**有线以太网连接属性**" 对话框中，单击 "**关闭**"。  
  
8.  关闭 **“网络连接”** 窗口。  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>在 DC1 上为 Windows 7 DirectAccess 客户端创建安全组  
通过以下过程创建适用于 Windows 7 的 DirectAccess 安全组。  
  
 Windows 7 客户端计算机必须是单独的安全组的成员，因为它们只能通过单个入口点连接到内部资源。 启用多站点支持或添加入口点时，如果请求了 Windows 7 支持，则 DirectAccess 会自动为每个入口点的 Windows 7 客户端创建一个单独的 GPO。  
  
### <a name="create-security-groups"></a>创建安全组  
  
1.  在 "**开始**" 屏幕上，键入**dsa.msc**，然后按 enter。  
  
2.  在左侧窗格中，展开 " **corp.contoso.com**"，单击 "**用户**"，右键单击 "**用户**"，指向 "**新建**"，然后单击 "**组**"。  
  
3.  在 "**新建对象-组**" 对话框中的 "**组名**" 下，输入**Win7_Clients_Site1**。  
  
4.  在“组范围”下单击“全局”，在“组类型”下单击“安全”，然后单击“确定”。  
  
5.  双击 " **Win7_Clients_Site1** " 安全组，然后在 " **Win7_Clients_Site1 属性**" 对话框中，单击 "**成员**" 选项卡。  
  
6.  在“成员” 选项卡上，单击“添加”。  
  
7.  在 "**选择用户、联系人、计算机或服务帐户**" 对话框中，单击 "**对象类型**"。 在 "**对象类型**" 对话框中，选择 "**计算机**"，然后单击 **"确定"** 。  
  
8.  在 "**输入要选择的对象名称**" 中，键入**client2**，然后单击 **"确定**"，然后在 " **Win7_Clients_Site1 属性**" 对话框中单击 **"确定"** 。  
  
9. 在**Active Directory 用户和计算机**"控制台的左窗格中，右键单击"**用户**"，指向"**新建**"，然后单击"**组**"。  
  
10. 在 "**新建对象-组**" 对话框中的 "**组名**" 下，输入**Win7_Clients_Site2**。  
  
11. 在“组范围”下单击“全局”，在“组类型”下单击“安全”，然后单击“确定”。  
  
12. 关闭 **“Active Directory 用户和计算机”** 控制台。  
  
## <a name="to-add-a-new-ad-ds-site"></a>添加新的 AD DS 站点  
  
1.  在 "**开始**" 屏幕上，键入 "**dssite.msc**"，然后按 enter。  
  
2.  在 "Active Directory 站点和服务" 控制台的控制台树中，右键单击 "**站点**"，然后单击 "**新建站点**"。  
  
3.  在 "**新建对象-站点**" 对话框的 "**名称**" 框中，键入 " **Second**"。  
  
4.  在列表框中，单击 " **DEFAULTIPSITELINK**"，然后单击 **"确定"** 两次。  
  
5.  在控制台树中，展开 "**站点**"，右键单击 "**子网**"，然后单击 "**新建子网**"。  
  
6.  在 "**新建对象-子网**" 对话框中的 "**前缀**" 下，键入**10.0.0.0/24**，在 "**为此前缀选择一个站点对象**" 列表中，单击 "**默认-第一个站点名称**"，然后单击 **"确定"** 。  
  
7.  在控制台树中，右键单击 "**子网**"，然后单击 "**新建子网**"。  
  
8.  在 "**新建对象-子网**" 对话框中的 "**前缀**" 下，键入 " **2001：%：/64**"，在 "**为此前缀选择一个站点对象**" 列表中，单击 "默认" "**第一个站点-名称**"，然后单击 **"确定"** 。  
  
9. 在控制台树中，右键单击 "**子网**"，然后单击 "**新建子网**"。  
  
10. 在 "**新建对象-子网**" 对话框中的 "**前缀**" 下，键入**10.2.0.0/24**，在 "**为此前缀选择一个站点对象**" 列表中，单击 "**辅助站点**"，然后单击 **"确定"** 。  
  
11. 在控制台树中，右键单击 "**子网**"，然后单击 "**新建子网**"。  
  
12. 在 "**新建对象-子网**" 对话框中的 "**前缀**" 下，键入 " **2001：%：/64**"，在 "**为此前缀选择一个站点对象**" 列表中，单击 "**第二个站点**"，然后单击 **"确定"** 。  
  
13. 关闭 Active Directory 站点和服务。  
  



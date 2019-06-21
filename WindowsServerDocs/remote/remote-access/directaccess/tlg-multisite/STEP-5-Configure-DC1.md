---
title: 步骤 5 配置 DC1
description: 本主题是测试实验室指南的一部分-演示 DirectAccess 多站点部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 70357156-fcb0-4346-a61e-4ea963e3ffb0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108e517923c75f685d817cdf9fad9b14132e3bb0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281433"
---
# <a name="step-5-configure-dc1"></a>步骤 5 配置 DC1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

DC1 将充当域控制器、 DNS 服务器和为 corp.contoso.com 域的 DHCP 服务器。  
  
若要配置远程访问权限以使用多站点拓扑，有必要添加第二个域控制器 2-DC1，其他 Active Directory 域服务 (AD DS) 站点和子网之间路由的配置。  
  
1. 若要在域控制器上配置默认网关。 在 DC1 上配置默认网关。  
  
2. 对于 Windows 7 DirectAccess 客户端在 DC1 上创建安全组。 配置 DirectAccess 后，它会自动创建组策略对象 (Gpo) 和 GPO 设置应用于 DirectAccess 客户端和服务器。 DirectAccess 客户端 GPO 应用于特定的 Active Directory 安全组。  
  
3. 若要添加新的 AD DS 站点。 创建第二个 AD DS 站点。  
  
## <a name="to-configure-the-default-gateway-on-the-domain-controller"></a>若要在域控制器上配置默认网关  
  
1.  在服务器管理器控制台中，单击**本地服务器**，然后在**属性**区域中，为下的一步**有线以太网连接**，单击的链接。  
  
2.  在网络连接窗口中，右键单击**有线以太网连接**，然后单击**属性**。  
  
3.  单击 **“Internet 协议版本 4 (TCP/IPv4)”** ，然后单击 **“属性”** 。  
  
4.  在中**默认网关**，类型**10.0.0.254**，然后在**备用 DNS 服务器**，类型**10.2.0.1**，然后单击**确定**.  
  
5.  单击“Internet 协议版本 6 (TCP/IPv6)”  ，然后单击“属性”  。  
  
6.  在中**默认网关**，类型**2001:db8:1::fe**，然后在**备用 DNS 服务器**，类型**2001:db8:2::1**，然后单击**确定**。  
  
7.  上**有线以太网连接属性**对话框中，单击**关闭**。  
  
8.  关闭 **“网络连接”** 窗口。  
  
## <a name="create-security-groups-for-windows-7-directaccess-clients-on-dc1"></a>对于 Windows 7 DirectAccess 客户端在 DC1 上创建安全组  
使用以下过程创建 Windows 7 DirectAccess 安全组。  
  
 Windows 7 客户端计算机必须是单独的安全组的成员，因为它们能够连接到内部资源通过单一入口点。 当启用多站点支持或添加入口点，如果 Windows 7 支持请求时，然后一个单独的 GPO 将自动创建每个入口点的 Windows 7 的 DirectAccess 客户端。  
  
### <a name="create-security-groups"></a>创建安全组  
  
1.  上**启动**屏幕上，键入**dsa.msc**，然后按 ENTER。  
  
2.  在左窗格中，展开**corp.contoso.com**，单击**用户**，然后右键单击**用户**，指向**新建**，然后单击**组**。  
  
3.  上**新建对象-组**对话框中的**组名称**，输入**Win7_Clients_Site1**。  
  
4.  在  “组范围”下单击  “全局”，在  “组类型”下单击“安全”  ，然后单击“确定”  。  
  
5.  双击**Win7_Clients_Site1**安全组，然后在**Win7_Clients_Site1 属性**对话框中，单击**成员**选项卡。  
  
6.  在“成员”  选项卡上，单击“添加”  。  
  
7.  上**选择用户、 联系人、 计算机或服务帐户**对话框中，单击**对象类型**。 上**对象类型**对话框中，选择**计算机**，然后单击**确定**。  
  
8.  在中**输入要选择的对象名称**，类型**client2**，然后单击**确定**，然后在**Win7_Clients_Site1 属性**对话框中，单击**确定**。  
  
9. 在中**Active Directory 用户和计算机**控制台中的，在左窗格中，右键单击**用户**，指向**新建**，然后单击**组**.  
  
10. 上**新建对象-组**对话框中的**组名称**，输入**Win7_Clients_Site2**。  
  
11. 在  “组范围”下单击  “全局”，在  “组类型”下单击“安全”  ，然后单击“确定”  。  
  
12. 关闭 **“Active Directory 用户和计算机”** 控制台。  
  
## <a name="to-add-a-new-ad-ds-site"></a>若要添加新的 AD DS 站点  
  
1.  上**启动**屏幕上，键入**dssite.msc**，然后按 ENTER。  
  
2.  在 Active Directory 站点和服务控制台中，在控制台树中，右键单击**站点**，然后单击**新站点**。  
  
3.  上**新建对象-站点**对话框中**名称**框中，键入**第二个站点**。  
  
4.  在列表框中，单击**DEFAULTIPSITELINK**，然后单击**确定**两次。  
  
5.  在控制台树中，展开**站点**，右键单击**子网**，然后单击**新子网**。  
  
6.  上**新建对象-子网**对话框中的**前缀**，类型**10.0.0.0/24**，在**选择站点对象为此前缀**列表中，单击**默认第一个站点名称**，然后单击**确定**。  
  
7.  在控制台树中，右键单击**子网**，然后单击**新的子网**。  
  
8.  上**新建对象-子网**对话框中的**前缀**，类型**2001:db8:1:: / 64**，在**选择站点对象为此前缀**列表中，单击**默认第一个站点名称**，然后单击**确定**。  
  
9. 在控制台树中，右键单击**子网**，然后单击**新的子网**。  
  
10. 上**新建对象-子网**对话框中的**前缀**，类型**10.2.0.0/24**，在**选择站点对象为此前缀**列表中，单击**第二个站点**，然后单击**确定**。  
  
11. 在控制台树中，右键单击**子网**，然后单击**新的子网**。  
  
12. 上**新建对象-子网**对话框中的**前缀**，类型**2001:db8:2:: / 64**，在**选择站点对象为此前缀**列表中，单击**第二个站点**，然后单击**确定**。  
  
13. 关闭 Active Directory 站点和服务。  
  



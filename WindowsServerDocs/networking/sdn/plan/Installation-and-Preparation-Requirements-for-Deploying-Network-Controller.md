---
title: 安装和部署网络控制器的准备工作要求
description: 你可以使用本主题准备您的数据中心网络控制器部署。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>安装和部署网络控制器的准备工作要求

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题准备您的数据中心网络控制器部署。  
  
> [!NOTE]  
> 本主题中，除了以下网络控制器文档才可用。  
> 
> - [网络控制器](../technologies/network-controller/Network-Controller.md)
> - [网络控制器高可用性](../technologies/network-controller/network-controller-high-availability.md)
> - [部署使用 Windows PowerShell 网络控制器](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安装网络控制器服务器角色使用服务器管理器](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

以下是安装、 软件和其他要求和准备必须部署网络控制器之前采取的步骤。

## <a name="installation-requirements"></a>安装要求

以下是网络控制器安装要求。

- 对于 Windows Server 2016 部署，你可以将网络控制器部署一个或多个计算机、 一个或多个虚拟机的功能，或计算机和虚拟机的功能的组合。 所有虚拟机的功能和作为网络控制器节点计划的计算机必须运行 Windows Server 2016 Datacenter edition。

## <a name="software-requirements"></a>软件要求

网络控制器部署需要一个或多个计算机或将其作为网络控制器和一台计算机虚拟机的功能或 VM 管理客户端网络控制器身份提供服务。 这些计算机或 Vm 必须运行以下操作系统。  

- 任何计算机或在其你安装了网络控制器的虚拟机 (VM) 必须运行 Windows Server 2016 的 Datacenter edition。  
  
- 管理客户端计算机或 VM 网络控制器必须运行 Windows 8、 Windows 8.1 或 Windows 10 中。  
  
## <a name="additional-requirements"></a>其他要求

以下是其他必须部署网络控制器之前采取的步骤。
  
### <a name="configure-security-groups"></a>配置安全组
  
如果计算机或网络控制器和管理客户端虚拟机的功能已加入域，配置 Kerberos 身份验证的以下安全组。

- 创建一个安全组并添加的所有用户有权配置了网络控制器。 例如，创建一个名为组**网络控制器管理员**。 你添加到此组的用户的所有还必须成员**域用户**组中的 Active Directory 用户和计算机。  
  
    > [!NOTE]  
    > 在 Active Directory 用户和计算机创建一组的详细信息，请参阅[创建一组新](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx)。  

- 创建一个安全组并添加的所有用户有权配置和管理网络通过网络控制器。  例如，创建一个名为新组**网络控制器用户**。 你将添加到新组的用户的所有还必须的成员**域用户**组中的 Active Directory 用户和计算机。 使用具像状态传输 \(REST\) 执行的所有网络控制器配置和管理。

### <a name="configure-log-file-locations-if-needed"></a>如果需要，配置日志文件的位置

你可以将网络控制器调试日志存储网络控制器计算机或 VM 中，或远程文件共享。 如果你想要在远程文件共享存储日志，确保共享的网络控制器可访问。

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>配置动态 DNS 注册网络控制器
  
你可以将部署网络控制器相同子网或不同的子网上的群集节点。 

>[!NOTE]
>如果网络控制器节点是相同子网，必须提供网络控制器其余 IP 地址，动态 DNS 注册配置网络控制器时。 如果在不同的子网上节点，必须提供网络控制器其余 DNS 名称，动态 DNS 注册配置时。

如果网络控制器节点在不同的子网上，你必须执行以下额外 DNS 配置：

- 部署过程中创建网络控制器 DNS 名称

- 配置 DNS 服务器上的动态更新 DNS 网络控制器 DNS 名称

- 将 DNS 的动态更新限制为仅网络控制器节点

若要配置 DNS 的动态更新，并限制网络控制器名称记录的动态更新，你可以使用下面的过程。

> [!NOTE]
> 在会员**域管理员**，或等效的最低要求执行这些过程。
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>若要允许 DNS 为某个区域的动态更新

1. 打开 DNS 管理器。

2. 控制台树中，右键单击相应的区域，然后单击**属性**。 该区域**属性**对话框中打开。

3. 在**常规**选项卡上，验证区域类型是否**主要**或**Active Directory 集成**。

4. 在**的动态更新**，确认**仅安全**选择。 如果未选中，更改的值**的动态更新**到**仅安全**，然后单击**确定**。

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>若要配置 DNS 区域安全权限的网络控制器节点

1.  打开 DNS 管理器。

2.  控制台树中，右键单击相应的区域，然后单击**属性**。 该区域**属性**对话框中打开。

3.  单击**安全**选项卡，然后单击**高级**。 **高级安全设置**对话框中打开。

4. 在**高级安全设置**，单击**添加**。 **权限项**对话框中打开。
  
5. 单击**选择主体**。 **选择用户、 计算机、 服务帐户或组**对话框中打开。

6. 在**选择用户、 计算机、 服务帐户或组**对话框中，单击**对象类型**。 **对象类型**对话框中打开。 

7. 在**对象类型**，选择**计算机**，然后单击**确定**。

8. 在**选择用户、 计算机、 服务帐户或组**对话框中，键入你的部署中的某个网络控制器节点 NetBIOS 名称，然后单击**确定**。

9. 在**权限项**，确保的值**类型**是**允许**的值和**适用于**是**此对象及所有后代**。
  
10. 在权限中，选择**写入所有属性**和**删除**，然后单击**确定**。

11. 重复步骤**5**通过**10**的所有计算机和在网络控制器群集虚拟机的功能。

有关详细信息，请参阅[计划软件定义网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure)。

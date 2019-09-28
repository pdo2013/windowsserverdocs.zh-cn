---
title: 部署网络控制器的要求
description: 为网络控制器部署准备你的数据中心，这需要一个或多个计算机或 vm 以及一个计算机或 VM。 部署网络控制器之前，必须配置安全组、日志文件位置（如果需要）和动态 DNS 注册。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 38d104bc3ceca478f0e261b3a364b5d4448b22f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406017"
---
# <a name="requirements-for-deploying-network-controller"></a>部署网络控制器的要求

>适用于：Windows Server（半年频道）、Windows Server 2016

为网络控制器部署准备你的数据中心，这需要一个或多个计算机或 vm 以及一个计算机或 VM。 部署网络控制器之前，必须配置安全组、日志文件位置（如果需要）和动态 DNS 注册。


## <a name="network-controller-requirements"></a>网络控制器要求

网络控制器部署需要一个或多个用作网络控制器的计算机或 vm，另一个计算机或 VM 充当网络控制器的管理客户端。 

- 规划为网络控制器节点的所有 Vm 和计算机都必须运行 Windows Server 2016 Datacenter edition。 
- 安装网络控制器的任何计算机或虚拟机（VM）必须运行 Windows Server 2016 Datacenter 版本。 
- 网络控制器的管理客户端计算机或 VM 必须运行 Windows 10。 


## <a name="configuration-requirements"></a>配置需求

部署网络控制器之前，必须配置安全组、日志文件位置（如果需要）和动态 DNS 注册。

### <a name="step-1-configure-your-security-groups"></a>步骤 1： 配置安全组

你要做的第一件事是为 Kerberos 身份验证创建两个安全组。 

为有权执行以下操作的用户创建组： 

1. 配置网络控制器<p>例如，你可以为此组网络控制器管理员命名。 
2.  使用网络控制器配置和管理网络<p>例如，你可以为此组网络控制器用户命名。 使用具象状态传输（REST）来配置和管理网络控制器。

>[!NOTE]
>你添加的所有用户都必须是 "域用户" 组的成员，Active Directory 用户和计算机 "。

### <a name="step-2-configure-log-file-locations-if-needed"></a>步骤 2： 根据需要配置日志文件位置

接下来要做的就是将文件位置配置为在网络控制器计算机或 VM 上或远程文件共享上存储网络控制器调试日志。 

>[!NOTE]
>如果将日志存储在远程文件共享中，请确保可以从网络控制器访问该共享。


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>步骤 3： 为网络控制器配置动态 DNS 注册

最后，您要做的下一件事就是在同一子网或不同子网中部署网络控制器群集节点。 


|         如果...         |                                                                                                                                                         则...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  在同一子网中，  |                                                                                                                                必须提供网络控制器 REST IP 地址。                                                                                                                                 |
| 在不同的子网中， | 你必须提供在部署过程中创建的网络控制器 REST DNS 名称。 还必须执行以下操作：<ul><li>为 DNS 服务器上的网络控制器 DNS 名称配置 DNS 动态更新。</li><li>仅将 DNS 动态更新限制为网络控制器节点。</li></ul> |

---

> [!NOTE]
> **Domain Admins**中的成员身份或同等身份是执行这些过程所需的最低要求。

1. 允许对区域进行 DNS 动态更新。

   a. 打开 DNS 管理器，然后在控制台树中右键单击相应的区域，然后单击 "**属性**"。 

   b. 在 "**常规**" 选项卡上，验证区域类型为 "**主要**" 或 " **Active Directory 集成**"。

   c. 在 "**动态更新**" 中，验证是否选择了 "**仅安全**"，然后单击 **"确定"** 。

2. 为网络控制器节点配置 DNS 区域安全权限

   a.  单击“安全”选项卡，然后单击“高级”。 

   b. 在 "**高级安全设置**" 中，单击 "**添加**"。 

   c. 单击**选择主体**。 

   d. 在 "**选择用户、计算机、服务帐户或组**" 对话框中，单击 "**对象类型**"。 

   e. 在 "**对象类型**" 中，选择 "**计算机**"，然后单击 **"确定"** 。

   f. 在 "**选择用户、计算机、服务帐户或组**" 对话框中，键入部署中其中一个网络控制器节点的 NetBIOS 名称，然后单击 **"确定"** 。

   g. 在 "**权限条目**" 中，验证以下值：

      - **Type** = Allow
      - **适用**于 = 此对象和所有后代对象

   h. 在 "**权限**" 中，选择 "**写入所有属性**" 和 "**删除**"，然后单击 **"确定"** 。

3. 对网络控制器群集中的所有计算机和 Vm 重复此操作。

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>步骤 4： 如果使用基于 Kerberos 的身份验证，请配置服务主体名称

如果网络控制器使用基于 Kerberos 的身份验证来与管理客户端通信，则必须在 Active Directory 中为网络控制器配置服务主体名称（SPN）。 网络控制器会自动配置 SPN。 只需为网络控制器计算机提供注册和修改 SPN 的权限即可。 有关更多详细信息，请参阅[配置服务主体名称（SPN）](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn)。

## <a name="deployment-options"></a>部署选项

### <a name="network-controller-deployment"></a>网络控制器部署

使用在虚拟机上配置的三个网络控制器节点，安装程序具有高可用性。 还显示了两个租户，租户2的虚拟网络分为两个虚拟子网，用于模拟 web 层和数据库层。  

![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>网络控制器和软件负载平衡器部署

为实现高可用性，有两个或多个 SLB/MUX 节点。

![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>网络控制器、软件负载平衡器和 RAS 网关部署

有三个网关虚拟机;两个处于活动状态，一个是冗余的。

![SDN NC 计划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  



对于基于 TP5 的部署自动化，Active Directory 必须可用并可从这些子网访问。 有关 Active Directory 的详细信息，请参阅[Active Directory 域服务概述](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)。  

>[!IMPORTANT] 
>如果使用 VMM 进行部署，请确保不会在关系图中显示的四个主机上承载基础结构虚拟机（VMM 服务器、AD/DNS、SQL Server 等）。  


## <a name="next-steps"></a>后续步骤
[规划软件定义的网络基础结构](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure)。

## <a name="related-topics"></a>相关主题
- [网络控制器](../technologies/network-controller/Network-Controller.md) 
- [网络控制器高可用性](../technologies/network-controller/network-controller-high-availability.md) 
- [使用 Windows PowerShell 部署网络控制器](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [使用服务器管理器安装网络控制器服务器角色](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   

---
title: 部署网络控制器的要求
description: 为网络控制器部署，需要一个或多个计算机或 Vm 和一台计算机或 VM 准备你的数据中心。 部署网络控制器之前，必须配置安全组、 日志文件位置 （如果需要） 和动态 DNS 注册。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 9db7609f6f1273c46cba1dd29f81c297bb26f94b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829858"
---
# <a name="requirements-for-deploying-network-controller"></a>部署网络控制器的要求

>适用于：Windows 服务器 （半年频道），Windows Server 2016

为网络控制器部署，需要一个或多个计算机或 Vm 和一台计算机或 VM 准备你的数据中心。 部署网络控制器之前，必须配置安全组、 日志文件位置 （如果需要） 和动态 DNS 注册。


## <a name="network-controller-requirements"></a>网络控制器要求

网络控制器部署需要一个或多个计算机或 Vm 用作网络控制器和一台计算机或 VM 以用作网络控制器管理客户端。 

- 所有 Vm 和计算机在网络控制器节点必须都运行 Windows Server 2016 Datacenter edition。 
- 任何计算机或虚拟机 (VM) 在其安装网络控制器必须运行 Windows Server 2016 Datacenter edition。 
- 管理客户端计算机或为网络控制器的虚拟机必须运行 Windows 10。 

  
## <a name="configuration-requirements"></a>配置需求

部署网络控制器之前, 必须配置安全组、 日志文件位置 （如果需要） 和动态 DNS 注册。
  
### <a name="step-1-configure-your-security-groups"></a>步骤 1： 配置安全组
  
想要执行的第一件事是创建两个安全组的 Kerberos 身份验证。 

创建具有权限的用户的组为： 

1. 配置网络控制器<p>例如，您可以命名网络控制器管理员，此组。 
2.  配置和使用网络控制器管理网络<p>可以命名此组网络控制器用户，例如。 使用具象状态传输 (REST) 来配置和管理网络控制器。

>[!NOTE]
>所有你添加的用户必须是 Active Directory 用户和计算机中的域用户组的成员。

### <a name="step-2-configure-log-file-locations-if-needed"></a>步骤 2： 如果需要，配置日志文件位置

想要执行的下一件事是配置网络控制器计算机或 VM 上或远程文件共享上的存储网络控制器的调试日志文件位置。 

>[!NOTE]
>如果在远程文件共享中存储日志，确保可从网络控制器访问共享。


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>步骤 3： 为网络控制器配置动态 DNS 注册
  
最后，想要执行的下一步就是部署网络控制器上的同一子网或不同的子网的群集节点。 

|如果...  |则...  |
|---------|---------|
|在同一子网上， |必须提供网络控制器 REST IP 地址。 |
|在不同子网上， |必须提供在部署过程中创建的网络控制器 REST DNS 名称。 您必须执行以下操作：<ul><li>DNS 服务器上配置网络控制器 DNS 名称的 DNS 动态更新。</li><li>将 DNS 动态更新限制为仅限网络控制器节点。</li></ul> |
---

> [!NOTE]
> 中的成员身份**Domain Admins**，或等效身份是执行这些过程所需的最小。
  
1. 允许区域的 DNS 动态更新。

   a. 打开 DNS 管理器，并在控制台树中，右键单击适用区域，然后单击**属性**。 
      
   b. 上**常规**选项卡上，验证区域类型是否**主**或**Active Directory 集成**。

   c. 在中**动态更新**，确认**仅限 Secure**已选择，然后单击**确定**。

2. 配置网络控制器节点的 DNS 区域安全权限

   a.  单击“安全”选项卡，然后单击“高级”。 

   b. 在中**高级安全设置**，单击**添加**。 
  
   c. 单击**选择主体**。 

   d. 在中**选择用户、 计算机、 服务帐户或组**对话框中，单击**对象类型**。 

   e. 在中**对象类型**，选择**计算机**，然后单击**确定**。

   f. 在中**选择用户、 计算机、 服务帐户或组**对话框中，键入你的部署中的一个网络控制器节点的 NetBIOS 名称，然后单击**确定**。

   g. 在中**权限条目**，验证以下值：

      - **类型**= 允许
      - **适用于**= 此对象及全部后代
  
   h. 在中**权限**，选择**写入所有属性**并**删除**，然后单击**确定**。

3. 网络控制器群集中重复的所有计算机和虚拟机。

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>步骤 4： 如果使用 Kerberos 身份验证，配置服务主体名称

如果网络控制器使用基于 Kerberos 的身份验证与管理客户端进行通信，你必须在 Active Directory 中的网络控制器的配置服务主体名称 (SPN)。 网络控制器会自动配置 SPN。 您需要做是提供注册和修改 SPN 的网络控制器计算机的权限。 有关更多详细信息，请参阅[配置服务主体名称 (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn)。

## <a name="deployment-options"></a>部署选项

### <a name="network-controller-deployment"></a>网络控制器部署

安装程序包含三个虚拟机上配置的网络控制器节点高度可用。 此外显示的是两个租户和租户 2 的分解为两个虚拟子网的虚拟网络，用于模拟 web 层和数据库层。  

![SDN NC 规划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>网络控制器和软件负载平衡器部署

以实现高可用性，有两个或多个 SLB/MUX 节点。
   
![SDN NC 规划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>网络控制器、 软件负载平衡器和 RAS 网关部署

有三个网关虚拟机;两个处于活动状态，而且其中一个是冗余。

![SDN NC 规划](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
对于基于 TP5 中的部署自动化，Active Directory 必须可用且可从这些子网访问。 有关 Active Directory 的详细信息，请参阅[Active Directory 域服务概述](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)。  
  
>[!IMPORTANT] 
>如果使用 VMM 部署，请确保你的基础结构虚拟机 (VMM 服务器，AD/DNS，SQL Server 等) 未托管在任何关系图所示的四个主机上。  


## <a name="next-steps"></a>后续步骤
[软件定义网络基础结构计划](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure)。

## <a name="related-topics"></a>相关主题
- [网络控制器](../technologies/network-controller/Network-Controller.md) 
- [网络控制器的高可用性](../technologies/network-controller/network-controller-high-availability.md) 
- [部署网络控制器使用 Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [安装网络控制器服务器角色使用服务器管理器](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   

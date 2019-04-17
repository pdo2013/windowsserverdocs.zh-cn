---
title: 部署软件定义的网络基础结构使用脚本
description: 本主题介绍如何部署 Windows Server 2016 中使用脚本 Microsoft 软件定义网络 (SDN) 基础结构。
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4428ad73ab8933510d5a759ec4fa7377ea222ebd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>部署软件定义的网络基础结构使用脚本

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍如何部署使用脚本 Microsoft 软件定义网络 (SDN) 基础结构。 基础结构包含高可用（高可用性）网络控制器、HA 软件负载平衡 (SLB) / 输入混合，虚拟网络，并且关联访问控制列表 (Acl)。 此外，另一个脚本部署供你验证你的 SDN 基础结构租户工作负载。  
  
如果你希望你租户工作负载通信其虚拟网络之外时，你可以设置 SLB NAT 规则、站点的关隧道，或层 3 转发路由之间虚拟和物理工作负载。  
  
你还可以部署使用虚拟机管理器 (VMM) SDN 基础结构。 有关详细信息，请参阅[设置 VMM 结构中软件定义网络 (SDN) 基础结构](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。  
  
## <a name="pre-deployment"></a>预先部署  
  
> [!IMPORTANT]  
> 开始部署之前，你必须计划，并配置您的主机和物理网络基础结构。 有关详细信息，请参阅[计划软件定义网络基础结构](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。  
  
所有 Hyper-V 主机必须都具有安装了 Windows Server 2016。  
  
## <a name="deployment-steps"></a>部署步骤  
配置 Hyper-V 主机（物理服务器）Hyper-V 虚拟交换机用来和 IP 地址分配儿开始。 可以使用任何使用 Hyper-V，共享或当地兼容的存储类型。  
### <a name="install-host-networking"></a>安装网络主机  
1. 安装最新的网络驱动程序为 NIC 硬件。  
2. 安装的所有主机上的 Hyper-V 角色 (有关详细信息，请参阅[入门 Hyper-V 在 Windows Server 2016](https://technet.microsoft.com/en-us/library/mt126159.aspx)。   
  
   从提升了权限的 Windows PowerShellcommand 提示：  
   ``Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart``  
    
    。 创建 Hyper-V 虚拟交换机（使用相同的所有主机切换名称。 例如：**sdnSwitch**)。 配置至少一个网络适配器，或如果使用切换 Embedded 团队合作，配置在至少两个网络适配器。 使用两个 Nic 时，会出现最大的入站传播。  
 `` New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True``  
 
 >[!NOTE] 
 >如果你拥有单独管理 Nic，你可以跳过步骤 3 和 4。

3. 计划主题，请参考 ([计划软件定义网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md))，可以使用你的网络管理员联系以获得管理 VLAN VLAN ID。 连接到管理 VLAN 的新创建虚拟交换机用来管理 vNIC。 如果您的环境中不会使用 VLAN 标记，可以忽略此步骤。  
 `` Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True``  
 
4. 计划主题，请参考 ([计划软件定义网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 和与你的网络管理员，要使用 DHCP 既或静态 IP 分配，若要指定 IP 地址的新创建 vSwitch 管理 vNIC 到工作。 下面的示例显示了如何创建的静态 IP 地址和为其指定到该 vSwitch 的管理 vNIC:  
 ``New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>``  
      
5. [可选]部署到主机 Active Directory 域服务虚拟机 ([安装 Active Directory 域服务 (级别 100)](https://technet.microsoft.com/library/hh472162.aspx)和 DNS 服务器。  
   
    。 连接的 Active Directory / DNS 服务器虚拟机到管理 VLAN:
    
            Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
   
   b。 安装 Active Directory 域服务和 DNS。  
      >[!NOTE]
      >网络控制器身份验证的支持 Kerberos 和 X.509 证书。 本指南（尽管只有一个是需要），针对不同的用途使用这两种身份验证机制。  
        
6. 加入的域的所有 Hyper-V 主机。 确保已 IP 地址分配给可以解决域名 DNS 服务器管理网络点到该网络适配器 DNS 服务器条目。 例如：

        Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   
   。 右键单击**开始**，单击**系统**，然后单击**更改设置**。  
   b。 单击**更改**。  
   c。 单击**域**和指定的域名。  
   d。 单击**确定**。  
   e。 键入出现提示时的的用户名和密码凭据。  
   f。 重新启动的服务器。  
  
### <a name="validation"></a>验证  
使用以下步骤来验证联网该主机正确设置了。  
1. 确保成功创建 VM 切换：  
      
    ``Get-VMSwitch "<switch name>"``  
2. 验证，在 VM 切换管理 vNIC 已连接到管理 VLAN:  
    >[!NOTE]
    >仅当您管理和租户交通共享相同 NIC 相关    
      
    ``Get-VMNetworkAdapterIsolation -ManagementOS``  
3. 验证该所有 Hyper-V 主机 (和外部管理资源：DNS 服务器) 可以通过使用其管理 IP 地址和/或完全限定的域名 (FQDN) ping 访问。   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  
4. 运行以下命令部署主机上，并指定的每个 Hyper-V 主机，以确保所使用的 Kerberos 凭据 FQDN 提供访问权限的所有服务器。  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Nano 安装要求和笔记  
如果你为你 Hyper-V 主机（物理服务器）中使用 Nano 部署，以下是附加要求：  
1. 所有 Nano 节点都需要具有安装了语言包 DSC 包：  
   
   * Microsoft-NanoServer-DSC-Package.cab  
   * Microsoft-NanoServer-DSC-Package_en-us.cab
   
        ``dism /online /add-package /packagepath:<Path> /loglevel:4``  
2. 从非 Nano 主机（Windows Server 核心或 Windows Server 带 GUI），必须运行 SDN Express 的脚本。 PowerShellNano 不支持其工作流程。  
3.  会网络控制器 NorthBound API 调用使用 PowerShell 或（其中依赖 Invoke-WebRequest 和 Invoke-RestMethod）NC 其余包装必须从非 Nano 主机。  
   
         
### <a name="run-sdn-express-scripts"></a>运行 SDN 快速脚本  
  
1.  安装文件位于 GitHub 上。 下载从 zip 文件[Microsoft SDN GitHub 存储库](https://github.com/Microsoft/SDN.git)。 在 Microsoft SDN 知识库页上，单击**或下载**，然后单击**下载压缩**。  
  
2.  为你部署的计算机指定一台计算机。  该计算机必须运行 Windows Server 2016。 展开压缩文件和副本**SDNExpress**用于部署的计算机文件夹`C:\`文件夹。  
  
3.  共享`C:\SDNExpress`与文件夹"**SDNExpress**"使用权限的**每个人都**到**读取/写入**。  
  
4.  导航到`C:\SDNExpress`文件夹。

 你将看到以下文件夹：  

|文件夹名称|描述|  
|---------------|---------------|  
|AgentConf|保存 OVSDB 架构 SDN 主机代理计划网络策略为每个 Windows Server 2016 Hyper-V 主机上使用的最新副本。|  
|证书|临时 NC 证书文件的共享的位置。|  
|图像|清空、Windows Server 2016 vhdx 映像在此处|  
|工具|实用疑难解答和调试程序。  复制到主机和虚拟机。  我们建议您将网络显示器或 Wireshark 此处所以在需要时。|  
|脚本|部署脚本。<br /><br />-   **SDNExpress.ps1**<br />    部署和配置结构，包括虚拟机网络控制器、SLB 输入混合虚拟机、网关池和 HNV 网关虚拟机对应于池。<br />-   **FabricConfig.psd1**<br />    配置文件 SDNExpress 脚本模板。  您会将此自定义您的环境。<br />-   **SDNExpressTenant.ps1**<br />    部署负载平衡 VIP 的虚拟网络上示例租户工作负载。<br />    此外提供一个或多个网络连接（IPSec S2S VPN、GRE、L3）上的服务提供商 edge 网关其连接到之前创建的租户工作负载。 IPSec 和 GRE 网关可用于连接通过相应 VIP IP 地址，或者 L3 转移网关通过相应地址池。<br />    此脚本可用于还删除相应配置的撤消选项。<br />-   **TenantConfig.psd1**<br />    对租户工作负载和 S2S 网关配置模板配置文件。<br />-   **SDNExpressUndo.ps1**<br />    清理结构环境，并将其重置为起始状态。<br />-   **SDNExpressEnterpriseExample.ps1**<br />    提供了一个远程访问网关和每个站点（可选）个相应企业虚拟机的一个或多个企业站点环境。 IPSec 或 GRE 企业网关连接到相应的服务提供商网关建立 S2S 隧道 VIP IP 地址。 L3 转发网关连接通过相应等 IP 地址。 <br />            此脚本可用于还删除相应配置的撤消选项。<br />-   **EnterpriseConfig.psd1**<br />    企业站点的网关和客户端 VM 配置模板配置文件。|  
|TenantApps|使用部署示例租户工作负载的文件。|  
  
5.  Windows Server 2016 VHDX 文件是在验证**图像**文件夹。  
  
6. 通过更改来自定义 SDNExpress\scripts\FabricConfig.psd1 文件**<< 替换 >>**规划网络主题中列出标记的特定值适合你实验基础结构，包括主名称、域姓名、用户名和密码和网络的网络的信息。  
7. 创建 NetworkControllerRestName (FQDN) 和 NetworkControllerRestIP DNS 主机 A 记录。  
8. 运行使用管理员凭据的域的用户脚本：  
      
    ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
9.  若要还原所有操作，请运行以下命令：  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>验证  
假设 SDN Express 脚本完成到运行，并且不会报告的任何错误，你可以执行以下步骤，确保已正确部署结构资源，并且适用于租户部署。  

- 使用[诊断工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)以确保没有中有错误任何结构资源网络控制器。  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>部署示例租户工作负载的软件负载平衡  
    
现在，已部署结构资源，你可以通过部署示例租户工作负载来验证你 SDN 部署端到端。 两个虚拟子网（web 层和数据库层）通过访问控制列表 (ACL) 规则使用 SDN 分发防火墙保护包括此租户工作负载。 Web 层虚拟子网是通过使用虚拟 IP (VIP) 地址/SLB 输入混合访问。 此脚本自动部署和两个服务 web 层虚拟机和一个数据库层虚拟机并连接到虚拟个子网。  
  
1.  通过更改来自定义 SDNExpress\scripts\TenantConfig.psd1 文件**<< 替换 >>**标记的特定值 (例如：VHD 映像的名称、网络控制器其余部分名称、vSwitch 名称、FabricConfig.psd1 文件中，之前定义等)  
2.  运行此脚本。 例如：  
``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  
3.  撤消该配置，运行带有的相同脚本**撤消**参数。 例如：  
``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>验证  
若要验证租户部署成功，执行以下操作：
1.  登录到数据库层虚拟机并尝试连接的一项 web 层虚拟机（确保中 web 层虚拟机关闭 Windows 防火墙）的 IP 地址。  
2.  检查有错误的网络控制器租户资源。 运行以下命令从任何 Hyper-V 主机了一层 3 连接到网络控制器：  
      
    ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``
3. 若要验证负载平衡正确运行，从任何 Hyper-V 主机运行以下命令：
    
        wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing
   
   其中`<VIP IP address>`是 web 层在 TenantConfig.psd1 文件配置 VIP IP 地址。 搜索`VIPIP`中 TenantConfig.psd1 变量。

   运行此多情况下，若要查看负载平衡可用 Dip 之间进行切换。 你还可以观察使用 web 浏览器此问题。 浏览到`<VIP IP address>/unique.htm`。 关闭使用浏览器打开新实例，然后再次浏览。 你将看到蓝色页和备用，除在浏览器缓存页面缓存超时之前绿色的页面。
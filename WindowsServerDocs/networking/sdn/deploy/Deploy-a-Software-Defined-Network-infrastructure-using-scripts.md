---
title: 部署软件定义的网络基础结构使用脚本
description: 本主题介绍如何部署使用 Windows Server 2016 中的脚本的 Microsoft 软件定义网络 (SDN) 基础结构。
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: dabfe3de4cc307723ff7e614fb73e3903e74aeb2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844618"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>使用脚本部署软件定义的网络基础结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题中，将部署使用脚本的 Microsoft 软件定义网络 (SDN) 基础结构。 基础结构包括具有高可用性 (HA) 网络控制器，HA 软件负载均衡器 (SLB) / MUX、 虚拟网络和关联的访问控制列表 (Acl)。 此外，另一个脚本部署，以验证在 SDN 基础结构的租户工作负荷。  
  
如果你想租户工作负载以其虚拟网络外部进行通信，你可以设置 SLB NAT 规则、 站点到站点网关隧道或第 3 层转发以虚拟和物理工作负荷之间进行路由。  
  
您还可以部署 SDN 基础结构使用 Virtual Machine Manager (VMM)。 有关详细信息，请参阅[设置在 VMM 构造中软件定义网络 (SDN) 基础结构](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。  

  
## <a name="pre-deployment"></a>预先部署  
  
> [!IMPORTANT]  
> 在开始部署之前，必须规划和配置在主机和物理网络基础结构。 有关详细信息，请参阅[计划软件定义的网络基础结构](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。  
  
所有的 HYPER-V 主机必须具有 Windows Server 2016 安装。  
  
## <a name="deployment-steps"></a>部署步骤  
开始配置的 HYPER-V 主机 （物理服务器） 的 HYPER-V 虚拟交换机和 IP 地址分配。 可以使用适用于 HYPER-V，共享或本地任何存储类型。  

### <a name="install-host-networking"></a>安装主机网络  

1. 安装适用于 NIC 硬件的最新网络驱动程序。  
2. 在所有主机上安装 HYPER-V 角色 (有关详细信息，请参阅[开始使用 Windows Server 2016 上的 HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows)。   
  
   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  
    
3. 创建 HYPER-V 虚拟交换机。<p>例如，使用相同的交换机名称的所有主机**sdnSwitch**。 配置至少一个网络适配器，或者，如果使用的设置，配置至少两个网络适配器。 使用两个 Nic 时，会出现最大入站进行传播。  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >如果你有独立的管理 Nic，则可以跳过步骤 4 和 5。

3. 请参阅规划主题 ([规划软件定义网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 和工作与网络管理员联系以获取管理 VLAN 的 VLAN ID。 将新创建的虚拟交换机管理 vNIC 连接到管理 VLAN。 如果你的环境不使用 VLAN 标记，则可以省略此步骤。  
   
   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  
 
4. 请参阅规划主题 ([规划软件定义网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)) 以及使用网络管理员联系，以使用是 DHCP 或静态 IP 分配，以将 IP 地址分配给新创建的管理 vNICvSwitch。 下面的示例演示如何创建静态 IP 地址并将其分配给管理 vSwitch 的 vNIC:  
 
   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  
      
5. [可选]将虚拟机部署到主机 Active Directory 域服务 ([安装 Active Directory 域服务 (级别 100)](https://technet.microsoft.com/library/hh472162.aspx)和 DNS 服务器。  
   
    a. Active Directory/DNS 服务器虚拟机连接到管理 VLAN:
    
       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. 安装 Active Directory 域服务和 DNS。  

   >[!NOTE]
   >网络控制器进行身份验证支持 Kerberos 和 X.509 证书。 （尽管只有一个是必需的），本指南针对不同目的使用这两种身份验证机制。  
        
6. 加入到域的所有 HYPER-V 主机。 请确保已分配到管理网络点到可解析的域名的 DNS 服务器的 IP 地址的网络适配器的 DNS 服务器条目。 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```
   
   a. 右键单击**启动**，单击**系统**，然后单击**更改设置**。  
   b. 单击“更改”。  
   c. 单击**域**和指定域的名称。  
   d. 单击 **“确定”**。  
   e. 键入用户名称和密码凭据出现提示时。  
   f. 重新启动服务器。  
  
### <a name="validation"></a>验证  
使用以下步骤来验证该主机网络设置正确。  

1. 请确保已成功创建虚拟机交换机：  
      
   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. 验证 VM 交换机上的管理 vNIC 连接到管理 VLAN:  

   >[!NOTE]
   >只适用于管理和租户通信共享同一 nic。    
      
   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. 验证所有的 HYPER-V 主机和外部管理资源，例如，DNS 服务器。<p>请确保它们是可通过使用其管理 IP 地址和/或完全限定的域名 (FQDN) 的 ping 访问。   
      
   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. 在部署主机上运行以下命令并指定每个 HYPER-V 主机以确保使用的 Kerberos 凭据的 FQDN 提供对所有服务器的访问。  
      
   ``winrm id -r:<Hyper-V Host FQDN>``  
      
### <a name="nano-installation-requirements-and-notes"></a>Nano 安装要求和说明  

如果使用 Nano 作为 HYPER-V 主机 （物理服务器） 的部署，以下是其他要求：  

1. 所有 Nano 节点都需要用该语言包已安装 DSC 包：  
   
    - Microsoft-NanoServer-DSC-Package.cab  
    - Microsoft-NanoServer-DSC-Package_en-us.cab
   
    ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. SDN Express 脚本必须从非 Nano 主机 （Windows Server Core 或带 GUI 的 Windows Server） 运行。 在 Nano 上不支持 PowerShell 工作流。  

3.  网络控制器 NorthBound API 调用使用 PowerShell 或 NC REST 包装器 （后者又依赖于调用 WebRequest 和 Invoke-restmethod） 必须在完成从非 Nano 主机。  
   
         
### <a name="run-sdn-express-scripts"></a>运行 SDN Express 脚本  
  
1. 转到[Microsoft SDN GitHub 存储库](https://github.com/Microsoft/SDN.git)的安装文件。

2. 从存储库将安装文件下载到指定的部署计算机。 单击**克隆或下载**，然后单击**下载 ZIP**。  
 
   >[!NOTE]
   >指定的部署计算机必须运行 Windows Server 2016 或更高版本。
 
3. 展开 zip 文件并复制**SDNExpress**到部署计算机的文件夹`C:\`文件夹。  
  
4. 共享`C:\SDNExpress`的文件夹"**SDNExpress**"使用权限**每个人都**到**读/写**。  
  
5. 导航到`C:\SDNExpress`文件夹。<p>您将看到以下文件夹：  

   |文件夹名|描述|  
   |---------------|---------------|  
   |AgentConf|保存到程序网络策略的每个 Windows Server 2016 HYPER-V 主机上的 SDN 主机代理所使用的 OVSDB 架构的最新副本。|  
   |证书|NC 证书文件的临时共享的位置。|  
   |映像|为空，将你的 Windows Server 2016 vhdx 映像放在此处|  
   |工具|用于故障排除和调试实用工具。  复制到主机和虚拟机。  我们建议您将放在网络监视器或 Wireshark 此处这样才可以根据需要使用它。|  
   |脚本|部署脚本。<br /><br />-   **SDNExpress.ps1**<br />    部署和配置构造，包括网络控制器虚拟机、 SLB Mux 虚拟机、 网关池和 HNV 网关虚拟机对应的池。<br />-   **FabricConfig.psd1**<br />    SDNExpress 脚本配置文件模板。  你会将此自定义您的环境。<br />-   **SDNExpressTenant.ps1**<br />    部署使用负载均衡 VIP 的虚拟网络上的示例租户工作负荷。<br />    此外将一个或多个网络连接 （IPSec S2S VPN，GRE，L3） 预配服务提供商边缘网关连接到以前创建的租户工作负荷上。 IPSec 和 GRE 网关都可以进行连接对相应的 VIP IP 地址和 L3 转发网关通过相应的地址池。<br />    此脚本可用于删除相应的配置，并可通过撤消选项。<br />-   **TenantConfig.psd1**<br />    用于租户工作负载和 S2S 网关配置的模板配置文件。<br />-   **SDNExpressUndo.ps1**<br />    清理 fabric 环境并将其重置为起始状态。<br />-   **SDNExpressEnterpriseExample.ps1**<br />    预配一个或多个企业站点环境中使用一个远程访问网关和每个站点 （可选） 一个相应的企业版虚拟机。 IPSec 和 GRE 企业网关连接到服务提供商网关建立 S2S 隧道的相应 VIP IP 地址。 L3 转发网关连接通过相应的对等 IP 地址。 <br />            此脚本可用于删除相应的配置，并可通过撤消选项。<br />-   **EnterpriseConfig.psd1**<br />    用于企业站点到站点网关和客户端 VM 配置的模板配置文件。|  
   |TenantApps|若要部署的示例租户工作负荷所使用的文件。|  
   ---
  
6. 验证 Windows Server 2016 VHDX 文件位于**映像**文件夹。  
  
7. 通过更改来自定义 SDNExpress\scripts\FabricConfig.psd1 文件 **<< 替换 >>** 标记具有特定值，以适合你的实验室基础结构包括主机名、 域名、 用户名和密码，并规划网络主题中列出的网络的网络信息。  

8. NetworkControllerRestName (FQDN) 和 NetworkControllerRestIP 在 DNS 中创建的主机 A 记录。  

9. 具有域管理员凭据的用户身份运行脚本：  
      
   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
10. 若要撤消所有操作，请运行以下命令：  
      
    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  
      
#### <a name="validation"></a>验证  

假设 SDN Express 脚本已完成运行而不报告任何错误，可以执行以下步骤，以确保构造资源已正确部署，并且可供租户部署。  

使用[诊断工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)以确保没有任何错误上任何构造资源在网络控制器。  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  
        
   
### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>部署具有软件负载均衡器的示例租户工作负荷  
    
现在，已部署构造资源，您可以通过部署示例租户工作负荷来验证你 SDN 部署端到端。 此租户工作负荷包含两个虚拟子网 （web 层和数据库层） 通过使用 SDN 分布式防火墙的访问控制列表 (ACL) 规则保护。 通过使用虚拟 IP (VIP) 地址在 SLB/MUX 可访问 web 层的虚拟子网。 该脚本自动部署两个 web 层虚拟机和一个数据库层虚拟机，并将连接到的虚拟子网。  
  
1.  通过更改来自定义 SDNExpress\scripts\TenantConfig.psd1 文件 **<< 替换 >>** 具有特定值的标记 (例如：VHD 映像名称，网络控制器 REST 名称、 vSwitch 名称，如前面 FabricConfig.psd1 文件中定义的等）  

2.  运行脚本。 例如：  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  若要撤消该配置，请运行与同一个脚本**撤消**参数。 例如：  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>验证  

若要验证租户部署成功，请执行以下操作：

1. 登录到数据库层虚拟机，并尝试对其中一个 web 层虚拟机 （请确保 Windows 防火墙处于关闭状态在 web 层虚拟机） 的 IP 地址执行 ping 操作。  

2. 检查有任何错误的网络控制器租户资源。 运行以下命令从任何 HYPER-V 主机与第 3 层连接到网络控制器：  
      
   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. 若要验证负载均衡器正常运行，从任何 HYPER-V 主机运行以下命令：
    
   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``
   
   其中`<VIP IP address>`是 web 层 TenantConfig.psd1 文件中配置的 VIP IP 地址。 

   >[!TIP]
   >搜索`VIPIP`TenantConfig.psd1 中用户定义变量。

   运行此多时间以查看负载均衡器可用 Dip 之间进行切换。 您可以观察此行为，使用 web 浏览器。 浏览到 `<VIP IP address>/unique.htm`。 关闭浏览器支持和打开的新实例，然后再次浏览。 你将看到蓝色页面和备用，除了当浏览器缓存该页面之前缓存超时绿色页。

---
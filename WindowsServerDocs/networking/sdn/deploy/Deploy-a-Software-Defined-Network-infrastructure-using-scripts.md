---
title: 使用脚本部署软件定义的网络基础结构
description: 本主题介绍如何使用 Windows Server 2016 中的脚本部署 Microsoft 软件定义的网络（SDN）基础结构。
manager: dougkim
ms.prod: windows-server
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 5ba5bb37-ece0-45cb-971b-f7149f658d19
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 29013827d0cde0447c48afa7a42551760ab9e940
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356003"
---
# <a name="deploy-a-software-defined-network-infrastructure-using-scripts"></a>使用脚本部署软件定义的网络基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何使用脚本部署 Microsoft 软件定义的网络（SDN）基础结构。 该基础结构包括高可用性（HA）网络控制器、HA 软件负载平衡器（SLB）/MUX、虚拟网络和关联的访问控制列表（Acl）。 此外，另一个脚本将部署一个租户工作负荷，用于验证 SDN 基础结构。  

如果你希望你的租户工作负荷在其虚拟网络外部进行通信，则可以设置 SLB NAT 规则、站点到站点网关隧道或第3层转发，以在虚拟和物理工作负载之间进行路由。  

你还可以使用 Virtual Machine Manager （VMM）部署 SDN 基础结构。 有关详细信息，请参阅[在 VMM 构造中设置软件定义的网络（SDN）基础结构](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-overview)。  


## <a name="pre-deployment"></a>预先部署  

> [!IMPORTANT]  
> 在开始部署之前，必须计划和配置主机和物理网络基础结构。 有关详细信息，请参阅[计划软件定义的网络基础结构](../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)。  

所有 Hyper-v 主机都必须安装 Windows Server 2016。  

## <a name="deployment-steps"></a>部署步骤  
首先配置 Hyper-v 主机的（物理服务器） Hyper-v 虚拟交换机和 IP 地址分配。 可以使用与 Hyper-v 兼容的任何存储类型。  

### <a name="install-host-networking"></a>安装主机网络  

1. 安装适用于 NIC 硬件的最新网络驱动程序。  
2. 在所有主机上安装 Hyper-v 角色（有关详细信息，请参阅[Windows Server 2016 上的 hyper-v 入门](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/Get-started-with-Hyper-V-on-Windows)。   

   ```PowerShell
   Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
   ```  

3. 创建 Hyper-v 虚拟交换机。<p>对所有主机使用相同的开关名称，例如， **sdnSwitch**。 至少配置一个网络适配器，如果使用 "设置"，则至少配置两个网络适配器。 使用两个 Nic 时发生最大入站分配。  

   ```PowerShell
   New-VMSwitch "<switch name>" -NetAdapterName "<NetAdapter1>" [, "<NetAdapter2>" -EnableEmbeddedTeaming $True] -AllowManagementOS $True
   ```  
   >[!TIP] 
   >如果有单独的管理 Nic，则可以跳过步骤4和5。

3. 请参阅规划主题（[规划软件定义的网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)），与网络管理员合作以获取管理 VLAN 的 vlan ID。 将新创建的虚拟交换机的管理 vNIC 附加到管理 VLAN。 如果你的环境不使用 VLAN 标记，则可以省略此步骤。  

   ```PowerShell
   Set-VMNetworkAdapterIsolation -ManagementOS -IsolationMode Vlan -DefaultIsolationID <Management VLAN> -AllowUntaggedTraffic $True
   ```  

4. 请参阅规划主题（[规划软件定义的网络基础结构](../../sdn/plan/../../sdn/plan/../../sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)），与网络管理员协作，使用 DHCP 或静态 IP 分配将 IP 地址分配给新创建的 VSwitch 的管理 vNIC。 下面的示例演示如何创建静态 IP 地址并将其分配给 vSwitch 的 Management vNIC：  

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (<switch name>)" -IPAddress <IP> -DefaultGateway <Gateway IP> -AddressFamily IPv4 -PrefixLength <Length of Subnet Mask - for example: 24>
   ```  

5. 可有可无将虚拟机部署到主机 Active Directory 域服务（[安装 Active Directory 域服务（级别100）](https://technet.microsoft.com/library/hh472162.aspx)和 DNS 服务器。  

    a. 将 Active Directory/DNS 服务器虚拟机连接到管理 VLAN：

       ```PowerShell
       Set-VMNetworkAdapterIsolation -VMName "<VM Name>" -Access -VlanId <Management VLAN> -AllowUntaggedTraffic $True  
       ```   

   b. 安装 Active Directory 域服务和 DNS。  

   >[!NOTE]
   >网络控制器支持 Kerberos 和 x.509 证书进行身份验证。 本指南出于不同目的使用这两种身份验证机制（虽然只需要一个）。  

6. 将所有 Hyper-v 主机加入到域。 确保将 IP 地址分配到管理网络的网络适配器的 DNS 服务器条目指向可解析域名的 DNS 服务器。 

   ```PowerShell   
   Set-DnsClientServerAddress -InterfaceAlias "vEthernet (<switch name>)" -ServerAddresses <DNS Server IP>  
   ```

   a. 右键单击 "**开始**"，单击 "**系统**"，然后单击 "**更改设置**"。  
   b. 单击“更改”。  
   c. 单击 "**域**" 并指定域名。  
   d. 单击 **“确定”** 。  
   e. 出现提示时，键入用户名和密码凭据。  
   f. 重新启动服务器。  

### <a name="validation"></a>验证  
使用以下步骤验证是否正确设置了主机网络。  

1. 请确保已成功创建 VM 交换机：  

   ```PowerShell
   Get-VMSwitch "<switch name>"
   ```  

2. 验证 VM 交换机上的管理 vNIC 是否已连接到管理 VLAN：  

   >[!NOTE]
   >仅适用于管理和租户通信共享同一 NIC 的情况。    

   ```PowerShell
   Get-VMNetworkAdapterIsolation -ManagementOS
   ```

3. 验证所有 Hyper-v 主机和外部管理资源，例如 DNS 服务器。<p>确保可以使用其管理 IP 地址和/或完全限定的域名（FQDN）通过 ping 访问它们。   

   ``ping <Hyper-V Host IP>``  
   ``ping <Hyper-V Host FQDN>``  

4. 在部署主机上运行以下命令，并指定每个 Hyper-v 主机的 FQDN，以确保使用的 Kerberos 凭据提供对所有服务器的访问。  

   ``winrm id -r:<Hyper-V Host FQDN>``  

### <a name="nano-installation-requirements-and-notes"></a>Nano 安装要求和说明  

如果使用 Nano 作为部署的 Hyper-v 主机（物理服务器），以下是其他要求：  

1. 所有 Nano 节点都需要安装带有语言包的 DSC 包：  

   - Microsoft-NanoServer-DSC-Package  
   - Microsoft-NanoServer-DSC-Package_en-us

     ``dism /online /add-package /packagepath:<Path> /loglevel:4``  

2. 必须从非 Nano 主机（Windows Server Core 或 Windows Server w/GUI）运行 SDN Express 脚本。 Nano 上不支持 PowerShell 工作流。  

3. 使用 PowerShell 或 NC REST 包装器（依赖于 WebRequest 和 Invoke-restmethod）调用网络控制器 NorthBound API 必须从非 Nano 主机进行。  


### <a name="run-sdn-express-scripts"></a>运行 SDN Express 脚本  

1. 请参阅[MICROSOFT SDN GitHub 存储库](https://github.com/Microsoft/SDN.git)中的安装文件。

2. 将安装文件从存储库下载到指定的部署计算机。 单击 "**克隆或下载**"，然后单击 "**下载 ZIP**"。  

   >[!NOTE]
   >指定的部署计算机必须运行 Windows Server 2016 或更高版本。

3. 展开 zip 文件并将**SDNExpress**文件夹复制到部署计算机的 @no__t 1 文件夹中。  

4. 将 `C:\SDNExpress` 文件夹共享为 "**SDNExpress**"，以便**每个人都**可以**读取/写入**。  

5. 导航到该`C:\SDNExpress`文件夹。<p>你会看到以下文件夹：  


   | 文件夹名 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
   |-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |  AgentConf  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   保存每个 Windows Server 2016 Hyper-v 主机上 SDN 主机代理使用的 OVSDB 架构的新副本，以对网络策略进行编程。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
   |    证书    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         NC 证书文件的临时共享位置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |   映像    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         空，将 Windows Server 2016 vhdx 映像置于此处                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   |    工具    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            用于故障排除和调试的实用程序。  复制到主机和虚拟机。  建议将网络监视器或 Wireshark 放在此处，以便它在需要时可用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |   脚本   | 部署脚本。<br /><br />@no__t**SDNExpress**<br />    部署和配置构造，包括网络控制器虚拟机、SLB Mux 虚拟机、网关池和与池相对应的 HNV 网关虚拟机。<br />-   **FabricConfig. psd1**<br />    SDNExpress 脚本的配置文件模板。  你将为你的环境自定义此。<br />@no__t**SDNExpressTenant**<br />    在具有负载平衡 VIP 的虚拟网络上部署示例租户工作负荷。<br />    还在已连接到以前创建的租户工作负荷的服务提供商边缘网关上预配一个或多个网络连接（IPSec S2S VPN、GRE、L3）。 IPSec 和 GRE 网关可通过相应的 VIP IP 地址连接，并可通过相应的地址池连接到 L3 转发网关。<br />    此脚本也可用于删除带有撤消选项的相应配置。<br />-   **TenantConfig. psd1**<br />    租户工作负荷和 S2S 网关配置的模板配置文件。<br />@no__t**SDNExpressUndo**<br />    清理构造环境，并将其重置为启动状态。<br />@no__t**SDNExpressEnterpriseExample**<br />    为每个站点提供一个或多个具有一个远程访问网关和（可选）一个对应企业虚拟机的企业站点环境。 IPSec 或 GRE 企业网关连接到服务提供商网关对应的 VIP IP 地址，以建立 S2S 隧道。 L3 转发网关通过相应的对等 IP 地址进行连接。 <br />            此脚本也可用于删除带有撤消选项的相应配置。<br />-   **EnterpriseConfig. psd1**<br />    企业站点到站点网关和客户端 VM 配置的模板配置文件。 |
   | TenantApps  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             用于部署示例租户工作负荷的文件。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

   ---

6. 验证 Windows Server 2016 VHDX 文件是否位于**Images**文件夹中。  

7. 通过更改 < 自定义 SDNExpress\scripts\FabricConfig.psd1 文件 **< 将 > > 标记替换**为特定值，以适合实验室基础结构（包括主机名、域名、用户名和密码）以及的网络信息。规划网络主题中列出的网络。  

8. 在 DNS 中为 NetworkControllerRestName （FQDN）和 NetworkControllerRestIP 创建主机 A 记录。  

9. 以具有域管理员凭据的用户身份运行脚本：  

   ``SDNExpress\scripts\SDNExpress.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

10. 若要撤消所有操作，请运行以下命令：  

    ``SDNExpress\scripts\SDNExpressUndo.ps1 -ConfigurationDataFile FabricConfig.psd1 -Verbose``  

#### <a name="validation"></a>验证  

假设 SDN Express 脚本在不报告任何错误的情况下运行到完成，则可以执行以下步骤以确保构造资源已正确部署并且可用于租户部署。  

使用[诊断工具](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack)确保网络控制器中的任何构造资源上无错误。  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller Rest Name>``  


### <a name="deploy-a-sample-tenant-workload-with-the-software-load-balancer"></a>使用软件负载平衡器部署示例租户工作负荷  

现在已部署了构造资源，你可以通过部署示例租户工作负荷来端到端地验证 SDN 部署。 此租户工作负荷由使用 SDN 分布式防火墙通过访问控制列表（ACL）规则保护的两个虚拟子网（web 层和数据库层）组成。 可以使用虚拟 IP （VIP）地址通过 SLB/MUX 访问 web 层的虚拟子网。 脚本会自动部署两个 web 层虚拟机和一个数据库层虚拟机，并将这些虚拟机连接到虚拟子网。  

1.  通过更改 **< < 将 > > 标记替换**为特定值来自定义 SDNExpress\scripts\TenantConfig.psd1 文件（例如：VHD 映像名称、网络控制器 REST 名称、vSwitch 名称等，如先前在 FabricConfig. psd1 文件中定义的那样）  

2.  运行脚本。 例如：  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

3.  若要撤消配置，请运行包含**undo**参数的同一个脚本。 例如：  

    ``SDNExpress\scripts\SDNExpressTenant.ps1 -Undo -ConfigurationDataFile TenantConfig.psd1 -Verbose``  

#### <a name="validation"></a>验证  

若要验证租户部署是否成功，请执行以下操作：

1. 登录到数据库层虚拟机，尝试对其中一台 web 层虚拟机的 IP 地址进行 ping 操作（确保 web 层虚拟机中的 Windows 防火墙处于关闭状态）。  

2. 检查网络控制器租户资源是否有任何错误。 从具有与网络控制器的第3层连接的任何 Hyper-v 主机运行以下内容：  

   ``Debug-NetworkControllerConfigurationState -NetworkController <FQDN of Network Controller REST Name>``

3. 若要验证负载均衡器是否正常运行，请从任何 Hyper-v 主机运行以下内容：

   ``wget <VIP IP address>/unique.htm -disablekeepalive -usebasicparsing``

   其中 `<VIP IP address>` 是在 TenantConfig. psd1 文件中配置的 web 层 VIP IP 地址。 

   >[!TIP]
   >在 TenantConfig. psd1 中搜索 `VIPIP` 变量。

   运行此多个时间以查看可用 Dip 之间的负载均衡器开关。 你还可以使用 web 浏览器查看此行为。 浏览到 `<VIP IP address>/unique.htm`。 关闭浏览器并打开一个新的实例，然后重新浏览。 你将看到蓝页和绿色页备用，除非浏览器在缓存超时前缓存页面。

---
---
title: 步骤3配置多站点部署
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccfde5d13b9b2b722498e824d497a9b790875e14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404508"
---
# <a name="step-3-configure-the-multisite-deployment"></a>步骤3配置多站点部署

>适用于：Windows Server（半年频道）、Windows Server 2016

配置多站点基础结构后，请按照以下步骤设置远程访问多站点部署。  
  
|任务|描述|  
|----|--------|  
|3.1. 配置远程访问服务器|通过设置 IP 地址、将它们加入域以及安装远程访问角色来配置其他远程访问服务器。|  
|3.2. 授予管理员访问权限|向 DirectAccess 管理员授予对其他远程访问服务器的权限。|  
|3.3. 为多站点部署配置 ip-https|配置在多站点部署中使用的 ip-https 证书。|  
|3.4. 为多站点部署配置网络位置服务器|配置在多站点部署中使用的网络位置服务器证书。|  
|3.5. 为多站点部署配置 DirectAccess 客户端|从 Windows 8 安全组中删除 Windows 7 客户端计算机。|  
|3.6. 启用多站点部署|在第一个远程访问服务器上启用多站点部署。|  
|3.7。 向多站点部署添加入口点|向多站点部署添加其他入口点。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_ConfigServer"></a>3.1。 配置远程访问服务器  

  
### <a name="to-install-the-remote-access-role"></a>安装远程访问角色  
  
1.  请确保每个远程访问服务器都配置了适当的部署拓扑（在 NAT、单个网络接口后面有边缘）以及相应的路由。  
  
2.  根据站点拓扑和组织的 IP 寻址方案，配置每个远程访问服务器上的 IP 地址。  
  
3.  将每个远程访问服务器联接到 Active Directory 域。  
  
4.  在服务器管理器控制台的 "**仪表板**" 中，单击 "**添加角色和功能**"。  
  
5.   单击 **“下一步”** 三次以打开服务器角色选择屏幕。  
  
6.  在 "**选择服务器角色**" 对话框中，选择 "**远程访问**"，然后单击 "**下一步**"。  
  
7.  单击 "**下一步**" 三次。  
  
8.  在 "**选择角色服务**" 对话框中，选择 " **DirectAccess 和 VPN （RAS）** "，然后单击 "**添加功能**"。  
  
9.  依次选择 "**路由**"、" **Web 应用程序代理**"、"**添加功能**"，然后单击 "**下一步**"。  
  
10. 单击 **“下一步”** ，然后单击 **“安装”** 。  
  
11.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
  
@no__t 0Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  

  
步骤 1-3 必须手动执行，不能使用此 Windows PowerShell cmdlet 完成。  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Admin"></a>3.2。 授予管理员访问权限  
  
#### <a name="to-grant-administrator-permissions"></a>授予管理员权限  
  
1.  在远程访问服务器的其他入口点上：在 "**开始**" 屏幕上，键入 "**计算机管理**"，然后按 enter。  
  
2.  在左窗格中，单击 "**本地用户和组**"。  
  
3.  双击 "**组**"，然后双击 "**管理员**"。  
  
4.  在 "**管理员属性**" 对话框中，单击 "**添加**"，然后在 "**选择用户、计算机、服务帐户或组**" 对话框中，单击 "**位置**"。  
  
5.  在 "**位置**" 对话框的 "**位置**" 树中，单击包含 DirectAccess 管理员用户帐户的位置，然后单击 **"确定"** 。  
  
6.  在 "**输入要选择的对象名称**" 中，输入 DirectAccess 管理员的用户名，然后单击 **"确定"** 两次。  
  
7.  在 "**管理员属性**" 对话框中，单击 **"确定"** 。  
  
8.  关闭 "计算机管理" 窗口。  
  
9. 在将成为多站点部署的一部分的所有远程访问服务器上重复此过程。  
  
## <a name="BKMK_IPHTTPS"></a>3.3。 为多站点部署配置 ip-https  
在将添加到多站点部署的每个远程访问服务器上，需要使用 SSL 证书来验证到 ip-https web 服务器的 HTTPS 连接。 必须至少具有本地 **Administrators** 组中的成员身份或同等身份才能完成此过程。  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>获取 IP-HTTPS 证书  
  
1.  在每个远程访问服务器上：在 "**开始**" 屏幕上，键入**mmc**，然后按 enter。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  单击 "**文件**"，然后单击 "**添加/删除管理单元**"。  
  
3.  依次单击 "**证书**"、"**添加**"、"**计算机帐户**"、"**下一步**"、"**本地计算机**"、"**完成**" 和 **"确定"** 。  
  
4.  在证书管理单元的控制台树中，依次打开“证书(本地计算机)\个人\证书”。  
  
5.  右键单击“证书”，指向“所有任务”，然后单击“申请新证书”。  
  
6.  单击“下一步” 两次。  
  
7.  在 "**申请证书**" 页上，单击 "Web 服务器证书" 模板，然后单击 "**注册此证书需要详细信息**"。  
  
    如果未显示 "Web 服务器证书" 模板，请确保远程访问服务器计算机帐户具有 Web 服务器证书模板的 "注册" 权限。 有关详细信息，请参阅[配置 Web 服务器证书模板上的权限](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx)。  
  
8.  在 "**证书属性**" 对话框的 "**使用者**" 选项卡的 "**使用者名称**" 中，为 "**类型**" 选择 "**公用名**"。  
  
9. 在 "**值**" 中，键入远程访问服务器的 Internet 名称的完全限定的域名（FQDN）（例如，Europe.contoso.com），然后单击 "**添加**"。  
  
10. 依次单击“确定”、“注册”和“完成”。  
  
11. 在 "证书" 管理单元的详细信息窗格中，验证具有 FQDN 的新证书是否已注册到**服务器身份验证**的**预期目的**。  
  
12. 右键单击该证书，然后单击 "**属性**"。  
  
13. 在 "**友好名称**" 中，键入**ip-https 证书**，然后单击 **"确定"** 。  
  
    > [!TIP]  
    > 步骤12和13是可选的，但在配置远程访问时，可以更轻松地选择 ip-https 证书。  
  
14. 在部署中的所有远程访问服务器上重复此过程。  
  
## <a name="BKMK_NLS"></a>3.4。 为多站点部署配置网络位置服务器  
如果在设置第一台服务器时选择了在远程访问服务器上设置网络位置服务器网站，则需要为所添加的每个新的远程访问服务器配置一个 Web 服务器证书，该证书具有为 t 选择的相同使用者名称第一台服务器的网络位置服务器。 每个服务器都需要一个证书来验证网络位置服务器的连接，并且位于内部网络中的客户端计算机必须能够解析 DNS 中网站的名称。  
  
#### <a name="to-install-a-certificate-for-network-location"></a>为网络位置安装证书  
  
1.  在远程访问服务器上：在 "**开始**" 屏幕上，键入**mmc**，然后按 enter。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  单击 "**文件**"，然后单击 "**添加/删除管理单元**"。  
  
3.  依次单击 "**证书**"、"**添加**"、"**计算机帐户**"、"**下一步**"、"**本地计算机**"、"**完成**" 和 **"确定"** 。  
  
4.  在证书管理单元的控制台树中，依次打开“证书(本地计算机)\个人\证书”。  
  
5.  右键单击“证书”，指向“所有任务”，然后单击“申请新证书”。  
  
    > [!NOTE]  
    > 你还可以为第一个远程访问服务器导入用于网络位置服务器的相同证书。  
  
6.  单击“下一步” 两次。  
  
7.  在 "**申请证书**" 页上，单击 "Web 服务器证书" 模板，然后单击 "**注册此证书需要详细信息**"。  
  
    如果未显示 "Web 服务器证书" 模板，请确保远程访问服务器计算机帐户具有 Web 服务器证书模板的 "注册" 权限。 有关详细信息，请参阅[配置 Web 服务器证书模板上的权限](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx)。  
  
8.  在 "**证书属性**" 对话框的 "**使用者**" 选项卡的 "**使用者名称**" 中，为 "**类型**" 选择 "**公用名**"。  
  
9. 在 "**值**" 中，键入为第一个远程访问服务器的网络位置服务器证书配置的完全限定的域名（FQDN）（例如，nls.corp.contoso.com），然后单击 "**添加**"。  
  
10. 依次单击“确定”、“注册”和“完成”。  
  
11. 在 "证书" 管理单元的详细信息窗格中，验证具有 FQDN 的新证书是否已注册到**服务器身份验证**的**预期目的**。  
  
12. 右键单击该证书，然后单击 "**属性**"。  
  
13. 在 "**友好名称**" 中，键入**网络位置证书**，然后单击 **"确定"** 。  
  
    > [!TIP]  
    > 步骤12和13是可选的，但在配置远程访问时，可以更轻松地选择网络位置的证书。  
  
14. 在部署中的所有远程访问服务器上重复此过程。  
  
### <a name="NLS"></a>创建网络位置服务器 DNS 记录  
  
1.  在 DNS 服务器上：在 "**开始**" 屏幕上，键入 " **dnsmgmt.msc**"，然后按 enter。  
  
2.  在**DNS 管理器**控制台的左窗格中，打开内部网络的 "正向查找" 区域。 右键单击相关区域，然后单击 "**新建主机（A 或 AAAA）** "。  
  
3.  在 "**新建主机**" 对话框的 "**名称（如果为空则使用父域名称）** " 框中，输入用于第一个远程访问服务器的网络位置服务器的名称。 在 " **IP 地址**" 框中，输入远程访问服务器的面向 Intranet 的 IPv4 地址，然后单击 "**添加主机**"。 在“DNS”对话框中，单击“确定”。  
  
4.  在 "**新建主机**" 对话框的 "**名称（如果为空则使用父域名称）** " 框中，输入用于第一个远程访问服务器的网络位置服务器的名称。 在 " **IP 地址**" 框中，输入远程访问服务器的面向 Intranet 的 IPv6 地址，然后单击 "**添加主机**"。 在“DNS”对话框中，单击“确定”。  
  
5.  为部署中的每个远程访问服务器重复步骤3和4。  
  
6.  单击 **“完成”** 。  
  
7.  在将服务器添加为部署中的其他入口点之前重复此过程。  
  
## <a name="BKMK_Client"></a>3.5。 为多站点部署配置 DirectAccess 客户端  
DirectAccess Windows 客户端计算机必须是定义其 DirectAccess 关联的安全组的成员。 启用多站点之前，这些安全组可能包含 Windows 8 客户端和 Windows 7 客户端（如果选择适当的 "下层" 模式）。 启用了 multisite 后，在单服务器模式下的现有客户端安全组将仅转换为适用于 Windows 8 的安全组。 启用多站点后，必须将 DirectAccess Windows 7 客户端计算机移动到相应的专用 Windows 7 客户端安全组（与特定入口点相关联），否则它们将不能通过 DirectAccess 进行连接。 必须首先从现在为 Windows 8 安全组的现有安全组中删除 Windows 7 客户端。 注意：Windows 7 和 Windows 8 客户端安全组的成员的 windows 7 客户端计算机将失去远程连接，并且没有安装 SP1 的 Windows 7 客户端也将失去企业连接性。 因此，必须从 Windows 8 安全组中删除所有 Windows 7 客户端计算机。  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>从 Windows 8 安全组中删除 Windows 7 客户端  
  
1.  在主域控制器上，单击 "**开始**"，然后单击 " **Active Directory 用户和计算机**"。  
  
2.  若要从安全组中删除计算机，请双击安全组，然后在 " **< Group_Name" > 属性**"对话框中，单击"**成员**"选项卡。  
  
3.  选择 Windows 7 客户端计算机，然后单击 "**删除**"。  
  
4.  重复此过程以从 Windows 8 安全组中删除 Windows 7 客户端计算机。  
  
> [!IMPORTANT]  
> 启用远程访问多站点配置时，所有客户端计算机（Windows 7 和 Windows 8）都将失去远程连接，直到它们能够直接连接到公司网络或通过 VPN 更新其组策略。 当首次启用多站点功能时，以及禁用 multisite 时，这种情况都是如此。  
  
## <a name="BKMK_Enable"></a>3.6。 启用多站点部署  
若要配置多站点部署，请在现有的远程访问服务器上启用多站点功能。 在部署中启用多站点之前，请确保你具有以下信息：  
  
1.  全局负载均衡器设置和 IP 地址，如果要对部署中的所有入口点进行 DirectAccess 客户端连接的负载平衡。  
  
2.  如果要为 Windows 7 客户端计算机启用远程访问，则为部署中的第一个入口点包含 Windows 7 客户端计算机的安全组。  
  
3.  如果需要对 Windows 7 客户端计算机提供支持，则组策略对象名称（如果需要使用在部署中的第一个入口点上适用的非默认组策略对象）。  
  
### <a name="EnabledMultisite"></a>启用多站点配置  
  
1.  在现有的远程访问服务器上：在 "**开始**" 屏幕上，键入**ramgmtui.exe**，然后按 enter。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在远程访问管理控制台中，单击 "**配置**"，然后在 "**任务**" 窗格中单击 "**启用多站点**"。  
  
3.  在 "**启用多站点部署**向导" 的 "**开始之前**" 页上，单击 "**下一步**"。  
  
4.  在 "**部署名称**" 页的 "**多站点部署名称**" 中，输入部署的名称。 在 "**第一个入口点名称**" 中，输入一个名称以标识作为当前远程访问服务器的第一个入口点，然后单击 "**下一步**"。  
  
5.  在 "**入口点选择**" 页上，执行下列操作之一：  
  
    -   单击 "**自动分配入口点"，并允许客户端手动选择**自动将客户端计算机路由到最合适的入口点，同时还允许客户端计算机手动选择入口点。 手动入口点选择仅适用于 Windows 8 计算机。 单击“下一步”。  
  
    -   单击 "**自动分配入口点**"，自动将客户端计算机路由到最适合的入口点，然后单击 "**下一步**"。  
  
6.  在 "**全局负载平衡**" 页上，执行下列操作之一：  
  
    -   如果不想使用全局负载均衡，请单击 "**否，不要使用全局负载平衡**"，然后单击 "**下一步**"。  
  
        > [!NOTE]  
        > 选择此选项时，客户端计算机会自动连接到其最近的入口点。  
  
    -   如果要在所有入口点之间对流量进行负载均衡，请单击 **"是，使用全局负载均衡**"。 在 **"键入要由所有入口点使用的全局负载平衡 fqdn**" 中，输入全局负载平衡 fqdn，然后在 "键入包含第一个远程访问服务器的**此入口点的全局负载平衡 IP 地址**" 中输入全局负载为此入口点平衡 IP 地址，然后单击 "**下一步**"。  
  
7.  在 "**客户端支持**" 页上，执行下列操作之一：  
  
    -   若要限制对运行 Windows 8 或更高版本操作系统的客户端计算机的访问，请单击 "**限制访问运行 windows 8 或更高版本操作系统的客户端计算机**"，然后单击 "**下一步**"。  
  
    -   若要允许运行 Windows 7 的客户端计算机访问此入口点，请单击 "**允许运行 windows 7 的客户端计算机访问此入口点**"，然后单击 "**添加**"。 在 "**选择组**" 对话框中，选择包含 Windows 7 客户端计算机的安全组，单击 **"确定**"，然后单击 "**下一步**"。  
  
8.  在 "**客户端 GPO 设置**" 页上，接受此入口点的适用于 Windows 7 客户端计算机的默认 gpo，键入要自动创建远程访问的 gpo 的名称，或者单击 "**浏览**" 找到 Windows 7 客户端计算机的 gpo。然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > -   仅当你将入口点配置为允许 Windows 7 客户端计算机访问入口点时，才会显示 "**客户端 GPO 设置**" 页。  
    > -   您可以选择单击 "**验证 gpo** " 以确保您对此入口点的选定 gpo 具有适当的权限。 如果 GPO 不存在并且将自动创建，则需要创建和链接权限。 如果已手动创建 Gpo，则需要编辑、修改安全性和删除权限。  
  
9. 在 "**摘要**" 页上，单击 "**提交**"。  
  
10. 在 "**启用多站点部署**" 对话框中，单击 "**关闭**"，然后在 "启用多站点部署向导" 上，单击 "**关闭**"。  
  
@no__t 0Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要在名为 "Edge1-US" 的第一个入口点上启用名为 "Contoso" 的多站点部署。 部署允许客户端手动选择入口点，而不使用全局负载均衡器。  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
若要允许 Windows 7 客户端计算机通过安全组 DA_Clients_US 访问第一个入口点并使用 GPO DA_W7_Clients_GPO_US，请访问。  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="BKMK_EntryPoint"></a>3.7。 向多站点部署添加入口点  
在部署中启用多站点后，可以使用 "添加入口点向导" 添加其他入口点。 添加入口点之前，请确保你具有以下信息：  
  
-   如果使用全局负载平衡，则为每个新入口点使用全局负载均衡器 IP 地址。  
  
-   如果要为 Windows 7 客户端计算机启用远程访问，则为每个入口点包含 Windows 7 客户端计算机的安全组。  
  
-   组策略对象名称，如果需要使用非默认组策略对象，则在需要对 Windows 7 客户端计算机提供支持的情况下，这些对象适用于将添加的每个入口点的 Windows 7 客户端计算机。  
  
-   如果在组织的网络中部署了 IPv6，则需要为新的入口点准备 IP-HTTPS 前缀。  
  
### <a name="AddEP"></a>向多站点部署添加入口点  
  
1.  在现有的远程访问服务器上：在 "**开始**" 屏幕上，键入**ramgmtui.exe**，然后按 enter。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
2.  在远程访问管理控制台中，单击 "**配置**"，然后在 "**任务**" 窗格中单击 "**添加入口点**"。  
  
3.  在 "添加入口点" 向导的 "**入口点详细信息**" 页上的 "**远程访问服务器**" 中，输入要添加的服务器的完全限定的域名（FQDN）。 在 "**入口点名称**" 中，输入入口点的名称，然后单击 "**下一步**"。  
  
4.  在 "**全局负载平衡设置**" 页上，输入此入口点的全局负载平衡 IP 地址，然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > 仅当多站点配置使用全局负载均衡器时，才会显示 "**全局负载平衡设置**" 页。  
  
5.  在 "**网络拓扑**" 页上，单击与要添加的远程访问服务器的网络拓扑相对应的拓扑，然后单击 "**下一步**"。  
  
6.  在 "**网络名称或 Ip 地址**" 页上，在 **"键入客户端用于连接到远程访问服务器的公用名称或 ip 地址**" 中输入客户端用于连接到远程访问服务器的公用名称或 ip 地址。 公共名称对应于 IP-HTTPS 证书的使用者名称。 在实现了边缘网络拓扑的情况下，IP 地址是远程访问服务器的外部适配器的 IP 地址。 单击“下一步”。  
  
7.  在 "**网络适配器**" 页上，执行以下操作之一：  
  
    -   如果要部署具有两个网络适配器的拓扑，请在 "**外部适配器**" 中选择连接到外部网络的适配器。 在 "**内部适配器**" 中，选择连接到内部网络的适配器。  
  
    -   如果要使用一个网络适配器部署拓扑，请在 "**网络适配器**" 中选择连接到内部网络的适配器。  
  
8.  在 "**网络适配器**" 页上的 "**选择用于对 ip-https 连接进行身份验证的证书**" 页上，单击 "**浏览**" 查找并选择 ip-https 证书。 单击“下一步”。  
  
9. 如果在企业网络中配置了 IPv6，则在 "**前缀配置**" 页上的 "**分配给客户端计算机的 ipv6 前缀**" 中，输入一个 Ip-https 前缀以将 Ipv6 地址分配到 DirectAccess 客户端计算机，然后单击 "**下一步**"。  
  
10. 在 "**客户端支持**" 页上，执行下列操作之一：  
  
    -   若要限制对运行 Windows 8 或更高版本操作系统的客户端计算机的访问，请单击 "**限制访问运行 windows 8 或更高版本操作系统的客户端计算机**"，然后单击 "**下一步**"。  
  
    -   若要允许运行 Windows 7 的客户端计算机访问此入口点，请单击 "**允许运行 windows 7 的客户端计算机访问此入口点**"，然后单击 "**添加**"。 在 "**选择组**" 对话框中，选择包含将连接到此入口点的 Windows 7 客户端计算机的安全组，单击 **"确定**"，然后单击 "**下一步**"。  
  
11. 在 "**客户端 GPO 设置**" 页上，接受此入口点的适用于 Windows 7 客户端计算机的默认 gpo，键入你希望远程访问自动创建的 gpo 的名称，或单击 "**浏览**" 查找适用于 Windows 7 客户端计算机的 gpo，然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > -   仅当你将入口点配置为允许 Windows 7 客户端计算机访问入口点时，才会显示 "**客户端 GPO 设置**" 页。  
    > -   您可以选择单击 "**验证 gpo** " 以确保您对此入口点的选定 gpo 具有适当的权限。 如果 GPO 不存在并且将自动创建，则需要创建和链接权限。 如果已手动创建 Gpo，则需要编辑、修改安全性和删除权限。  
  
12. 在 "**服务器 GPO 设置**" 页上，接受此远程访问服务器的默认 gpo，键入要自动创建远程访问的 gpo 的名称，或者单击 "**浏览**" 找到此服务器的 gpo，然后单击 "**下一步**"。  
  
13. 在 "**网络位置服务器**" 页上，单击 "**浏览**" 为在远程访问服务器上运行的网络位置服务器网站选择证书，然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > 仅当网络位置服务器网站在远程访问服务器上运行时，才会显示 "**网络位置服务器**" 页。  
  
14. 在 "**摘要**" 页上，查看入口点设置，然后单击 "**提交**"。  
  
15. 在 "**添加入口点**" 对话框中，单击 "**关闭**"，然后在 "添加入口点" 向导中，单击 "**关闭**"。  
  
    > [!NOTE]  
    > 如果添加的入口点与现有入口点或客户端计算机在不同的林中，则必须在 "**任务**" 窗格中单击 "**刷新管理服务器**" 才能发现域控制器和 System Center新林中的 Configuration Manager。  
  
16. 对于要添加到多站点部署的每个入口点，从步骤2中重复执行此过程。  
  
@no__t 0Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要将 corp2 域中的计算机 edge2 添加为名为 Edge2-欧洲的第二个入口点。 入口点配置是：客户端 IPv6 前缀 "2001： db8：2：2000：/64"、"连接到地址" （edge2 计算机上的 ip-https 证书） "edge2.contoso.com"、名为 "DirectAccess 服务器设置-Edge2-欧洲" 的服务器 GPO 以及内部和分别名为 Internet 和 Corpnet2 的外部接口：  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
若要允许 Windows 7 客户端计算机通过安全组 DA_Clients_Europe 访问第二个入口点，并使用 GPO DA_W7_Clients_GPO_Europe。  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 2：配置多站点基础结构 @ no__t

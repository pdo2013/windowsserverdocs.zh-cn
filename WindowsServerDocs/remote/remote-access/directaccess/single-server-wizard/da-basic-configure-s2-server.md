---
title: 步骤2配置基本 DirectAccess 服务器
description: 本主题是使用 Windows Server 2016 的入门向导部署单个 DirectAccess 服务器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a21e5799824c968b29c719585ca16b6b45a9ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404915"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>步骤2配置基本 DirectAccess 服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何配置基本 DirectAccess 部署所需的客户端和服务器设置。 在开始执行部署步骤之前，请确保已完成[规划基本 DirectAccess 部署](Plan-a-Basic-DirectAccess-Deployment.md)中所述的规划步骤。  
  
|任务|描述|  
|----|--------|  
|安装远程访问角色|安装远程访问角色。|  
|使用开始向导配置 DirectAccess|新开始向导大大简化了配置体验。 该向导掩饰了 DirectAccess 的复杂性，在几个简单步骤内允许自动安装。 该向导通过自动配置 Kerberos 代理消除内部 PKI 部署需要，为管理员提供无缝体验。|  
|使用 DirectAccess 配置更新客户端|若要接收 DirectAccess 设置，连接到 Intranet 时客户端必须更新组策略。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Role"></a>安装远程访问角色  
若要部署远程访问，你必须将远程访问角色安装在组织中即将充当远程访问服务器的服务器上。  
  
#### <a name="to-install-the-remote-access-role"></a>安装远程访问角色  
  
1.  在远程访问服务器上的 "服务器管理器" 控制台的 "**仪表板**" 中，单击 "**添加角色和功能**"。  
  
2.  单击 **“下一步”** 三次以打开服务器角色选择屏幕。  
  
3.  在“选择服务器角色” 对话框中选择“远程访问”，然后单击“下一步”。  
  
4.  在“选择功能”对话框上，单击“下一步”。  
  
5.  单击 "**下一步**"，然后在 "**选择角色服务**" 对话框中，单击 " **DirectAccess 和 VPN （RAS）** " 复选框。  
  
6.  单击 "**添加功能**"，单击 "**下一步**"，然后单击 "**安装**"。  
  
7.  在“安装进度”对话框中，验证安装是否成功，然后单击“关闭”。  
  
@no__t 0Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>Windows powershell 等效命令</em>***  
  
以下 Windows PowerShell cmdlet 或 cmdlet 安装远程访问角色： 

1. 以管理员身份打开 PowerShell。

2. 安装远程访问功能：

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. 重新启动计算机：

   ```
   Restart-Computer
   ```
   
4. 安装远程访问 PowerShell：

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>使用开始向导配置 DirectAccess  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>使用开始向导配置 DirectAccess 的步骤  
  
1.  在服务器管理器中单击 **“工具”** ，然后单击 **“远程访问管理”** 。  
  
2.  在远程访问管理控制台中，在左侧导航窗格中选择要配置的角色服务，然后单击 "**运行入门向导"** 。  
  
3.  单击 **“仅部署 DirectAccess”** 。  
  
4.  选择网络配置拓扑，并键入远程访问客户端将连接到的公用名称。 单击“下一步”。  
  
    > [!NOTE]  
    > 默认情况下，通过将 WMI 筛选器应用到客户端设置 GPO，开始向导将 DirectAccess 部署到域中所有便携式计算机和笔记本计算机。  
  
5.  单击 **“完成”** 。  
  
6.  因为在此部署中未使用 PKI，所以如果找不到证书，则向导将自动为 IP-HTTPS 和网络位置服务器准备自签名证书，并且将自动启用 Kerberos 代理。 该向导还将为仅 IPv4 环境中的协议转换启用 NAT64 和 DNS64。 向导成功完成应用配置后，单击 **“关闭”** 。  
  
7.  在“远程访问管理”控制台的控制台树中，选择 **“操作状态”** 。 等待，直到所有监视器的状态都显示为“正在工作”。 在“监视”下的“任务”窗格中，单击 **“刷新”** 定时更新显示。  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>使用 DirectAccess 配置更新客户端  
  
#### <a name="to-update-directaccess-clients"></a>更新 DirectAccess 客户端的步骤  
  
1.  以管理员身份打开 PowerShell。  
  
2.  在 PowerShell 窗口中，键入 **gpupdate**，然后按 **Enter**。  
  
3.  等待计算机策略更新成功完成。  
  
4.  输入 **Get-DnsClientNrptPolicy**，然后按 **Enter**  
  
    会显示 DirectAccess 的名称解析策略表 (NRPT) 条目。 请注意，会显示 NLS 服务器免除。 开始向导自动为 DirectAccess 服务器创建此 DNS 条目，并准备相关自签名证书，以便 DirectAccess 服务器可以发挥网络位置服务器功能。  
  
5.  输入 **Get-NCSIPolicyConfiguration**，然后按 **Enter**。 会显示向导部署的网络连接状态指示器设置。 记录 DomainLocationDeterminationURL 的值。 只要可以访问此网络位置服务器 URL，客户端就会确定它是否在公司网络内部，且不会应用 NRPT 设置。  
  
6.  输入 **Get-DAConnectionStatus**，然后按 **Enter**。 因为客户端可以访问网络位置服务器 URL，状态会显示为 **“已本地连接”** 。  
  
## <a name="BKMK_Links"></a>上一步  
  
-   [步骤 1：配置 DirectAccess 基础结构](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>下一步  
  
-   [步骤3验证基本 DirectAccess 部署](da-basic-configure-s3-verify.md)  
  



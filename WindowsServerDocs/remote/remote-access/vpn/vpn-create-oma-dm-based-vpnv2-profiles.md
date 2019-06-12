---
title: 向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件
description: '可以使用两种方法来创建 OMA DM 基于 VPNv2 配置文件。 '
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 9051a5b4dc8055885bdc1f8f727514b6e049d74d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749508"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>步骤 7.5： 创建 OMA DM 基于 VPNv2 到 Windows 10 设备的配置文件

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 步骤 7.4：将条件性访问的根证书部署到的本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**下一步：** 了解 VPN 的工作原理的条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

在此步骤中，您可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署的 VPN 设备配置策略。 如果你想要使用 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)的更多详细信息。 

## <a name="managed-deployment-using-intune"></a>使用 Intune 的托管的部署

在本部分中介绍的所有内容是使 VPN 使用条件性访问所需的最低要求。 它不涵盖拆分隧道，使用 WIP，创建自定义的 Intune 设备配置文件以获取 AutoVPN 工作或 SSO。 将下面的设置集成到在前面创建的 VPN 配置文件[步骤 5。配置 Windows 10 客户端 Always On VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)。  在此示例中，我们会将它们集成到[通过使用 Intune 配置 VPN 客户端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)策略。 

**先决条件：**

已使用 Intune 的 VPN 连接配置 Windows 10 客户端计算机。   


**过程：**

1. 在 Azure 门户中，选择**Intune** > **设备配置** > **配置文件**选择在前面创建的VPN配置文件[使用 Intune 配置 VPN 客户端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)。
    
2. 在策略编辑器中，选择**属性** > **设置** > **基础 VPN**。 扩展的现有**EAP Xml**包括使 VPN 客户端逻辑的筛选器它需要从用户的证书存储，而不是将它保留为人使其可以使用第一个检索 AAD 条件性访问证书发现的证书。

    >[!NOTE]
    >如果没有，VPN 客户端无法检索用户证书从本地证书颁发机构颁发，导致失败的 VPN 连接。

    ![Intune 门户](../../media/Always-On-Vpn/intune-eap-xml.png)

3. 找到结尾的部分 **\</AcceptServerName >\</EapType >** 并插入以下字符串之间的 VPN 客户端提供用于选择 AAD 条件的逻辑这两个值访问证书：

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. 选择**条件访问**边栏选项卡并切换**此 VPN 连接的条件性访问**到**已启用**。
   
   启用此设置更改 **\<DeviceCompliance >\<已启用 > true\<启用 >** VPNv2 配置文件 XML 中的设置。

    ![Always On VPN-属性的条件性访问](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. 选择“确定”  。

6. 选择**分配**，在包括中下选择**选择要包括的组**。

7. 选择**VPN 用户**组中接收此策略，然后选择**保存**。

    ![自动 VPN 用户-分配的上限](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>在客户端上强制 MDM 策略同步

如果 VPN 配置文件未出现在客户端设备上，在设置下\\网络和 Internet\\VPN，可以强制 MDM 策略进行同步。

1. 作为的成员登录到已加入域的客户端计算机**VPN 用户**组。

2. 在开始菜单中，输入**帐户**，然后按 Enter。

3. 在左侧的导航窗格中，选择**访问工作或学校**。

4. 在访问工作单位或学校，选中**连接到 < \domain > MDM**，然后选择**信息**。

5. 选择**同步**，并验证的 VPN 配置文件将显示在设置下\\网络和 Internet\\VPN。


## <a name="next-steps"></a>后续步骤

完成配置要使用 Azure AD 条件访问的 VPN 配置文件。 

|如果想要...  |然后请参阅...  |
|---------|---------|
|了解有关条件性访问的工作原理与 Vpn 的详细信息  |[VPN 和条件性访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):此页提供有关条件性访问的详细信息适用于 Vpn。      |
|了解有关高级 VPN 功能的详细信息  |[高级 VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features):此页提供有关如何启用 VPN 流量筛选器、 如何配置自动 VPN 连接使用应用的触发器，以及如何将 NPS 配置为仅允许使用由 Azure AD 颁发的证书的客户端的 VPN 连接的指南。        |


## <a name="related-topics"></a>相关主题

- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):本主题为您提供的 VPNv2 CSP 概述。 VPNv2 配置服务提供程序允许移动设备管理 (MDM) 服务器配置的设备的 VPN 配置文件。

- [配置 Windows 10 客户端 Always On VPN 连接](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections):本主题提供有关 ProfileXML 选项和架构信息以及如何创建 ProfileXML VPN。 设置后的服务器基础结构，必须配置 Windows 10 客户端计算机与该基础结构与 VPN 连接进行通信。 

- [使用 Intune 配置 VPN 客户端](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune):本主题提供有关如何部署 Windows 10 的远程访问始终在 VPN 配置文件信息。 Intune 现在使用 Azure AD 组。 如果 Azure AD Connect 同步到 Azure AD 中从本地的 VPN 用户组，则无需配置 VPN 客户端使用 Intune。

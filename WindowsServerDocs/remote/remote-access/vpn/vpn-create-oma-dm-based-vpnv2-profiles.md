---
title: 向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件
description: '你可以使用两种方法，以创建 OMA DM 基于 VPNv2 配置文件。 '
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
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031291"
---
# 步骤 7.5： 创建基于 OMA-DM 的 VPNv2 配置文件到 Windows 10 设备

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 步骤 7.4。将条件访问根证书部署到本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187;[**下一步：** 了解如何条件访问 VPN 的工作原理](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

在此步骤中，你可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署 VPN 设备配置策略。 如果你想要使用 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅更多详细信息[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)。 

## 使用 Intune 托管的部署

本部分中讨论的所有内容都是使 VPN 使用条件访问所需的最低。 它不能在拆分隧道，使用 WIP 创建自定义 Intune 设备配置文件以获取 AutoVPN 工作或 SSO。 将下面的设置集成到你之前创建下[步骤 5 的 VPN 配置文件。配置 Windows 10 客户端始终启用 VPN 连接](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)。在此示例中，我们会将它们集成到[配置 VPN 客户端使用 Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)策略。 

**先决条件：**<p>
使用 Intune 的 VPN 连接与已配置 Windows 10 客户端计算机。   


**步骤：**

1. 在 Azure 门户中，单击**Intune** > **设备配置** > 更早版本中[配置 VPN 客户端使用 Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)创建**配置文件**，然后选择的 VPN 配置文件。
    
2. 在策略编辑器中，选择**属性** > **设置** > **基本 VPN**。 扩展现有**EAP Xml**要包含的筛选器为 VPN 客户端提供它需要从而不是将其保留为机会允许它使用的第一个证书的用户的证书存储中检索 AAD 条件访问证书的逻辑发现。

    >[!NOTE]
    >没有它，VPN 客户端可以检索用户证书从内部证书颁发机构颁发，导致失败的 VPN 连接。

    ![Intune 门户](../../media/Always-On-Vpn/intune-eap-xml.png)

3. 找到结尾**\</AcceptServerName>\</EapType>** 部分，然后插入 VPN 客户端提供的逻辑选择 AAD 条件访问证书这两个值之间的以下字符串：

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. 选择的**条件访问**边栏选项卡和 toogle**已启用****此 VPN 连接条件访问**。<p>启用此设置更改**\<DeviceCompliance>\<Enabled>true\</Enabled>** VPNv2 配置文件 xml 设置。

    ![始终启用 VPN 的属性的条件访问](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. 单击**确定**。

6. 选择下的**分配**，包括，单击**选择要包括的组**。

7. 选择**VPN 用户**组中接收此策略，然后单击**保存**。

    ![自动 VPN 用户-分配的上限](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## 在客户端上强制执行 MDM 策略同步
如果 VPN 配置文件不会不会显示在客户端设备上，在 Settings\\Network & Internet\\VPN，你可以强制 MDM 策略以同步。

1. **VPN 用户**组的成员身份登录到已加入域的客户端计算机。

2. 在开始菜单中，键入**帐户**，然后按 Enter。

3.  在左侧的导航窗格中，单击**访问工作单位或学校**。

5.  在访问工作单位或学校下，单击**连接到 <\domain> MDM**并单击**信息**。

6.  单击**同步**并验证 VPN 配置文件显示在 Settings\\Network & Internet\\VPN 下。


## 下一步
你已完成配置要使用 Azure AD 的条件访问权限的 VPN 配置文件。 

|如果你想要...  |请参阅...  |
|---------|---------|
|了解有关如何条件访问适用于 Vpn  |[VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 本页提供了有关如何条件访问的详细信息的工作原理与 Vpn。      |
|了解有关高级 VPN 功能的详细信息  |[高级 VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features)： 此页提供有关如何启用 VPN 流量筛选器、 如何配置自动 VPN 连接使用应用的触发器，以及如何将 NPS 配置为仅使用 Azure 颁发的证书从客户端允许 VPN 连接的指南广告。        |


---

## 相关主题
- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)： 本主题提供了 VPNv2 CSP 的概述。 VPNv2 配置服务提供程序允许移动设备管理 (MDM) 服务器配置设备的 VPN 配置文件。

- [配置 Windows 10 客户端始终在 VPN 连接](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)： 本主题提供有关 ProfileXML 选项和架构，信息以及如何创建 ProfileXML VPN。 设置服务器基础结构之后, 你必须配置 windows 10 客户端计算机与通过 VPN 连接的基础结构进行通信。 

- [配置 VPN 客户端使用 Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune)： 本主题提供有关如何部署 Windows 10 远程访问始终启用 VPN 配置文件信息。 现在，Intune 使用 Azure AD 组。 如果 Azure AD Connect 同步到 Azure AD VPN 用户组中的本地，则无需配置使用 Intune 的 VPN 客户端。

---

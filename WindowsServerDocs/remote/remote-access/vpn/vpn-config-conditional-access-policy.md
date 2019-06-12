---
title: 配置条件访问策略
description: 创建根证书后，VPN 连接触发客户的租户中的 VPN 服务器云应用程序的创建。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 466e76d01ca99a1e1ed72fa955ccd287ae63c5df
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749500"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>步骤 7.3： 配置条件性访问策略

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

- [**上一：** 步骤 7.2：使用 Azure AD 创建 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**下一步：** 步骤 7.4：将条件性访问的根证书部署到的本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步骤中，你可以配置针对 VPN 连接的条件性访问策略。 在 VPN 连接边栏选项卡中创建第一个根证书后，会自动在租户中创建的 VPN 服务器云应用程序。

创建条件性访问策略分配给 VPN 用户组和作用域**云应用程序**到**VPN 服务器**:

- **Users**：VPN 用户
- **云应用**:VPN 服务器
- **Grant （访问控制）** :需要多重身份验证。 如果需要，可以使用其他控件。

**过程：** 此步骤介绍如何创建最基本的条件性访问策略。  如果需要，可以使用其他条件和控制。


1. 上**条件访问**页上，在顶部工具栏中的选择**添加**。

    ![选择添加条件性访问页面上](../../media/Always-On-Vpn/07.png)

2. 上**新建**页上，在**名称**框中，输入你的策略的名称。 例如，输入**VPN 策略**。

    ![条件性访问页面上添加策略名称](../../media/Always-On-Vpn/08.png)

3. 在中**赋值**部分中，选择**用户和组**。

    ![选择用户和组](../../media/Always-On-Vpn/09.png)

4. 上**用户和组**页上，执行以下步骤：

    ![选择测试用户](../../media/Always-On-Vpn/10.png)

    a. 选择**选择用户和组**。

    b. 选择**选择**。

    c. 上**选择**页上，选择**VPN 用户**组，然后再选择**选择**。

    d. 上**用户和组**页上，选择**完成**。

5. 上**新建**页上，执行以下步骤：

    ![选择云应用](../../media/Always-On-Vpn/11.png)

    a. 在中**分配**部分中，选择**云应用**。

    b. 上**云应用**页上，选择**选择应用**。

    d. 选择**VPN 服务器**。

6.  上**新建**页面上，以打开**授予**页上，在**控件**部分中，选择**授予**。

    ![选择授权](../../media/Always-On-Vpn/13.png)

7.  上**授予**页上，执行以下步骤：

    ![选择需要多重身份验证](../../media/Always-On-Vpn/14.png)

    a. 选择**要求多重身份验证**。

    b. 选择**选择**。

8.  上**新建**页面上，在**启用策略**，选择**上**。

    ![启用策略](../../media/Always-On-Vpn/15.png)

9.  上**新建**页上，选择**创建**。


## <a name="next-steps"></a>后续步骤
[步骤 7.4.将条件性访问的根证书部署到的本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md):在此步骤中，将部署条件性访问的根证书作为 VPN 身份验证的受信任的根证书到你的本地 AD。

---
title: 配置条件访问策略
description: 创建根证书后，"VPN 连接" 会触发在客户的租户中创建 "VPN 服务器" 云应用程序。
services: active-directory
ms.prod: windows-server
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
ms.openlocfilehash: 22983c085f2b9d9e7e16810e25c6fa50111f9fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404344"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>步骤 7.3： 配置条件访问策略

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**以前**步骤 7.2：使用 Azure AD 创建 VPN 身份验证的根证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**一个**步骤 7.4：将条件性访问根证书部署到本地 AD @ no__t-0

在此步骤中，你将为 VPN 连接配置条件访问策略。 在 "VPN 连接" 边栏选项卡中创建第一个根证书时，它会自动在租户中创建 "VPN 服务器" 云应用程序。

创建一个条件性访问策略，该策略分配给 VPN 用户组，并将**云应用**范围限定为**vpn 服务器**：

- **Users**：VPN 用户
- **云应用**：VPN 服务器
- **授予（访问控制）** ："需要多重身份验证"。 如果需要，可以使用其他控件。

**方法**此步骤介绍如何创建最基本的条件性访问策略。  如果需要，还可以使用其他条件和控件。


1. 在 "**条件性访问**" 页顶部的工具栏中，选择 "**添加**"。

    ![选择 "条件访问时添加" 页](../../media/Always-On-Vpn/07.png)

2. 在 "**新建**" 页上的 "**名称**" 框中，输入策略的名称。 例如，输入 " **VPN 策略**"。

    ![为条件访问页上的策略添加名称](../../media/Always-On-Vpn/08.png)

3. 在 "**分配**" 部分，选择 "**用户和组**"。

    ![选择用户和组](../../media/Always-On-Vpn/09.png)

4. 在 "**用户和组**" 页上，执行以下步骤：

    ![选择测试用户](../../media/Always-On-Vpn/10.png)

    a. 选择 "**选择用户和组**"。

    b. 选择 "**选择**"。

    c. 在 "**选择**" 页上，选择 " **VPN 用户**" 组，然后选择 "**选择**"。

    d. 在 "**用户和组**" 页上，选择 "**完成**"。

5. 在 "**新建**" 页上，执行以下步骤：

    ![选择云应用](../../media/Always-On-Vpn/11.png)

    a. 在 "**分配**" 部分，选择 "**云应用**"。

    b. 在 "**云应用**" 页上，选择 "**选择应用**"。

    d. 选择 " **VPN 服务器**"。

6.  在 "**新建**" 页上，若要打开 "**授予**" 页，请在 "**控件**" 部分选择 "**授予**"。

    ![选择授予](../../media/Always-On-Vpn/13.png)

7.  在 "**授予**" 页上，执行以下步骤：

    ![选择 "需要多重身份验证"](../../media/Always-On-Vpn/14.png)

    a. 选择 "**需要多重身份验证**"。

    b. 选择 "**选择**"。

8.  在 "**新建**" 页上的 "**启用策略**" 下，选择 **"打开"** 。

    ![启用策略](../../media/Always-On-Vpn/15.png)

9.  在 "**新建**" 页上，选择 "**创建**"。


## <a name="next-steps"></a>后续步骤
[步骤 7.4.将条件性访问根证书部署到本地 AD @ no__t-0：在此步骤中，将条件访问根证书部署为本地 AD 的 VPN 身份验证的受信任根证书。

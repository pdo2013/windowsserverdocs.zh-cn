---
title: 配置条件访问策略
description: 在创建根证书后，VPN 连接触发 VPN 服务器云应用程序在客户租户的创建。
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
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067027"
---
# 步骤 7.3： 配置条件访问策略

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**以前：** 7.2 步骤。与 Azure AD 中创建 VPN 身份验证的根的证书](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187;[**下一步：** 7.4 步骤。将条件访问根证书部署到本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步骤中，你将配置 VPN 连接的条件访问策略。 在 VPN 连接边栏选项卡中创建的第一个根证书时，会自动在租户中创建的 VPN 服务器云应用程序。 

创建条件访问策略分配给 VPN 用户组和范围**云应用**到**VPN 服务器**： 

- **用户**： VPN 用户
- **云应用**： VPN 服务器
- **授权 （访问控制）**: 需要多重身份验证。 如果需要，可以使用其他控件。

**过程：** 此步骤介绍了创建最基本的条件访问策略。如有需要，可以使用其他条件和控件。


1. **条件访问**在页面上，在顶部工具栏上，单击**添加**。

    ![条件访问页面上选择添加](../../media/Always-On-Vpn/07.png)

2. 在**新建**页面上，在**名称**框中，键入你的策略的名称。 例如，键入**VPN 策略**。

    ![条件访问页面上添加策略名称](../../media/Always-On-Vpn/08.png)

3. 在**分配**部分中，单击**用户和组**。

    ![选择用户和组](../../media/Always-On-Vpn/09.png)

4. 在**用户和组**页面上，执行以下步骤：

    ![选择测试用户](../../media/Always-On-Vpn/10.png)

    a. 单击**选择用户和组**。

    b. 单击**选择**。

    c. 在**选择**页面上，选择**VPN 用户**组中，，然后单击**选择**。

    d. 在**用户和组**页面上，单击**完成**。

5. 在**新建**页面上，执行以下步骤：

    ![选择云应用](../../media/Always-On-Vpn/11.png)

    a. 在**分配**部分中，单击**云应用**。

    b. 在**云应用**页上，单击**选择应用**。

    d. 选择**VPN 服务器**。

13. 在**新建**页面上，若要打开**授予**页面中，**控件**部分中，单击**授予**。

    ![选择授予](../../media/Always-On-Vpn/13.png)

14. 在**授予**页上，执行以下步骤：

    ![选择需要多重身份验证](../../media/Always-On-Vpn/14.png)

    a. 选择**需要多重身份验证**。

    b. 单击**选择**。

15. **在**新建**页面上**启用策略**，请单击。**

    ![启用策略](../../media/Always-On-Vpn/15.png)

16. 在**新建**页面上，单击**创建**。


## 下一步
[步骤 7.4。将条件访问根证书部署到本地 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)： 在此步骤中，你将部署的条件访问根证书作为受信任的根证书进行 VPN 身份验证到你的本地 AD。

---
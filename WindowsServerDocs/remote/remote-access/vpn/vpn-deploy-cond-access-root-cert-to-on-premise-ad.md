---
title: 将条件访问根证书部署到本地 AD
description: ''
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
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067030"
---
# 步骤 7.4： 将条件访问根证书部署到本地 AD

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

在此步骤中，你将部署的条件访问根证书作为受信任的根证书进行 VPN 身份验证到你的本地 AD。

& #171; [**以前：** 7.3 步骤。配置条件访问策略](vpn-config-conditional-access-policy.md)<br>
& #187;[**下一步：** 7.5 步骤。创建 OMA DM 基于 VPNv2 配置文件到 Windows 10 设备](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. 在**VPN 连接**页上，单击**下载证书**。 
   
    ![下载证书进行条件访问](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >**下载 base64 证书**选项是可用于某些部署需要 base64 证书的配置。 

2. 登录到已加入域的计算机使用企业管理员权限，并将云根证书添加到*企业 NTauth*存储管理员命令提示符运行以下命令：

    >[!NOTE]
    >对于 VPN 服务器未加入到 Active Directory 域的环境，云根证书必须将添加到_受信任的根证书颁发机构_存储手动。

    |命令  |说明  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |创建两个**Microsoft VPN 根 CA 第 1 代**容器下的**CN = AIA**和**CN = 证书颁发机构**容器，并将每个根证书发布为两个**Microsoft VPN 根的_cACertificate_属性的值第 1 代 CA**容器。|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |创建一个**CN = NTAuthCertificates**下的容器**CN = AIA**和**CN = 证书颁发机构**容器，并将每个根证书发布**CN 的_cACertificate_属性的值为 =NTAuthCertificates**容器。 |  
    |`gpupdate /force`     |加快将根证书添加到 Windows server 和客户端计算机。  |

3.  验证存在企业 NTauth 应用商店中显示为受信任的根证书：

    a.  登录到的具有企业管理员权限的服务器上已安装的**证书颁发机构管理工具**。

    >[!NOTE]
    >默认情况下，**证书颁发机构管理工具**是已安装的证书颁发机构服务器。 它们可以在服务器上安装其他成员**角色管理工具**在服务器管理器的一部分。

    b.  在 VPN 服务器上，在开始菜单中，键入**pkiview.msc**即可打开企业 PKI 对话框。

    c.  从开始菜单中，键入**pkiview.msc**即可打开企业 PKI 对话框。

    d.  右键单击**企业 PKI** ，然后选择**管理 AD 容器**。

    d.  验证每个 Microsoft VPN 根 CA 第 1 代证书存在下：<ul><li>NTAuthCertificates</li><li>AIA 容器</li><li>证书颁发机构容器</li></ul>

    
## 下一步
[步骤 7.5。创建 OMA DM 基于 VPNv2 配置文件到 Windows 10 设备](vpn-create-oma-dm-based-vpnv2-profiles.md)： 在此步骤中，你可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署 VPN 设备配置策略。 如果你希望到 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅更多详细信息[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)。

---

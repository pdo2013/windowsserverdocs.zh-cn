---
title: 将条件访问根证书部署到本地 AD
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 200d3b96ee24b5e1264b4bf2e42d636f9e07fbef
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469682"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>步骤 7.4： 将条件性访问的根证书部署到的本地 AD

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

在此步骤中，将部署条件性访问的根证书作为 VPN 身份验证的受信任的根证书到你的本地 AD。

- [**上一：** 步骤 7.3：配置条件访问策略](vpn-config-conditional-access-policy.md)
- [**下一步：** 步骤 7.5：向 Windows 10 设备创建基于 OMA-DM 的 VPNv2 配置文件](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. 上**VPN 连接**页上，选择**下载证书**。

   >[!NOTE]
   >**下载 base64 证书**选项仅适用于某些需要 base64 证书进行部署的配置。

2. 登录到使用企业管理员权限和运行这些命令从管理员命令提示符添加云根到证书的已加入域的计算机*Enterprise NTauth*存储：

   >[!NOTE]
   >对于 VPN 服务器未加入到 Active Directory 域的环境，必须将云的根证书添加到_受信任的根证书颁发机构_手动存储。

   | Command | 描述 |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | 创建两个**Microsoft VPN 根 CA 第 1 代**下的容器**CN = AIA**并**CN = 证书颁发机构**容器，并将每个根证书作为值发布上_cACertificate_属性的同时**Microsoft VPN 根 CA 第 1 代**容器。 |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | 将创建一个**CN = NTAuthCertificates**下容器**CN = AIA**并**CN = 证书颁发机构**容器，并将每个根证书作为值发布上_cACertificate_的属性**CN = NTAuthCertificates**容器。 |
   | `gpupdate /force` | 加快了将根证书添加到 Windows 服务器和客户端计算机。 |

3. 验证位于企业 NTauth 存储和显示为受信任的根证书：
   1. 登录到了具有企业管理员权限的服务器**证书颁发机构管理工具**安装。

   >[!NOTE]
   >默认情况下**证书颁发机构管理工具**是已安装的证书颁发机构服务器。 它们可以在服务器上安装其他成员作为的一部分**角色管理工具**在服务器管理器。

   1. 在 VPN 服务器上，在开始菜单中，输入**pkiview.msc**以打开企业 PKI 对话框。
   1. 从开始菜单中，输入**pkiview.msc**以打开企业 PKI 对话框。
   1. 右键单击**企业 PKI** ，然后选择**管理 AD 容器**。
   1. 验证每个 Microsoft VPN 根 CA 第 1 代证书存在下：
      - NTAuthCertificates
      - AIA 容器
      - 证书颁发机构容器

## <a name="next-steps"></a>后续步骤

[步骤 7.5.创建到 Windows 10 设备的 OMA DM 基于 VPNv2 配置文件](vpn-create-oma-dm-based-vpnv2-profiles.md):在此步骤中，您可以创建 OMA DM 基于 VPNv2 配置文件使用 Intune 部署的 VPN 设备配置策略。 如果您想 SCCM 或 PowerShell 脚本创建 VPNv2 配置文件，请参阅[VPNv2 CSP 设置](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)的更多详细信息。

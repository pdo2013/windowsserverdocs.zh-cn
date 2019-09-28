---
title: 启用 OTP 疑难解答
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1c18f264a6a8d263f3e9f50bc325ef97f4240af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366924"
---
# <a name="troubleshooting-enabling-otp"></a>启用 OTP 疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题包含与使用**DAOtpAuthentication** PowerShell Cmdlet 或远程访问管理控制台启用 DirectAccess OTP 身份验证相关的问题的疑难解答信息。
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>未能注册 OTP 签名证书  
**收到错误**（服务器事件日志）。 无法使用证书模板 < OTP_signing_template_name 注册 OTP 签名证书 >  
  
**原因**  
  
导致此错误的可能原因有三个：  
  
-   模板不存在。  
  
-   对模板设置的权限不允许 DirectAccess 服务器进行注册。  
  
-   与证书颁发机构（CA）之间没有网络连接。  
  
**解决方案**  
  
1.  请确保具有给定名称的 OTP 签名证书模板为：  
  
    1.  存在并且具有正确的权限。  
  
    2.  设置为由至少一个可向 DirectAccess 服务器颁发证书的 CA 颁发。  
  
2.  如果模板不存在，请根据3.3 计划注册颁发机构证书中所述创建它，如果存在其他匹配的模板，则请重新配置 DirectAccess OTP 和新模板名称。  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>安装 WebDAV 后未能启用 DirectAccess OTP  
**方案**。 尝试在远程访问管理控制台中或通过使用 `Enable-DAOtpAuthentication` PowerShell cmdlet 应用 DirectAccess OTP 配置时，操作将失败。  
  
**收到错误**（服务器事件日志）。 无法应用 DirectAccess OTP 设置，因为 WebDAV IIS 扩展正在服务器上运行。 删除 WebDAV 并再次应用这些设置。  
  
**原因**  
  
DirectAccess OTP 服务与 WebDAV 发布功能不兼容，安装了 WebDAV 时无法启用。  
  
**解决方案**  
  
卸载 WebDAV 角色：  
  
1.  在服务器管理器控制台中，单击左窗格中的 " **IIS**"。  
  
2.  在主窗格中，滚动到 "**角色和功能**"。  
  
3.  右键单击 " **WebDAV 发布**"，然后单击 "**删除角色或功能**"。  
  
4.  完成 "删除角色和功能向导"。  
  
5.  重新应用 DirectAccess OTP 配置。  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>远程访问管理控制台中没有可用的模板  
**方案**。 虽然使用远程访问管理控制台配置 OTP 或注册机构证书模板，但某些或所有模板从选择窗口中丢失。  
  
**原因**  
  
导致此错误的可能原因有两个：  
  
-   未根据 DirectAccess OTP 要求配置模板，因此无法选择它。  
  
-   未将**OTP CA 服务器**下的选定 ca 配置为发出所需的模板。  
  
**解决方案**  
  
1.  请确保正确配置了 "otp 登录模板" 和 "OTP 签名证书" 模板，如3.2 计划 OTP 证书模板和3.3 中所述。  
  
2.  请确保已将**OTP Ca 服务器**列表中配置的 ca 配置为颁发相关模板：  
  
    1.  在 CA 服务器上，打开 "证书颁发机构" 控制台。  
  
    2.  在左侧窗格中，展开所选的 CA 服务器。  
  
    3.  单击 "**证书模板**"，并确保已启用所需模板。 如果不是，请右键单击 "**证书模板**"，单击 "**新建**"，单击 "**要颁发的证书模板**"，然后选择要启用的模板。  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>无法将 OTP 模板的续订期设置为1小时  
**方案**。 使用 Windows 2003 CA 配置 DirectAccess OTP 登录模板时，不能将模板的续订期设置为1小时。  
  
**原因**  
  
Windows Server 2003 中的 "证书模板" MMC 管理单元不允许你将模板的续订期设置为1小时。  
  
**解决方案**  
  
在 Windows Server 2003 服务器上安装证书模板管理单元，并使用它来配置 OTP 登录模板，请参阅[安装证书模板管理单元](https://technet.microsoft.com/library/cc732445.aspx)。  
  



---
title: 启用 OTP 疑难解答
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55314f3bd5e3500847beed256580b1924521abc9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280798"
---
# <a name="troubleshooting-enabling-otp"></a>启用 OTP 疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍与启用 DirectAccess OTP 身份验证使用相关的问题的疑难解答信息**启用 DAOtpAuthentication** PowerShell cmdlet 或远程访问管理控制台。
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>未能注册 OTP 签名证书  
**收到错误**（服务器事件日志）。 不能使用证书模板 < OTP_signing_template_name > 注册 OTP 签名证书  
  
**原因**  
  
有三个可能的原因，此错误：  
  
-   该模板不存在。  
  
-   在模板上设置的权限不允许 DirectAccess 服务器注册。  
  
-   没有网络连接到证书颁发机构 (CA)。  
  
**解决方案**  
  
1.  请确保，OTP 签名证书具有给定名称的模板：  
  
    1.  存在且具有适当权限。  
  
    2.  设置为至少一个可以向 DirectAccess 服务器颁发证书的 ca 颁发。  
  
2.  如果模板不存在，创建它如中所述 3.3 计划注册颁发机构证书，或者如果存在另一个匹配的模板重新 DirectAccess OTP 配置使用新的模板名称。  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>未能安装 WebDAV 时，启用 DirectAccess OTP  
**方案**。 尝试将 DirectAccess OTP 配置远程访问管理控制台中或通过使用应用时`Enable-DAOtpAuthentication`PowerShell cmdlet，则操作将失败。  
  
**收到错误**（服务器事件日志）。 无法应用 DirectAccess OTP 设置，因为 WebDAV IIS 扩展在服务器上运行。 删除 WebDAV 并再次应用设置。  
  
**原因**  
  
DirectAccess OTP 服务与 WebDAV 发布功能不兼容，并且在安装了 WebDAV 后无法启用。  
  
**解决方案**  
  
卸载 WebDAV 角色：  
  
1.  在服务器管理器控制台中，在左窗格中，单击**IIS**。  
  
2.  在主窗格中，向下滚动到**角色和功能**。  
  
3.  右键单击**WebDAV 发布**，然后单击**删除角色或功能**。  
  
4.  完成删除角色和功能向导。  
  
5.  重新应用 DirectAccess OTP 配置。  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>没有在远程访问管理控制台中可用的模板  
**方案**。 虽然从选择 windows 是缺少配置部分，使用远程访问管理控制台中，OTP 或注册颁发机构证书模板，或所有的模板。  
  
**原因**  
  
有两个可能的原因，此错误：  
  
-   模板未配置根据 DirectAccess OTP 要求，因此不能选择。  
  
-   在所选的 Ca **OTP CA 服务器**未配置为发出所需的模板。  
  
**解决方案**  
  
1.  请确保正确 3.2 计划 OTP 证书模板和 3.3 计划注册证书颁发机构证书中所述配置 OTP 登录模板和 OTP 的签名证书模板。  
  
2.  请确保在已配置的 Ca **OTP CA 服务器**列出了配置问题相关的模板：  
  
    1.  在 CA 服务器上，打开证书颁发机构控制台。  
  
    2.  在左窗格中，展开所选的 CA 服务器。  
  
    3.  单击**证书模板**，并确保启用了所需的模板。 如果没有，请右键单击**证书模板**，单击**新建**，单击**要颁发的证书模板**，然后选择你想要启用的模板。  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>不能设置为 1 小时的 OTP 模板的续订期  
**方案**。 在配置 DirectAccess OTP 登录模板使用 Windows 2003 CA 时，不能将该模板的续订期间设置为 1 小时。  
  
**原因**  
  
Windows Server 2003 中的证书模板 MMC 管理单元不允许你将模板的续订期间设置为 1 小时。  
  
**解决方案**  
  
在 Windows Server 2003 服务器上安装证书模板管理单元中并使用它来配置 OTP 登录模板，请参阅[安装证书模板管理单元](https://technet.microsoft.com/library/cc732445.aspx)。  
  



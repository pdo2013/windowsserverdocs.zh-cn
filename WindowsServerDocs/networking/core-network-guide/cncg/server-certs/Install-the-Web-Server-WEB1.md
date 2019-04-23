---
title: 安装 Web 服务器 WEB1
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef4f10a6ac1998850758f2c9db86bfd950c1ad70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833278"
---
# <a name="install-the-web-server-web1"></a>安装 Web 服务器 WEB1

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Windows Server 2016 中的 Web 服务器 (IIS) 角色以可靠地托管网站、 服务和应用程序提供安全、 易于管理、 模块化且可扩展的平台。 使用 IIS，可以共享 Internet、 intranet 或 extranet 上与用户的信息。 IIS 是一个集 IIS、 ASP.NET、 FTP 服务、 PHP 和 Windows Communication Foundation (WCF) 的统一的 web 平台。  

在部署服务器证书时，你的 Web 服务器将为您提供在其中可以为证书颁发机构 (CA) 发布证书吊销列表 (CRL) 的位置。 发布后，CRL 都可以访问你的网络上的所有计算机，以便它们可以身份验证过程中使用此列表以验证提供的其他计算机证书没有被吊销。   

如果证书，则为已撤消在 crl，身份验证工作失败，并从信任的实体将不再有效的证书保护您的计算机。  

在安装 Web 服务器 (IIS) 角色之前，请确保已配置的服务器名称和 IP 地址，并已将计算机加入到域。  

## <a name="to-install-the-web-server-iis-server-role"></a>若要安装 Web 服务器 (IIS) 服务器角色  
若要完成此过程，你必须是**Administrators**组的成员。  

>[!NOTE]  
>若要使用 Windows PowerShell 执行此过程，打开 PowerShell，键入以下命令，然后按 ENTER。  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 将打开“添加角色和功能向导”。  
2.  在“开始之前”中单击“下一步”。  

**请注意**   
**开始之前**添加角色和功能向导的页未显示如果以前运行的添加角色和功能向导并选中**默认情况下跳过此页**在该时间。  

3.  在 **“安装类型”** 页上，单击 **“下一步”**。  
4.  上**服务器选择**页上，单击**下一步**。  
5.  上**服务器角色**页上，选择**Web 服务器 (IIS)**，然后单击**下一步**。  
6.  单击 **“下一步”** 直到接受了所有默认的 web 服务器设置，然后单击 **“安装”**。  
7.  验证是否所有安装都已成功，然后单击“关闭”。

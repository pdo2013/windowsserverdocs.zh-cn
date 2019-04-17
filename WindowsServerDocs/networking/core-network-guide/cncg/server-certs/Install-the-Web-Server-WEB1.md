---
title: 安装 Web 服务器 WEB1
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>安装 Web 服务器 WEB1

>适用于：Windows Server（半年通道），Windows Server 2016

Windows Server 2016 中的 Web 服务器 (IIS) 角色提供适用于可靠举办的网站、 服务和应用程序的安全性、 易于管理、 模块和可扩展平台。 通过 IIS 中，您可以共享与用户 Internet、 intranet，或外部网络上的信息。 IIS 是集成 IIS、 ASP.NET、 FTP 服务、 PHP，以及 Windows 通信基础 (WCF) 统一的 web 平台。  

服务器证书部署时，你的 Web 服务器将为您提供的位置，您可以为你证书颁发机构） 发布证书吊销列表 (CRL)。 发布后，CRL 都可以访问你的网络上的所有计算机，以便他们可以使用此列表的身份验证过程验证证书的其他计算机所示未被吊销。   

如果证书位于 CRL 撤销作为，身份验证努力失败，并且你的计算机免受信任实体具有某个证书存在不会再有效。  

安装的 Web 服务器 (IIS) 角色之前，请确保你有配置服务器的名称和 IP 地址，并且已将计算机连接到域。  

## <a name="to-install-the-web-server-iis-server-role"></a>若要安装的 Web 服务器 (IIS) 服务器角色  
若要完成此过程，你必须**管理员**组。  

>[!NOTE]  
>若要执行此过程，方法是使用 Windows PowerShell，打开 PowerShell，键入以下命令，然后按 ENTER。  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。  
2.  在**开始之前**，单击**下一步**。  

**注意**   
**开始之前**如果你之前已运行添加角色和功能向导和您选择添加角色和功能向导中的页面未显示**默认情况下跳过此页**在该时间。  

3.  在**安装类型**页上，单击**下一步**。  
4.  在**选择服务器**页上，单击**下一步**。  
5.  在**服务器角色**页上，选择**Web 服务器 (IIS)**，然后单击**下一步**。  
6.  单击**下一步**直到接受你的所有默认 web 服务器设置，然后单击**安装**。  
7.  检查所有安装都已成功后，然后单击**关闭**。

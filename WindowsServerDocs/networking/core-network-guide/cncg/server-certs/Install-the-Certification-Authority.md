---
title: 安装证书颁发机构
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>安装证书颁发机构

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程，以便你可以注册网络策略 Server (NPS)、路由并远程访问服务 (RRAS) 或两者正在运行的服务器证书安装 Active Directory 证书服务（广告客户服务）。  
  
> [!IMPORTANT]  
> -   安装 Active Directory 证书服务之前，必须命名计算机，配置计算机的静态 IP 地址，并加入域的计算机。 有关如何来完成这些任务的详细信息，请参阅 Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要执行此步骤，必须到某个域的 Active Directory 域服务 (广告 DS) 装有加入安装广告客户服务的计算机。  
  
在这两会员**企业管理员**和根域**域管理员**组是的最低要求才能完成此过程。  
  
> [!NOTE]  
> 若要执行此过程，方法是使用 Windows PowerShell，打开 Windows PowerShell 和键入以下命令，然后按 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> 广告 C 安装后，键入以下命令，然后按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>若要安装的 Active Directory 证书服务  
  
1.  作为企业管理员组和根域域管理员组成员进行登录。  
  
2.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。  
  
3.  在**开始之前**，单击**下一步**。  
  
    > [!NOTE]  
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。  
  
4.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。  
  
5.  在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。  
  
6.  在**选择服务器角色**中**角色**、选择**Active Directory 证书服务**。 当提示你添加所需的功能时，请单击**添加功能**，然后单击**下一步**。  
  
7.  在**选择功能**，单击**下一步**。  
  
8.  在**Active Directory 证书服务**、阅读所提供的信息，然后单击**下一步**。  
  
9. 在**确认安装选择**，单击**安装**。 在安装过程中不要关闭向导。 安装完成后，单击**配置 Active Directory 证书服务目标服务器上**。 打开广告客户服务配置向导。 阅读凭据信息，并根据需要提供的凭据组成员的企业管理员帐户。 单击**下一步**。  
  
10. 在**角色服务**，单击**证书颁发机构**，然后单击**下一步**。  
  
11. 在**安装类型**页上，检查**企业 CA**选中，则，然后单击**下一步**。  
  
12. 在**指定的一种 CA**页上，检查**根 CA**选中，则，然后单击**下一步**。  
  
13. 在**指定专用密钥种**页上，检查**创建新的专用密钥**选中，则，然后单击**下一步**。  
  
14. 在**CA 的加密**页上，继续为 CSP 默认设置 (**RSA # Microsoft 软件键存储提供商**) 和哈希算法 (**SHA1**)，并确定你部署的最佳键字符长度。 较大的关键字符长度提供最佳的安全;但是，他们可能会影响性能服务器，并且可能无法与传统应用程序兼容。 建议你保留 2048 年的默认设置。 单击**下一步**。  
  
15. 在**CA 名称**页面上，继续 CA 建议常见名称或更改你的要求根据名称。 确保您已某些 CA 名称是兼容命名惯例和目的，因为你无法安装广告客户服务后更改 CA 名称。 单击**下一步**。  
  
16. 在**有效期内**页上，在**指定有效期内**键入数字，选择一个时间值（年、月、周或天）。 建议五年的默认设置。 单击**下一步**。  
  
17. 在**CA 数据库**页上，在**指定数据库位置**，指定证书数据库和证书数据库日志的文件夹位置。 如果指定位置以外的默认位置，请确保与阻止访问 CA 数据库和日志文件未经授权的用户或计算机的访问控制列表 (Acl) 来保护文件夹。 单击**下一步**。  
  
18. 在**确认**，单击**配置**应用您的选择，然后单击**关闭**。  
  



---
title: 安装证书颁发机构
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 024fc73c4ed089d81808cf44d7cfe8b01bfffaa0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406324"
---
# <a name="install-the-certification-authority"></a>安装证书颁发机构

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程安装 Active Directory 证书服务（AD CS），以便您可以向运行网络策略服务器（NPS）、路由和远程访问服务（RRAS）的服务器注册服务器证书。  
  
> [!IMPORTANT]  
> -   安装 Active Directory 证书服务之前，必须为计算机命名，为计算机配置静态 IP 地址，并将计算机加入域。 有关如何完成这些任务的详细信息，请参阅 Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要执行此过程，你在其上安装 AD CS 的计算机必须加入到安装 Active Directory 域服务（AD DS）的域中。  
  
**企业管理员**和根域的**domain admins**组中的成员身份是完成此过程的最低要求。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 执行此过程，请打开 Windows PowerShell 并键入以下命令，然后按 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> 安装 AD CS 后，键入以下命令，然后按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>安装 Active Directory 证书服务  

> [!TIP]
> 如果要使用 Windows PowerShell 安装 Active Directory 的证书服务，请参阅[AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) for cmdlet 和可选参数。
  
1.  以 Enterprise Admins 组和根域的 Domain Admins 组的成员身份登录。  
  
2.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 将打开“添加角色和功能向导”。  
  
3.  在“开始之前”中单击“下一步”。  
  
    > [!NOTE]  
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。  
  
4.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。  
  
5.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。  
  
6.  在 "**选择服务器角色**" 的 "**角色**" 中，选择**Active Directory 证书服务**"。 当系统提示你添加所需功能时，单击 "**添加功能**"，然后单击 "**下一步**"。  
  
7.  在 "**选择功能**" 中，单击 "**下一步**"。  
  
8.  在**Active Directory 证书服务**"中，阅读提供的信息，然后单击"**下一步**"。  
  
9. 在 **“确认安装选择”** 中，单击 **“安装”** 。 请勿在安装过程中关闭向导。 安装完成后，请单击 **"配置" Active Directory 目标服务器上的证书服务**。 此时将打开 "AD CS 配置" 向导。 阅读凭据信息，如果需要，请提供作为 Enterprise Admins 组成员的帐户的凭据。 单击“下一步”。  
  
10. 在 "**角色服务**" 中单击 "**证书颁发机构**"，然后单击 "**下一步**"。  
  
11. 在 "**安装类型**" 页上，验证是否选择了 "**企业 CA** "，然后单击 "**下一步**"。  
  
12. 在 "**指定 CA 类型**" 页上，确认已选中 "**根 ca** "，然后单击 "**下一步**"。  
  
13. 在 "**指定私钥类型**" 页上，验证是否选择了 "**创建新的私钥**"，然后单击 "**下一步**"。  
  
14. 在 "**为 CA 加密**" 页上，保留 CSP 的默认设置（**RSA # Microsoft 软件密钥存储提供程序**）和哈希算法（**SHA2**），并确定部署的最佳密钥字符长度。 大型关键字符长度提供最佳安全性;但是，它们可能会影响服务器性能，并可能不与旧应用程序兼容。 建议保留默认设置2048。 单击“下一步”。  
  
15. 在 " **Ca 名称**" 页上，保留 ca 的建议公用名，或根据要求更改名称。 确保你确定 CA 名称与命名约定和用途兼容，因为你无法在安装 AD CS 后更改 CA 名称。 单击“下一步”。  
  
16. 在 "**有效期**" 页上，在 "**指定有效期**" 中键入数字，然后选择时间值（"年"、"月"、"周" 或 "天"）。 建议使用5年的默认设置。 单击“下一步”。  
  
17. 在 " **CA 数据库**" 页上的 "**指定数据库位置**" 中，指定证书数据库和证书数据库日志的文件夹位置。 如果指定的位置不是默认位置，请确保使用阻止未授权的用户或计算机访问 CA 数据库和日志文件的访问控制列表（Acl）来保护文件夹。 单击“下一步”。  
  
18. 在 "**确认**" 中，单击 "**配置**" 以应用你的选择，然后单击 "**关闭**"。  
  



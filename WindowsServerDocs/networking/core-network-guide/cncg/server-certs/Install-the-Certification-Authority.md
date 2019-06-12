---
title: 安装证书颁发机构
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1774d235703bd75d810f2649cb8ed3f2f92622d5
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811595"
---
# <a name="install-the-certification-authority"></a>安装证书颁发机构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于安装 Active Directory 证书服务 (AD CS)，以便你可以注册到运行网络策略服务器 (NPS)、 路由和远程访问服务 (RRAS) 或两者的服务器的服务器证书。  
  
> [!IMPORTANT]  
> -   在安装 Active Directory 证书服务之前，必须命名计算机，将计算机配置具有静态 IP 地址，并将计算机加入到域。 有关如何完成这些任务的详细信息，请参阅 Windows Server 2016[核心网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要执行此过程，在其安装 AD CS 的计算机必须加入到 Active Directory 域服务 (AD DS) 安装的域。  
  
在这种成员资格**Enterprise Admins**和根域**Domain Admins**组是完成此过程所需的最低。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 执行此过程，打开 Windows PowerShell 并键入以下命令，然后按 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> 安装 AD CS 后，键入以下命令，然后按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>若要安装 Active Directory 证书服务  

> [!TIP]
> 如果你想要使用 Windows PowerShell 来安装 Active Directory 证书服务，请参阅[Install-adcscertificationauthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) cmdlet 和可选参数。
  
1.  以 Enterprise Admins 组和根域的 Domain Admins 组的成员身份登录。  
  
2.  在“服务器管理器”中，单击“管理”  ，然后单击“添加角色和功能”  。 将打开“添加角色和功能向导”。  
  
3.  在“开始之前”  中单击“下一步”  。  
  
    > [!NOTE]  
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。  
  
4.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。  
  
5.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”  。  
  
6.  在中**选择服务器角色**，在**角色**，选择**Active Directory 证书服务**。 当系统提示添加必需的功能时，单击**添加功能**，然后单击**下一步**。  
  
7.  在中**选择的功能**，单击**下一步**。  
  
8.  在中**Active Directory 证书服务**，阅读提供的信息，然后单击**下一步**。  
  
9. 在 **“确认安装选择”** 中，单击 **“安装”** 。 不要在安装过程中关闭该向导。 安装完成后，单击**配置 Active Directory 证书服务在目标服务器上**。 AD CS 配置向导将打开。 读取凭据信息和必要时，该帐户是 Enterprise Admins 组的成员提供的凭据。 单击“下一步”  。  
  
10. 在中**角色服务**，单击**证书颁发机构**，然后单击**下一步**。  
  
11. 上**安装类型**页上，确认**企业 CA**已选择，然后单击**下一步**。  
  
12. 上**指定的 CA 的类型**页上，确认**根 CA**已选择，然后单击**下一步**。  
  
13. 上**指定的类型的私匙**页上，确认**创建新的私钥**已选择，然后单击**下一步**。  
  
14. 上**为 CA 加密**页上，保留默认设置用于 CSP (**RSA #Microsoft Software Key Storage Provider**) 和哈希算法 (**SHA2**)，并确定最佳你的部署的的密钥字符长度。 大型密钥字符长度提供最佳安全性;但是，它们可能会影响服务器性能并且可能不是与旧版应用程序兼容。 建议您保留默认设置为 2048年。 单击“下一步”  。  
  
15. 上**CA 名称**页上，保留 CA 的建议公用名或根据您的需求将名称更改。 请确保您已确定 CA 名称是符合命名约定和目的，因为不能在安装了 AD CS 之后更改 CA 名称。 单击“下一步”  。  
  
16. 上**有效期**页上，在**指定的有效期**，键入数并选择时间值 （年、 月、 周或天）。 建议的五年的默认设置。 单击“下一步”  。  
  
17. 上**CA 数据库**页上，在**指定的数据库位置**，指定证书数据库和证书数据库日志的文件夹位置。 如果指定的默认位置以外的位置，请确保使用阻止未经授权的用户或计算机访问 CA 数据库和日志文件的访问控制列表 (Acl) 进行保护的文件夹。 单击“下一步”  。  
  
18. 在中**确认**，单击**配置**以应用你的选择，然后单击**关闭**。  
  



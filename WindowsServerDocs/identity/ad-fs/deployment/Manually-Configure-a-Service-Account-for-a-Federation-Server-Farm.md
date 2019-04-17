---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: "手动联合身份验证的服务器场配置服务帐户"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>手动联合身份验证的服务器场配置服务帐户

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

如果你想要在 Active Directory 联合身份验证服务 \(AD FS\) 配置联盟服务器电场的日落环境，你必须在创建和配置专用的服务帐户 Active Directory 域服务 \(AD DS\) 场所在的位置。 然后你在使用此帐户电场的日落配置每个联合身份验证的服务器。 当你想要允许任何中使用 Windows 的集成身份验证广告 FS 场联盟服务器的身份验证的企业网络上的客户端计算机时，必须在你的组织完成以下任务。  
  
> [!NOTE]  
> 你需要在此过程中只有一个时间整个联合身份验证的服务器场执行任务。 以后，当您使用广告 FS 联盟服务器配置向导创建联盟服务器，你必须指定该相同帐户上**服务帐户**向导场中每个联合身份验证的服务器上的页面。  
  
#### <a name="create-a-dedicated-service-account"></a>创建专用的 service 帐户  
  
1.  创建专用的 user\/服务帐户位于身份提供商组织 Active Directory 森林。 此帐户时需要 Kerberos 身份验证的协议电场的日落方案中起效，并允许 pass\ 通过身份验证在每个联合身份验证的服务器上。 仅联合身份验证的服务器场供使用该帐户。  
  
2.  编辑用户帐户的属性，然后选择**密码永远不会到期**复选框。 此操作可确保，此服务帐户功能不会中断域密码更改要求的结果。  
  
    > [!NOTE]  
    > 使用此台专用的帐户的网络服务帐户将随机故障时导致 Windows 身份验证集成，通过定不到另一个上验证从一台服务器 Kerberos 门票尝试访问。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>若要设置的服务帐户 SPN  
  
1.  由于广告 FS 应用程序池应用程序池身份运行作为 user\/服务域帐户，你必须具有 Setspn.exe command\ 行工具域中配置服务主要名称 \(SPN\) 该帐户。 默认情况下，在运行 Windows Server 2008 计算机上安装 Setspn.exe。 已加入 user\/服务帐户所在的相同域的计算机上运行以下命令：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    例如，在聚集在域名系统 \(DNS\) 主机名称 fs.fabrikam.com 所有联合身份验证的服务器和 adfs2farm 名为到广告 FS 应用程序池分配的服务帐户名的情况下，键入命令，如下所示，然后按 ENTER:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    有必要才能完成此任务为此帐户一次。  
  
2.  广告 FS 应用程序池身份更改为服务帐户后，设置访问控制列表 \(ACLs\) SQL Server 数据库允许阅读这个新帐户的访问，以便广告 FS 应用程序池可以阅读策略数据。  
  


---
title: 在 CA1 上配置 CDP 和 AIA 扩展
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 2bdd03636d1cbbcf5b0355358cbedeb63f97af1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834218"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上配置 CDP 和 AIA 扩展

>适用于：Windows 服务器 （半年频道），Windows Server 2016

此过程可用于在 CA1 配置证书吊销列表 (CRL) 分发点 (CDP) 和颁发机构信息访问 (AIA) 设置。  
  
若要执行此过程，必须是域管理员组的成员。  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>若要在 CA1 配置 CDP 和 AIA 扩展  
  
1.  在服务器管理器中，单击 **“工具”** ，然后单击 **“证书颁发机构”**。  
  
2.  在证书颁发机构控制台树中，右键单击**corp CA1 CA**，然后单击**属性**。  
  
    > [!NOTE]  
    > 如果您未命名为 CA1 的计算机，并且你的域名不同于在此示例中，您的 ca 名称不相同。 CA 名称的格式*域*-*CAComputerName*-CA。  
  
3.  单击 **“扩展”** 选项卡。絋粄**选择扩展**设置为**CRL 分发点 (CDP)**，然后在**指定用户可以从中获取证书吊销列表 (CRL)**，执行以下操作：  
  
    1.  选择条目`file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
    2.  选择条目`https://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
    3.  选择路径开头的条目`ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
4.  在中**指定用户可以从中获取证书吊销列表 (CRL) 的位置**，单击**添加**。 **添加位置**对话框随即打开。  
  
5.  在中**添加位置**，在**位置**，类型`https://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**确定**。 将返回到 CA 属性对话框。  
  
6.  上**扩展**选项卡上，选中以下复选框：  
  
    -   **包含在 Crl 中。客户端用它来寻找增量 CRL 的位置**  
  
    -   **包括在颁发的证书的 CDP 扩展中**  
  
7.  在中**指定用户可以从中获取证书吊销列表 (CRL) 的位置**，单击**添加**。 **添加位置**对话框随即打开。  
  
8.  在中**添加位置**，在**位置**，类型`file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**确定**。 将返回到 CA 属性对话框。  
  
9. 上**扩展**选项卡上，选中以下复选框：  
  
    -   **将 Crl 发布到此位置**  
  
    -   **将增量 Crl 发布到此位置**  
  
10. 更改**选择扩展**到**颁发机构信息访问 (AIA)**，然后在**指定用户可以从中获取证书吊销列表 (CRL)**，执行操作以下内容：  
  
    1.  选择路径开头的条目`ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
    2.  选择条目`https://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
    3.  选择条目`file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`，然后单击**删除**。 在中**确认删除**，单击**是**。  
  
11. 在中**指定用户可以获取该证书为此 CA 的位置**，单击**添加**。 **添加位置**对话框随即打开。  
  
12. 在中**添加位置**，在**位置**，类型`https://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`，然后单击**确定**。 将返回到 CA 属性对话框。  
  
13. 上**扩展**选项卡上，选择**包括在已颁发证书的 AIA**。  
  
14. 当系统提示重新启动 Active Directory 证书服务，单击**否**。 稍后将重新启动该服务。  
  


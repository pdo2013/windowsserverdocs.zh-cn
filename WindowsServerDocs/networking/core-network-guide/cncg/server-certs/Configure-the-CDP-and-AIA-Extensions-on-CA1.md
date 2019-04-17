---
title: 上 CA1 配置 CDP 和 AIA 扩展
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80f6d68816a58b2ac30010e0917ce0f816a30234
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>上 CA1 配置 CDP 和 AIA 扩展

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程配置 CA1 上证书吊销列表 (CRL) Distribution 点 (CDP) 和颁发机构信息的访问权限 (AIA) 的设置。  
  
若要执行此过程，你必须域管理员的成员。  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 配置 CDP 和 AIA 的扩展  
  
1.  在服务器管理器中，单击**工具**，然后单击**证书颁发机构**。  
  
2.  在未经认证颁发控制台树中，右键单击**corp CA1 CA**，然后单击**属性**。  
  
    > [!NOTE]  
    > 如果你未命名计算机 CA1，并且你的域名不同在此示例中，您 CA 名称是不同。 加拿大名称是格式*域*-*CAComputerName*-CA。  
  
3.  单击**扩展**选项，确保**选择扩展**设置为**CRL Distribution 点 (CDP)**，并在指定用户可以获取证书吊销齐名 (CRL)，请执行以下操作：  
  
    1.  选择条目`file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
    2.  选择条目`http:\/\/<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
    3.  选择启动的路径条目`ldap:\/\/CN\=<CATruncatedName><CRLNameSuffix>,CN\=<ServerShortName>`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
4.  在**指定用户可以获取证书吊销列表 (CRL) 位置**，单击**添加**。 **添加位置**对话框中打开。  
  
5.  在**添加位置**，请在**位置**，类型`http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**确定**。 此操作将返回到 CA 属性对话框。  
  
6.  在**扩展**选项卡上，选中以下复选框：  
  
    -   **包含在 Crl。 客户使用此查找增量 CRL 位置**  
  
    -   **包含的证书颁发 CDP 扩展**  
  
7.  在**指定用户可以获取证书吊销列表 (CRL) 位置**，单击**添加**。 **添加位置**对话框中打开。  
  
8.  在**添加位置**，请在**位置**，类型`file:\/\/\\\\pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`，然后单击**确定**。 此操作将返回到 CA 属性对话框。  
  
9. 在**扩展**选项卡上，选中以下复选框：  
  
    -   **发布至该位置 Crl**  
  
    -   **发布至该位置增量 Crl**  
  
10. 更改**选择扩展**到**颁发机构信息的访问权限 (AIA)**，在**指定用户可以获取证书吊销列表 (CRL) 位置**，请执行以下操作：  
  
    1.  选择启动的路径条目`ldap:\/\/CN\=<CATruncatedName>,CN\=AIA,CN\=Public Key Services`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
    2.  选择条目`http:\/\/<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
    3.  选择条目`file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然后单击**删除**。 在**确认删除**，单击**是**。  
  
11. 在**指定用户可以获取证书吊销列表 (CRL) 位置**，单击**添加**。 **添加位置**对话框中打开。  
  
12. 在**添加位置**，请在**位置**，类型`http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然后单击**确定**。 此操作将返回到 CA 属性对话框。  
  
13. 在**扩展**选项卡上，选择**包含中的证书颁发 AIA**。  
  
14. 在**添加位置**，请在**位置**，类型`file:\/\/\\\\pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`，然后单击**确定**。 此操作将返回到 CA 属性对话框。  
  
    > [!IMPORTANT]  
    > 确保**包含的证书颁发 AIA 扩展**未选择。  
  
15. 出现提示时重新启动 Active Directory 证书服务，请单击**否**。 将稍后重启该服务。  
  



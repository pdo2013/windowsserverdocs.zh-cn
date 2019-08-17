---
title: 在 CA1 上配置 CDP 和 AIA 扩展
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 74d77347e0ca1dffbca4e2cf6f17b9faa25d55bf
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560494"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上配置 CDP 和 AIA 扩展

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用此过程在 CA1 上配置证书吊销列表 (CRL) 分发点 (CDP) 和颁发机构信息访问 (AIA) 设置。  
  
若要执行此过程, 您必须是 Domain Admins 的成员。  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>在 CA1 上配置 CDP 和 AIA 扩展  
  
1.  在服务器管理器中，单击 **“工具”** ，然后单击 **“证书颁发机构”** 。  
  
2.  在 "证书颁发机构" 控制台树中, 右键单击 " **CA1-CA**", 然后单击 "**属性**"。  
  
    > [!NOTE]  
    > 如果你未将计算机命名为 CA1, 并且你的域名与此示例中的名称不同, 则你的 CA 名称不同。 CA 名称的格式为*domain*-*CAComputerName*-ca。  
  
3.  单击 **“扩展”** 选项卡。确保将 "**选择扩展**" 设置为 " **CRL 分发点 (CDP)** ", 并在 "**指定用户可以从中获取证书吊销列表 (CRL) 的位置**" 中执行以下操作:  
  
    1.  选择该条目`file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
    2.  选择该条目`http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
    3.  选择以路径`ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`开头的条目, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
4.  在 "**指定用户可以从中获取证书吊销列表 (CRL) 的位置**" 中, 单击 "**添加**"。 "**添加位置**" 对话框将打开。  
  
5.  在 **"添加位置**"的 "位置`http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`" 中, 键入, 然后单击 **"确定"** 。 这会返回到 CA 属性对话框。  
  
6.  在 "**扩展**" 选项卡上, 选中以下复选框:  
  
    -   **包括在 Crl 中。客户端使用此来查找增量 CRL 位置**  
  
    -   **包含在已颁发证书的 CDP 扩展中**  
  
7.  在 "**指定用户可以从中获取证书吊销列表 (CRL) 的位置**" 中, 单击 "**添加**"。 "**添加位置**" 对话框将打开。  
  
8.  在 **"添加位置**"的 "位置`file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`" 中, 键入, 然后单击 **"确定"** 。 这会返回到 CA 属性对话框。  
  
9. 在 "**扩展**" 选项卡上, 选中以下复选框:  
  
    -   **将 Crl 发布到此位置**  
  
    -   **将增量 Crl 发布到此位置**  
  
10. 将**选择扩展**更改为**颁发机构信息访问 (AIA)** , 并在 "**指定用户可以从中获取证书吊销列表 (CRL) 的位置**" 中, 执行以下操作:  
  
    1.  选择以路径`ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`开头的条目, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
    2.  选择该条目`http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
    3.  选择该条目`file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`, 然后单击 "**删除**"。 在 "**确认删除**" 中单击 **"是"** 。  
  
11. 在 "**指定用户可以从中获取此 CA 的证书的位置**" 中, 单击 "**添加**"。 "**添加位置**" 对话框将打开。  
  
12. 在 **"添加位置**"的 "位置`http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`" 中, 键入, 然后单击 **"确定"** 。 这会返回到 CA 属性对话框。  
  
13. 在 "**扩展**" 选项卡上, 选择 **"在已颁发证书的 AIA 中包含"** 。  
  
14. 当系统提示重新启动 Active Directory 证书服务时, 单击 "**否**"。 稍后将重新启动该服务。  
  


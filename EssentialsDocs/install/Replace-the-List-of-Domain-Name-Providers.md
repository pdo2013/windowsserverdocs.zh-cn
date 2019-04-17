---
title: "替换域名称提供商列表"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>替换域名称提供商列表

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

可替换域名称提供商列表，所显示设置域名称向导中的完成以下任务：  
  

-   [创建推荐服务文件](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [添加到参考计算机上的注册表项](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [创建推荐服务文件](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [添加到参考计算机上的注册表项](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>创建推荐服务文件  
 下线服务管理工具创建一组用于定义的域名称提供设置域名称向导中的显示列表的文件。 XML 格式的文件全球范围内的每个区域创建并包含的工具中指定的域名称提供的信息。 必须的文件夹中，可以通过管理 Internet 的你的安全链接 (HTTPS) 来访问位于由工具创建的文件。  
  
##### <a name="to-create-the-referral-files"></a>若要创建检索文件  
  
1.  打开推荐服务管理工具。  
  
2.  单击**添加**。  
  
3.  在添加域名称提供商对话框中，输入域名称提供程序的名称。  
  
4.  添加顶级支持的域名称提供商的域。 执行此操作方法是依次单击**添加**，输入的顶级域标识符，并选择受支持的区域。 你可以选择**所有地区**。  
  
5.  输入域名称提供商的说明。  
  
6.  添加 Url 的所有相关联的域名称提供商的网站。  
  
7.  如果徽标域名称服务提供商的可用，方法是依次单击添加徽标**更改徽标**。  
  
8.  单击**保存**。  
  
9. 每个域名称提供程序，你想要列表向导中的重复步骤 8 通过 2。  
  
10. 添加的所有域名称提供程序后，选择引用文件将位于文件夹。 选择文件夹时，请记住，必须通过 HTTPS 链接访问推荐文件。  
  
11. 单击**生成文件系统文件**。  
  
###  <a name="BKMK_AddRegistry"></a>添加到参考计算机上的注册表项  
 若要指定在哪里操作系统可以找到推荐服务文件，必须添加注册表项。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要添加对注册表项  
  
1.  在参考计算机上，单击**开始**，输入**regedit**，然后按**Enter**。  
  
2.  在左侧窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**，展开**域经理**，然后展开**提供商**。  
  
3.  右键单击**E423C85D-6B1F-4583-95E0-449D8263BAC4**键，，然后单击**字符串值**。  
  
4.  键入**ReferralServerHttpsUri**字符串，并按的名称**Enter**。  
  
5.  右键单击新**ReferralServerHttpsUri**字符串的右窗格中，然后单击**修改**。  
  

6.  键入用于访问中创建的检索文件的 HTTPS URL[创建推荐服务文件](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)，然后单击**确定**。  

6.  键入用于访问中创建的检索文件的 HTTPS URL[创建推荐服务文件](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)，然后单击**确定**。  

  
    > [!IMPORTANT]
    >  斜杠（/），则需要 URL 的结尾。  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>域名状态问题  
 如果合作伙伴添加了域名称提供商，并使用 Windows Server Essentials SDK 中的应用程序编程接口 (API) 设置的证书未知、失败和 CertificateRequestNotSubmitted 状态，客户将收到错误消息和配置结果。 这是因为由异常情况下而不是退回状态。  
  
 以下域状态被失败，并且应为错误报告：  
  
-   失败  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 以下域状态后，应为成功报告：  
  
-   准备  
  
-   待付款  
  
-   InRenewal  
  
## <a name="see-also"></a>请参阅  

 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)

 [创建和自定义映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](../install/Additional-Customizations.md)   
 [准备部署该映像](../install/Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](../install/Testing-the-Customer-Experience.md)


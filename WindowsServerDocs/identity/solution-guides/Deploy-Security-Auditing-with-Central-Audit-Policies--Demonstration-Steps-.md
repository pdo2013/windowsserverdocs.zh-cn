---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: "部署安全审核中央审核策略（演示步骤）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>部署安全审核中央审核策略（演示步骤）

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在此情况下，将使用您在财经策略审核财经文档文件夹中文件的访问权限[部署中央访问策略 #40; 演示步骤 & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). 如果未授权访问该文件夹的用户尝试访问它，在事件查看器捕获活动。   
 测试此方案需要执行以下步骤。  
  
|任务|描述|  
|--------|---------------|  
|[配置全球对象的访问权限](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|在此步骤，域控制器上配置全球对象访问策略。|  
|[更新组策略设置](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|登录到文件服务器，并应用组策略更新。|  
|[验证已应用在全球对象访问策略](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|在事件查看器中查看相关的事件。 事件应包含的国家/地区和文档类型元数据。|  
  
## <a name="BKMK_1"></a>配置全球对象访问策略  
在此步骤中，您将配置域控制器策略全球对象访问。  
  
#### <a name="to-configure-a-global-object-access-policy"></a>若要配置访问策略全球对象  
  
1.  以 contoso\administrator 使用密码登录到域控制器 DC1 **pass@word1**。  
  
2.  在服务器管理器中，请指向**工具**，然后单击**组策略管理**。  
  
3.  控制台树中，双击**域**，双击**contoso.com**，单击**Contoso**，然后双击**文件服务器**。  
  
4.  右键单击**FlexibleAccessGPO**，然后单击**编辑**。  
  
5.  双击**计算机配置**，双击**策略**，然后双击**Windows 设置**。  
  
6.  双击**安全设置**，双击**高级审查策略配置**，然后双击**审核策略**。  
  
7.  双击**对象访问**，然后双击**审核文件系统**。  
  
8.  选择**配置以下事件**复选框、选择**成功**和**失败**复选框，然后单击**确定**。  
  
9. 在导航窗格中，双击**全球对象访问审核**，然后双击**文件系统**。  
  
10. 选择**定义该策略设置**复选框，然后单击**配置**。  
  
11. 在**的全球文件 SACL 高级安全设置**框中，单击**添加**，然后单击**选择主体**，类型**每个人都**，然后单击**确定**。  
  
12. 在**审核全球文件 SACL 条目**框中，选择**完全控制**中**权限**框。  
  
13. 在**添加条件：**部分中，单击**添加条件**和在下拉列表中选择   
    [**资源**][**部门**][**Any of**][**Value**][**财经**]。  
  
14. 单击**确定**三次完成全球对象访问配置审核策略设置。  
  
15. 在导航窗格中，单击**对象访问**，并在结果窗格中，双击**审核处理操作**。 单击**配置以下审核事件**，**成功**，并**失败**，单击**确定**，然后关闭灵活访问 GPO。  
  
## <a name="BKMK_2"></a>更新组策略设置  
在此步骤中之后你创建的审核策略, 更新组策略设置。  
  
#### <a name="to-update-group-policy-settings"></a>若要更新组策略设置  
  
1.  登录到文件服务器，文件作为 contoso\Administrator，使用密码 1 **pass@word1**。  
  
2.  按 Windows 键 + R，然后键入**cmd**打开 Command Prompt 窗口。  
  
    > [!NOTE]  
    > 如果**用户帐户控制**出现的对话框中，它将显示操作是您希望，并单击确认**是**。  
  
3.  键入**gpupdate /force**，然后按 ENTER。  
  
## <a name="BKMK_3"></a>验证已应用在全球对象访问策略  
在应用组策略设置后，你可以确定审核策略设置应用正确。  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>若要验证应用了全球对象访问策略  
  
1.  登录到客户端计算机，作为 Contoso\MReid 客户端的 1 中。 浏览到的文件夹超链接"file:///\\\ID_AD_FILE1\\\Finance"\\\ FILE1\Finance 文档和修改 Word 文档 2。  
  
2.  登录到文件服务器，文件 contoso\administrator 为 1。 打开事件查看器中，浏览到**Windows 日志**、选择**安全**，并确认你的活动导致审核事件**4656**和**4663**（即使你未对文件或文件夹您创建设置明确审核 Sacl，修改，或删除）。  
  
> [!IMPORTANT]  
> 在资源所在的位置，代表正在其检查有效访问权限的用户的计算机上生成新的登录活动。 当分析用户登录活动的以便区分由于有效访问生成的登录活动以及由于生成安全审核日志交互式网络用户在登录时，不包括模拟级别信息。 由于有效访问生成登录事件时, 模拟级别将身份。 网络交互式用户登录通常生成模拟级别也相同登录事件 = 模拟或委派。  
  
## <a name="BKMK_Links"></a>请参阅  
  
-   [方案：访问审核文件](Scenario--File-Access-Auditing.md)  
  
-   [文件计划访问审核](Plan-for-File-Access-Auditing.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  


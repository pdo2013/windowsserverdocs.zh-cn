---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: 使用中心审核策略部署安全审核（示范步骤）
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b14ded98c4f1a340349119bd9f5f42e3a1bf9434
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445742"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>使用中心审核策略部署安全审核（示范步骤）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在此方案中，你将使用你在中创建 Finance 策略审核对财务文档文件夹中的文件的访问[部署中心访问策略&#40;演示步骤&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)。 如果未授权访问该文件夹的用户尝试访问它，那么将事件查看器中捕获这一活动。   
 下列步骤是测试本应用场景所必需的。  
  
|任务|描述|  
|--------|---------------|  
|[配置全局对象访问](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|在这一步中，你在域控制器上配置全局对象访问策略。|  
|[更新组策略设置](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|登录到文件服务器并应用“组策略”更新。|  
|[验证已应用全局对象访问策略](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|在事件查看器中查看相关事件。 这些事件应包括国家和文档类型的元数据。|  
  
## <a name="BKMK_1"></a>配置全局对象访问策略  
在这一步中，你在域控制器上配置全局对象访问策略。  
  
#### <a name="to-configure-a-global-object-access-policy"></a>配置全局对象访问策略的步骤  
  
1. 以 contoso\administrator 的身份使用密码登录到域控制器 DC1 <strong>pass@word1</strong>。  
  
2. 在“服务器管理器”中，指向 **“工具”** ，然后单击 **“组策略管理器”** 。  
  
3. 在控制台树中，依次双击 **“域”** 、 **“contoso.com”** ，单击 **“Contoso”** ，然后双击 **“文件服务器”** 。  
  
4. 右键单击 **“FlexibleAccessGPO”** ，然后单击 **“编辑”** 。  
  
5. 依次双击 **“计算机配置”** 、 **“策略”** 和 **“Windows 设置”** 。  
  
6. 依次双击 **“安全设置”** 、 **“高级审核策略配置”** 和 **“审核策略”** 。  
  
7. 双击 **“对象访问”** ，然后双击 **“审核文件系统”** 。  
  
8. 依次选中 **“配置以下事件”** 复选框、 **“成功”** 和 **“失败”** 复选框，然后单击 **“确定”** 。  
  
9. 在导航窗格中，依次双击 **“全局对象访问审核”** 、**文件系统**。  
  
10. 选中 **“定义此策略设置”** 复选框，然后单击 **“配置”** 。  
  
11. 在 **“全局文件 SACL 的高级安全设置”** 框中，单击 **“添加”** ，然后单击 **选择主体**，键入 **Everyone**，然后单击 **确定**。  
  
12. 在 **“全局文件 SACL 的审核项”** 框中，选择 **“权限”** 框中的**完全控制**。  
  
13. 在“添加条件：”  部分，单击“添加条件”  并在下拉列表中选择   
    [**资源**] [**部门**] [**任何**] [**值**] [**财务**]。  
  
14. 单击 **“确定”** 三次，完成全局对象访问审核策略设置的配置。  
  
15. 在导航窗格中，单击 **“对象访问”** ，并在结果窗格中，双击 **“审核句柄操作”** 。 单击 **配置下列审核事件**、 **成功**、和 **失败**，单击 **确定**，然后关闭可变访问 GPO。  
  
## <a name="BKMK_2"></a>更新组策略设置  
在这一步中，你在创建审核策略之后更新“组策略”设置。  
  
#### <a name="to-update-group-policy-settings"></a>更新组策略设置的步骤  
  
1. 登录到文件服务器，以 contoso\administrator 的身份使用密码 FILE1 <strong>pass@word1</strong>。  
  
2. 按下 Windows 键+R，然后键入 **cmd** 打开“命令提示符”窗口。  
  
   > [!NOTE]  
   > 如果出现了“用户帐户控制”  对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”  。  
  
3. 键入 **gpupdate /force**，然后按回车。  
  
## <a name="BKMK_3"></a>验证已应用全局对象访问策略  
应用组策略设置之后，你可以验证是否正确应用审核策略设置。  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>验证是否应用全局对象访问策略的步骤  
  
1.  以 Contoso\MReid 身份登录到客户端计算机 CLIENT1。 浏览到文件夹超链接"file:///\\\\\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance Documents，并修改 Word Document 2。  
  
2.  以 contoso\administrator 的身份，登录到文件服务器 FILE1。 打开“事件查看器”，浏览到 **“Windows 日志”** ，选择 **“安全性”** ，并确认你的活动导致审核事件 **4656** 和 **4663** （虽然你没有在创建、修改和删除的文件或文件夹上设置显示审核 SACL）。  
  
> [!IMPORTANT]  
> 资源所在的计算机上会生成一个新的登录事件，以接受了有效访问检查的用户的名义。 为用户登录活动分析安全审核日志的时候，为了区分因有效访问生成的登录事件与因交互网络用户登录生成的登录事件，必须包含“模拟级别”的信息。 当登录事件是因有效访问生成的，“模拟级别”将会是“标识”。 网络交互用户的登录往往生成一个“模拟级别”=“模拟”或“委派”的登录事件。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [方案：文件访问审核](Scenario--File-Access-Auditing.md)  
  
-   [文件访问审核计划](Plan-for-File-Access-Auditing.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  


---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: "更新密码自定义项"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>更新密码自定义项 

>适用于：Windows Server 2016，Windows Server 2012 R2

在某些情况下，用户可能无法连接到公司的网络，若要更改其帐户密码。 此因素可能会出现问题，特别是对于远从最近可能 live 远程员工公司 office。 为这些特定的情况下，更新密码页面可用通过仅连接到 Internet。  
  
通过提供自己的页面的说明，你可以自定义的更新密码的页面。  
  
> 若要启用密码更新页面，请转到下端点广告 FS 管理。 更新密码的端点位于其他-/ adfs/门户/updatepassword/下底部。 一旦你已启用端点，你必须重新启动广告 FS 服务。 必须手动完成此操作。 你可以然后导航到 https://<fqdn>/adfs/门户/updatepassword / 工作区上已加入设备，你应该可以看到更新密码页面。  
  
![更新](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>自定义更新密码页面说明  
若要自定义更新密码页面说明，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>其他参考 
[广告 FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  

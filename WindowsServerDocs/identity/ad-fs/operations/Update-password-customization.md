---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: 更新密码自定义项
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876568"
---
# <a name="update-password-customization"></a>更新密码自定义项 

>适用于：Windows Server 2016, Windows Server 2012 R2

在某些情况下，用户可能无法连接到企业网络来更改其帐户密码。 此因素可能会产生问题，尤其对于那些可能离最近的企业办公室都较远的远程员工。 对于这些特定情况，只有通过连接到 Internet 才能使用更新密码页面。  
  
通过提供自己的页面描述，你可以自定义更新密码页。  
  
> 若要启用密码更新页面，请转到“终结点”下的“AD FS 管理”。 更新密码的终结点位于底部，在“其他 - /adfs/portal/updatepassword/”下。 一旦启用了该终结点，则必须重新启动 AD FS 服务。 必须手动完成此操作。 然后，你可以在加入工作区的设备上导航到 https://<fqdn>/adfs/portal/updatepassword/，并且你应看到更新密码页面。  
  
![更新](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>自定义更新密码页面描述  
若要自定义更新密码页面描述，请使用以下 Windows PowerShell cmdlet 和语法。  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>其他参考 
[AD FS 用户登录自定义](AD-FS-user-sign-in-customization.md)  

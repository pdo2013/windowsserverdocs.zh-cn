---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: 将客户端计算机配置为信任帐户联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8c047f69cb0cb2db57ea33697c49e53a212fe823
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359858"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>将客户端计算机配置为信任帐户联合服务器

为了使客户端计算机可以使用 Active Directory 联合身份验证服务 \(AD FS @ no__t 成功访问联合应用程序，你必须先在每台客户端计算机上配置 Internet Explorer 设置，以便浏览器信任帐户联合服务器。 你可以手动执行此操作，也可以通过完成以下过程之一，根据你的管理首选项组策略执行此操作。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>手动配置 Internet Explorer 设置  
你可以使用以下过程手动配置每个用户的 Internet Explorer 设置，以支持通过 AD FS 进行联合身份验证。 如果多个用户使用一台计算机，则对每个用户配置文件多次完成此过程。  
  
若要执行此过程，请以将要访问联合应用程序的用户身份登录。 这是配置文件 @ no__t-0specific 设置。 因此，它要求你为特定计算机上存在的每个配置文件手动添加此设置。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>手动将客户端计算机配置为信任帐户联合服务器  
  
1.  在客户端计算机上，启动 Internet Explorer。  
  
2.  在 "**工具**" 菜单上，单击 " **Internet 选项**"。  
  
3.  在 "**安全**" 选项卡上，单击 "**本地 intranet** " 图标，然后单击 "**站点**"。  
  
4.  单击 "**高级**"，然后在 **"将此网站添加到区域"** 中，键入帐户联合服务器的完整域名系统 \(DNS @ no__t \(for 示例，https： \/\/fs1.fabrikam.com @ no__t-7，然后单击 "添加".  
  
5.  单击“确定”三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>使用组策略配置 Internet Explorer 设置  
对于大多数部署，建议使用组策略将相应的 Internet Explorer 设置推送到每台客户端计算机。  
  
**Domain admins**或**Enterprise Admins**中的成员身份或同等身份 ACTIVE DIRECTORY 域服务 \(AD DS @ no__t 是完成此过程的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http\/：\/\/go.microsoft.com\/fwlink？LinkId\=83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>将客户端计算机配置为使用组策略信任帐户联合服务器  
  
1.  在帐户伙伴组织的林中的域控制器上，启动**组策略管理**"" 管理 no__t-1in "。  
  
2.  找到相应的组策略对象 \(GPO @ no__t; 右 @ no__t-2click，然后单击 "**编辑**"。  
  
3.  在控制台树中，打开 "**用户配置 @ no__t-1Preferences @ no__t-2Windows Settings @ no__t-3Internet Explorer 维护**"，然后单击 "**安全**"。  
  
4.  在详细信息窗格中，双击 "no__t-0click**安全区域和内容分级**"。  
  
5.  在 "**安全区域和隐私**" 下，单击 **"导入当前安全区域和隐私设置**"，然后单击 "**修改设置**"。  
  
6.  单击 "**本地 intranet**"，然后单击 "**站点**"。  
  
7.  在 **"将此网站添加到区域"** 中，键入帐户联合服务器 @no__t 的完整 DNS 名称-1for 示例，https： \/\/fs1.fabrikam.com @ no__t，单击 "**添加**"，然后单击 "**关闭**"。  
  
8.  单击 **"确定"** 两次以将这些更改应用到组策略。  
  

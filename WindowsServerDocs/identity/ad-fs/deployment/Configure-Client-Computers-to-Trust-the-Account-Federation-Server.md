---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: "将配置客户端计算机信任帐户联盟服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>将配置客户端计算机信任帐户联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

以便客户端计算机成功可以访问在使用 Active Directory 联合身份验证服务 \(AD FS\) 联合的应用，你必须先配置 Internet Explorer 设置每个客户端计算机上以便浏览器信任帐户联合身份验证的服务器。 你可以执行此操作手动或通过组策略，具体取决于您管理的首选项，通过完成以下过程之一。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>配置 Internet Explorer 设置手动  
你可以使用下面的过程中手动配置每个用户 Internet Explorer 设置，以支持通过 FS 广告联盟。 如果多个用户使用一台计算机，完成此过程多次 — 一次，每个用户配置文件。  
  
若要执行此过程，以将访问联合的应用的用户登录。 这是 profile\ 特定设置。 因此，它需要手动在特定计算机添加此设置针对每个存在的个人资料。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>若要手动配置客户端计算机信任帐户联盟服务器  
  
1.  在客户端计算机上，启动 Internet Explorer。  
  
2.  在**工具**菜单上，单击**Internet 选项**。  
  
3.  在**安全**选项卡上，单击**本地 intranet**图标，然后依次单击**站点**。  
  
4.  单击**高级**，并在**将该网站添加到区域**，键入的帐户联盟服务器域名系统 \(DNS\) 全名 \ (例如，https:\/\/fs1.fabrikam.com\)，然后单击**添加**。  
  
5.  单击**确定**三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>配置使用组策略 Internet Explorer 设置  
对于大多数部署，我们建议你使用组策略为每台计算机推送合适的 Internet Explorer 设置。  
  
在会员**域管理员**或**企业管理员**，或、 Active Directory 域服务中的等效 \(AD DS\) 的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)\ (http:///\/ go.microsoft.com\/fwlink\ /？LinkId\ = 83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>若要将配置客户端计算机使用组策略信任帐户联盟服务器  
  
1.  在帐户合作伙伴公司的森林中的域控制器，开始**组策略管理**snap\ 中。  
  
2.  查找相应的组策略对象 \(GPO\)，right\ 单击它，然后依次单击**编辑**。  
  
3.  在控制台树中，打开**用户 Configuration\\Preferences\\Windows Settings\\Internet Explorer 维护**，然后单击**安全**。  
  
4.  在详细信息窗格中，double\ 单击**安全区域和内容分级**。  
  
5.  下**区域安全和隐私**，单击**导入的当前安全区域和隐私设置**，然后单击**修改设置**。  
  
6.  单击**本地 intranet**，然后单击**站点**。  
  
7.  在**将该网站添加到区域**，键入的帐户联盟服务器 DNS 全名 \ (例如，https:\/\/fs1.fabrikam.com\) 单击**添加**，然后单击**关闭**。  
  
8.  单击**确定**将这些更改应用于组策略的两次。  
  

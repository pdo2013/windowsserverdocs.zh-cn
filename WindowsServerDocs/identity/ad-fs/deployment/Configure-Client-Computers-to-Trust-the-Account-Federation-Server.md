---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: 配置客户端计算机信任的帐户联合身份验证服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886748"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>配置客户端计算机信任的帐户联合身份验证服务器

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

以便客户端计算机可以成功访问联合应用程序使用 Active Directory 联合身份验证服务\(AD FS\)，您必须首先配置 Internet Explorer 设置每个客户端计算机上，以便在浏览器信任帐户联合身份验证服务器。 您可以执行此操作手动或通过组策略，具体取决于您管理的首选项，通过完成以下过程之一。  
  
## <a name="configuring-internet-explorer-settings-manually"></a>配置 Internet Explorer 设置手动  
可以使用以下过程来手动配置每个用户的 Internet Explorer 设置以支持通过 AD FS 联合身份验证。 如果多个用户使用一台计算机，完成此过程多次，一次针对每个用户配置文件。  
  
若要执行此过程，以将访问联合应用程序的用户登录。 这是一个配置文件\-特定设置。 因此，它需要的特定计算机上手动添加的每个配置文件存在于此设置。  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>若要手动配置客户端计算机信任的帐户联合身份验证服务器  
  
1.  在客户端计算机上，启动 Internet Explorer。  
  
2.  上**工具**菜单上，单击**Internet 选项**。  
  
3.  上**安全**选项卡上，单击**本地 intranet**图标，，然后单击**站点**。  
  
4.  单击**高级**，然后在**将该网站添加到区域**，键入在完整域名系统\(DNS\)帐户联合身份验证服务器的名称\(例如，https:\/\/fs1.fabrikam.com\)，然后单击**添加**。  
  
5.  单击“确定”三次。  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>通过使用组策略配置 Internet Explorer 设置  
对于大多数部署，我们建议使用组策略，将适当的 Internet Explorer 设置推送到每个客户端计算机。  
  
中的成员身份**Domain Admins**或**Enterprise Admins**，或，Active Directory 域服务中的等效\(AD DS\)身份是完成此过程所需的最低。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>若要配置客户端计算机通过使用组策略信任帐户联合身份验证服务器  
  
1.  在帐户伙伴组织的林中的域控制器上, 启动**组策略管理**对齐\-中。  
  
2.  找不到相应的组策略对象\(GPO\)，右键\-，单击，然后单击**编辑**。  
  
3.  在控制台树中，打开**用户配置\\首选项\\Windows 设置\\Internet Explorer 维护**，然后单击**安全**。  
  
4.  在详细信息窗格中，双击\-单击**安全区域和内容分级**。  
  
5.  下**安全区域和隐私**，单击**导入当前安全区域和隐私设置**，然后单击**修改设置**。  
  
6.  单击**本地 intranet**，然后单击**站点**。  
  
7.  在中**将该网站添加到区域**，键入帐户联合身份验证服务器的 DNS 全名\(例如，https:\/\/fs1.fabrikam.com\)，单击**添加**，然后单击**关闭**。  
  
8.  单击**确定**两次，以将这些更改应用于组策略。  
  

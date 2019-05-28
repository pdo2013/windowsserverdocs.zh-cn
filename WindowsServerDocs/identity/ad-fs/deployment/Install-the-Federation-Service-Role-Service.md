---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: 安装联合身份验证服务角色服务
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 80a6cb2bc8e6f0fdb1a777a42f5d245f98ac3dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192089"
---
# <a name="install-the-federation-service-role-service"></a>安装联合身份验证服务角色服务

现在，已正确配置一台计算机为必备的应用程序和证书，已准备好安装 Active Directory 联合身份验证服务的联合身份验证服务角色服务\(AD FS\)。 在计算机上安装联合身份验证服务时，该计算机将成为联合服务器。  
  
> [!NOTE]  
> 对于联合 Web 单一\-符号\-上\(SSO\)设计中，您必须在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器. 有关详细信息，请参阅 [Where to Place a Federation Server](https://technet.microsoft.com/library/dd807127.aspx)。  
  
可以使用以下过程将成为第一个联合身份验证服务器的计算机上或在将成为一个现有的联合服务器场的联合身份验证服务器的计算机上安装的 AD FS 的联合身份验证服务角色服务。  
  
## <a name="prerequisites"></a>系统必备  
验证带有私钥的 SSL 证书的已安装或导入到本地证书存储区\(个人存储区\)在开始此过程之前。 如果你将使用令牌\-签名的证书颁发机构颁发的证书\(CA\)，确认令牌\-使用私钥签名证书已安装或导入到本地证书存储区\(个人存储区\)在开始此过程之前。 或者，可以创建自\-已签名，令牌\-签名证书，证书使用添加角色向导，在此过程中所述。 有关令牌的详细信息\-签名证书，请参阅[联合身份验证服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   
  
#### <a name="to-install-the-federation-service-role-service"></a>安装联合身份验证服务角色服务  
  
1.  上**启动**屏幕上，键入**服务器管理器**，然后按 ENTER。  
  
2.  单击**管理**，然后单击**添加角色和功能**以启动添加角色和功能向导。  
  
3.  在“开始之前”  页上，单击“下一步”  。  
  
4.  上**选择安装类型**页上，单击**角色\-基于或功能\-基于安装**，然后单击**下一步**。  
  
5.  上**选择目标服务器**页上，单击**从服务器池中选择服务器**，验证目标计算机已突出显示，然后单击**下一步**。  
  
6.  上**选择服务器角色**页上，单击**Active Directory 联合身份验证服务**，然后单击下一步。  
  
    > [!NOTE]  
    > 如果系统提示您安装其他.NET Framework 或 Windows 进程激活服务功能，请单击**添加功能**安装它们。  
  
7.  上**选择的功能**页上，验证功能设置，然后单击**下一步**。  
  
8.  上**Active Directory 联合身份验证服务\(AD FS\)** 页上，单击**下一步**。  
  
9. 上**选择角色服务**页上，选择**联合身份验证服务**复选框，然后依次**下一步**。  
  
10. 上**Web 服务器角色\(IIS\)** 页上，单击**下一步**。  
  
11. 在**选择角色服务**页上，单击“**下一步**”。  
  
12. 在验证“确认安装选择”  页面上的信息后，选中“必要时自动重新启动目标服务器”  复选框，然后单击“安装”  。  
  
13. 在“安装进度”  页面上，验证所有内容是否正确安装，然后单击“关闭”  。  
  


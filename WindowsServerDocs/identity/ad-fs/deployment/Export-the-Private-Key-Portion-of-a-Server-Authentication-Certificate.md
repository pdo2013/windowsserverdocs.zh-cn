---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: "导出服务器身份验证证书专用键一部分"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>导出服务器身份验证证书专用键一部分

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

中 Active Directory 联合身份验证服务 \(AD FS\) 电场的日落，每个联盟服务器必须具有访问服务器身份验证证书的专用键。 如果你要实现的联合身份验证的服务器或 Web 服务器服务器场，你必须单个身份验证证书。 必须颁发该证书企业证书颁发机构颁发 \(CA\)，并且必须具有可以导出的专用键。 必须导出服务器身份验证证书的专用键，这样可以进行适用于电场的日落中的所有服务器。  
  
此相同的概念也是如此联盟服务器代理群在感知场中的所有联合 server 代理必须都共享相同服务器身份验证证书的隐私的主要部分。  
  
> [!NOTE]  
> 广告 FS 管理 snap\ 中将称为服务通信证书服务器身份验证证书的联合身份验证的服务器。  
  
将播放该计算机的角色，根据使用此过程联合身份验证的服务器计算机或联合身份验证的服务器代理计算机上安装服务器身份验证证书具有专用键。 完成该过程后，你可以然后导入默认网站电场的日落中每个服务器上的此证书。 有关详细信息，请参阅[将服务器身份验证证书导入默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>服务器身份验证证书的专用部分的导出  
  
1.  在**开始**屏幕上，键入**Internet 信息服务 \(IIS\) 管理器**，然后按 ENTER。  
  
2.  在控制台树中，单击**ComputerName**。  
  
3.  中间窗格中，double\ 单击**服务器证书**。  
  
4.  中间窗格中，单击你想要导出，然后单击证书 right\**导出**。  
  
5.  在**导出证书**对话框中，单击**…** 按钮。  
  
6.  在**文件名**，类型**C:\\***NameofCertificate*，然后单击**打开**。  
  
7.  键入的密码证书，确认它，然后单击**确定**。  
  
8.  验证成功导出的确认指定你的文件已创建的指定位置。  
  
    > [!IMPORTANT]  
    > 以便该证书可导入到新的服务器上的本地证书应用商店中，你必须传输到物理媒体文件，并保护期间传输到新的服务器的安全。 这是非常重要，以保护的安全密钥。 此项受到威胁后，如果你的整个广告 FS 部署的安全性 \（包括资源，在你的组织和资源合作伙伴 organizations\）受到威胁后。  
  
9. 将导出的服务器身份验证证书导入新的服务器上证书官方商城之前安装联合身份验证服务。 有关如何将证书导入的信息，请参阅导入服务器证书 \ ([http:///\/go.microsoft.com\/fwlink\/？LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\)。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[联合身份验证的服务器代理证书要求](https://technet.microsoft.com/library/dd807054.aspx)  
  


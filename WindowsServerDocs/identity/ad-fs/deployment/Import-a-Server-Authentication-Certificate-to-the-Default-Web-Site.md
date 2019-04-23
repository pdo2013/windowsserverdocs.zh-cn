---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: 将服务器身份验证证书导入到默认网站
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880938"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>将服务器身份验证证书导入到默认网站

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从证书颁发机构获取服务器身份验证证书后\(CA\)，您必须手动安装该证书默认 Web 站点上为每个联合服务器或服务器场中的联合服务器代理。  
  
对于 Web 服务器，必须在相应的网站或你的联合应用程序所在的虚拟目录上手动安装服务器身份验证证书。  
  
如果你要设置某个服务器场，请务必在服务器场中的每个服务器上执行同样的过程（使用完全相同的设置）。  
  
> [!NOTE]  
> 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>将服务器身份验证证书导入到默认网站  
  
1.  上**启动**屏幕上，键入**Internet Information Services \(IIS\) Manager**，然后按 ENTER。  
  
2.  在控制台树中，单击“计算机名称”。  
  
3.  在中心窗格中，双击\-单击**服务器证书**。  
  
4.  在“操作”  窗格中，单击“导入” 。  
  
5.  在中**导入证书**对话框中，单击 **...** 按钮。  
  
6.  浏览到 pfx 证书文件的位置，使其突出显示，然后单击“打开” 。  
  
7.  为该证书键入一个密码，然后单击“确定” 。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合身份验证服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合身份验证服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[联合服务器代理的证书要求](https://technet.microsoft.com/library/dd807054.aspx)  
   
  


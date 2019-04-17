---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: "将服务器身份验证证书导入默认网站"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>将服务器身份验证证书导入默认网站

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

向证书颁发机构获取服务器身份验证证书之后 \(CA\)，你必须手动安装该证书的每个联合身份验证的服务器或联合服务器代理服务器场中的将默认网站上。  
  
对于 Web 服务器，必须服务器身份验证证书适当的网站或联合的应用程序所在的目录虚拟手动安装。  
  
如果你要设置电场的日落，请务必执行此过程具有相同，使用精确相同的设置，在每一场中服务器上。  
  
> [!NOTE]  
> 广告 FS 管理 snap\ 中将称为服务通信证书服务器身份验证证书的联合身份验证的服务器。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>若要将服务器身份验证证书导入默认网站  
  
1.  在**开始**屏幕上，键入**Internet 信息服务 \(IIS\) 管理器**，然后按 ENTER。  
  
2.  在控制台树中，单击**ComputerName**。  
  
3.  中间窗格中，double\ 单击**服务器证书**。  
  
4.  在**操作**窗格中，单击**导入**。  
  
5.  在**导入证书**对话框中，单击**…** 按钮。  
  
6.  浏览到 pfx 证书文件的位置，突出显示它，然后单击**打开**。  
  
7.  键入的证书，密码，然后单击**确定**。  
  
## <a name="additional-references"></a>其他参考  
[清单：联合服务器设置](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：联合身份验证的服务器代理服务器设置](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[联合身份验证的服务器代理证书要求](https://technet.microsoft.com/library/dd807054.aspx)  
   
  


---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: 导出服务器身份验证证书的私钥部分
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1a7e59dd83ebc9a9eabd5bda1dc598d320f5028d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442499"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>导出服务器身份验证证书的私钥部分

在 Active Directory 联合身份验证服务中的每个联合身份验证服务器\(AD FS\)场必须具有服务器身份验证证书的私钥访问权限。 如果要实现的联合身份验证服务器或 Web 服务器的服务器场，必须具有单一身份验证证书。 此证书必须由企业证书颁发机构颁发\(CA\)，并且它必须可导出的私钥。 服务器身份验证证书的私钥必须可导出，以便使其可用于该服务器场中的所有服务器。  
  
此相同的概念是正确的意义上说，场中的所有联合服务器代理必须都共享相同的服务器身份验证证书的私钥部分中的联合身份验证服务器代理场。  
  
> [!NOTE]  
> 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。  
  
具体取决于此计算机将扮演哪个角色，使用此过程联合身份验证服务器计算机或联合服务器代理计算机上安装服务器身份验证证书使用私钥。 当完成该过程时，就可以在服务器场中的每个服务器的默认网站上导入此证书。 有关详细信息，请参阅[服务器身份验证证书导入到默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>导出服务器身份验证证书的私钥部分  
  
1. 上**启动**屏幕上，键入**Internet Information Services \(IIS\) Manager**，然后按 ENTER。  
  
2. 在控制台树中，单击“计算机名称”  。  
  
3. 在中心窗格中，双击\-单击**服务器证书**。  
  
4. 在中心窗格中，右键\-单击你想要导出，然后单击该证书**导出**。  
  
5. 在中**导出证书**对话框中，单击 **...** 按钮。  
  
6. 在中**文件名**，类型**c:\\** <em>NameofCertificate</em>，然后单击**打开**。  
  
7. 键入证书的密码、进行确认，然后单击“确定”  。  
  
8. 通过确认指定的文件创建在指定的位置上来验证导出成功。  
  
   > [!IMPORTANT]  
   > 必须将该文件传输到物理媒体，并在传输到新服务器的过程中保护其安全，以便可以将此证书导入到新服务器上的本地证书存储中。 保护私钥的安全非常重要。 如果此密钥已泄漏，整个 AD FS 部署的安全性\(包括在你的组织和资源伙伴组织中的资源\)遭到破坏。  
  
9. 请在安装联合身份验证服务之前，将已导出的服务器身份验证证书导入到新的服务器上的证书存储区。 有关如何将证书导入的信息，请参阅导入服务器证书\( [http:\/\/go.microsoft.com\/fwlink\/？LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\)。  
  
## <a name="additional-references"></a>其他参考  
[清单：设置联合服务器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[清单：设置联合服务器代理](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[联合服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[联合服务器代理的证书要求](https://technet.microsoft.com/library/dd807054.aspx)  
  


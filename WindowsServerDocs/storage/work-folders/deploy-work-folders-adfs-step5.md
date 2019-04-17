---
title: "使用 AD FS 和 Web 应用程序代理部署工作文件夹 - 步骤 5，设置客户端"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: fa8b2b15ff411a59b28308a329d7ca2341ef0886
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 5，设置客户端

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的第五个步骤。 你可以在这些主题中查找这一过程的其他步骤：  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：概述](deploy-work-folders-adfs-overview.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 1，设置 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 2，AD FS 配置后工作](deploy-work-folders-adfs-step2.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 3，设置工作文件夹](deploy-work-folders-adfs-step3.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 4，设置 Web 应用程序代理](deploy-work-folders-adfs-step4.md)  
  
使用以下步骤设置加入域和未加入域的 Windows 客户端。 你可以使用这些客户端测试客户端之间的工作文件夹是否正确同步。  
  
## <a name="set-up-a-domain-joined-client"></a>设置加入域的客户端  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>安装 AD FS 和工作文件夹证书  
你必须在加入域的客户端计算机的本地计算机证书存储中安装之前创建的 AD FS 和工作文件夹证书。  
  
由于你正在安装的自签名证书无法追溯到受信任的根证书颁发机构证书存储中的发布者，你还必须将证书复制到该存储。  
  
若要安装证书，请执行以下步骤：  
  
1.  单击**开始**，然后单击**运行**。  
  
2.  键入 **MMC**。  
  
3.  在**文件**菜单上，单击**添加/删除管理单元**。  
  
4.  在**可用的管理单元**列表中，单击**证书**，然后单击**添加**。 启动证书管理单元向导。  
  
5.  选择**计算机帐户**，然后单击**下一步**。  
  
6.  选择**本地计算机：（运行此控制台的计算机）**，然后单击**完成**。  
  
7.  单击 **OK**。  
  
8.  展开文件夹“控制台根节点\证书\(本地计算机)\个人\证书”。  
  
9. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
10. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在证书存储中。  
  
11. 重复步骤 9 和 10，这一次，浏览到工作文件夹证书并将其导入。  
  
12. 展开文件夹控制台根节点\证书\(本地计算机)\受信任的根证书颁发机构\证书。  
  
13. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
14. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在受信任的根证书颁发机构存储中。  
  
15. 重复步骤 13 和 14，这一次，浏览到工作文件夹证书并将其导入。  
  
### <a name="configure-work-folders-on-the-client"></a>配置客户端上的工作文件夹  
要配置客户端计算机上的工作文件夹，请遵循以下步骤：  
  
1.  在客户端计算机上打开**控制面板**，然后单击**工作文件夹**。  
  
2.  单击**设置工作文件夹**。  
  
3.  在**输入你的工作电子邮件地址**页上，输入该用户的电子邮件地址（例如，user@contoso.com）或工作文件夹 URL（在测试示例中为 https://workfolders.contoso.com），然后单击**下一步**。  
  
4.  如果用户连接到公司网络，则由 Windows 集成身份验证执行身份验证。 如果用户未连接到公司网络，则由 ADFS (OAuth) 执行身份验证，它将提示用户输入凭据。 输入你的凭据，然后单击**确定**。  
  
5.  进行身份验证后，**介绍工作文件夹**页面将显示，你可以在其中选择更改工作文件夹目录的位置。 单击**下一步**。  
  
6.  **安全策略**页列出了你为工作文件夹设置的安全策略。 单击**下一步**。  
  
7.  将显示一条消息，指出工作文件夹已开始与你的电脑同步。 单击**关闭**。  
  
8.  **管理工作文件夹**页可显示服务器上的可用空间量、同步状态等。 如有必要，你可以在此处重新输入凭据。 关闭窗口。  
  
9. 你的“工作文件夹”文件夹将自动打开。 你可以向此文件夹中添加内容，并在设备之间同步。  
  
    为了实现测试示例的目的，请向本“工作文件夹”文件夹添加测试文件。 在未加入域的计算机上设置工作文件夹后，你将能够同步每台计算机上的工作文件夹之间的文件。  
  
## <a name="set-up-a-non-domain-joined-client"></a>设置未加入域的客户端  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>安装 AD FS 和工作文件夹证书  
通过与针对加入域的计算机使用的步骤相同的步骤，在未加入域的计算机上安装 AD FS 和工作文件夹证书。  
  
### <a name="update-the-hosts-file"></a>更新主机文件  
由于未针对工作文件夹创建公用 DNS 记录，因此必须针对测试环境更新未加入域的客户端上的主机文件。 将以下条目添加到主机文件夹中：  
  
-  workfolders.domain  
  
-  AD FS service name.domain  
  
-  enterpriseregistration.domain  
  
对于测验示例，请使用以下值：  
  
-  **10.0.0.10 workfolders.contoso.com**  
  
-  **10.0.0.10 blueadfs.contoso.com**  
  
-  **10.0.0.10 enterpriseregistration.contoso.com**  
  
### <a name="configure-work-folders-on-the-client"></a>配置客户端上的工作文件夹  
通过与针对加入域的计算机使用的相同步骤，在未加入域的计算机上配置工作文件夹。  
  
在此客户端上打开新的“工作文件夹”文件夹时，你可以看到，已加入域的计算机上的文件已经同步到未加入域的计算机中。 你可以开始将内容添加到文件夹中，并在设备之间同步。  
  
以上就是通过 Windows Server UI 部署工作文件夹、AD FS 和 Web 应用程序代理的过程。  
  
## <a name="see-also"></a>另请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
  


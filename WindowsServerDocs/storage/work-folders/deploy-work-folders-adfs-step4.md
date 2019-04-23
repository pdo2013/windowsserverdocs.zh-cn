---
title: 使用 AD FS 和 Web 应用程序代理部署工作文件夹 - 步骤 4，设置 Web 应用程序代理
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 6/242017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 1f452fd1e2f054c449660eb0ee12642fefe4da8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865048"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 4 中，设置 Web 应用程序代理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的第四个步骤。 你可以在这些主题中查找这一过程的其他步骤：  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：概述](deploy-work-folders-adfs-overview.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：第 1 步： 设置 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 2 中，AD FS 配置后工作](deploy-work-folders-adfs-step2.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 3，设置了工作文件夹](deploy-work-folders-adfs-step3.md)  
  
-   [部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 5 中，设置客户端](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   本节中包含的说明适用于 Windows Server 2016 环境。 如果你使用的是 Windows Server 2012 R2，请遵循 [Windows Server 2012 R2 说明](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

要设置用于工作文件夹的 Web 应用程序代理，请使用以下过程。  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>安装 AD FS 和工作文件夹证书  
你必须将先前（在步骤 1“设置 AD FS”和步骤 3“设置工作文件夹”中）创建的 AD FS 和工作文件夹证书安装到计算机上的本地计算机证书存储中，此计算机上将安装 Web 应用程序代理角色。  
  
由于你正在安装的自签名证书无法追溯到受信任的根证书颁发机构证书存储中的发布者，你还必须将证书复制到该存储。  
  
若要安装证书，请执行以下步骤：  
  
1.  单击 **“开始”**，然后单击 **“运行”**。  
  
2.  键入 **MMC**。  
  
3.  在“文件”  菜单上，单击“添加/删除管理单元” 。  
  
4.  在**可用的管理单元**列表中，单击**证书**，然后单击**添加**。 证书管理单元向导启动。  
  
5.  选择“计算机帐户”，然后单击“下一步”。  
  
6.  选择**本地计算机：（运行此控制台的计算机）**，然后单击**完成**。  
  
7.  单击 **“确定”**。  
  
8.  展开文件夹**控制台根节点\证书\(本地计算机)\个人\证书**。  
  
9. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
10. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在证书存储中。  
  
11. 重复步骤 9 和 10，这一次，浏览到工作文件夹证书并将其导入。  
  
12. 展开文件夹**控制台根节点\证书\(本地计算机)\受信任的根证书颁发机构\证书**。  
  
13. 右键单击**证书**，单击**所有任务**，然后单击**导入**。  
  
14. 浏览到包含 AD FS 证书的文件夹，然后遵循向导中的说明导入文件，并将其放置在受信任的根证书颁发机构存储中。  
  
15. 重复步骤 13 和 14，这一次，浏览到工作文件夹证书并将其导入。  
  
## <a name="install-web-application-proxy"></a>安装 Web 应用程序代理  
要安装 Web 应用程序代理，请遵循下列步骤：  
  
1.  在计划安装 Web 应用程序代理的服务器上，打开**服务器管理器**并启动**添加角色和功能**向导。  
  
2.  在向导的第一页和第二页上单击**下一步**。  
  
3.  在**服务器选择**页上，选择你的服务器，然后单击**下一步**。  
  
4.  在**服务器角色**页上，选择**远程访问**角色，然后单击**下一步**。  
  
5.  在“功能”页和“远程访问”页上，单击**下一步**。  
  
6.  在**角色服务**页上，选择 **Web 应用程序代理**，单击**添加功能**，然后单击**下一步**。

7.  在 **“确认安装选择”** 页上，单击 **“安装”**。  
  
## <a name="configure-web-application-proxy"></a>配置 Web 应用程序代理  
要配置 Web 应用程序代理，请遵循下列步骤：  
  
1.  单击服务器管理器顶部的警告标志，然后单击链接以打开 Web 应用程序代理配置向导。  
  
2.  在“欢迎”页上，按**下一步**。  
  
3.  在**联合服务器**页上，输入联合身份验证服务的名称。 在测试示例中，此为 **blueadfs.contoso.com**。  
  
4.  在联合服务器上输入本地管理员帐户的凭据。 不要输入域凭据（例如 contoso\administrator），而是输入本地凭据（例如 administrator）。  
  
5.  在 **AD FS 代理证书**页上，选择之前导入的 AD FS 证书。 在测试用例中，证书为 **blueadfs.contoso.com**。 单击“下一步” 。  
  
6.  确认页显示将执行以配置服务的 Windows PowerShell 命令。 单击 **“配置”**。  
  
## <a name="publish-the-work-folders-web-application"></a>发布工作文件夹 Web 应用程序  
下一步是发布 Web 应用程序，它会使工作文件夹可用于客户端。 要发布工作文件夹 Web 应用程序，请遵循下列步骤：  
  
1.  打开**服务器管理器**，并在**工具**菜单上，单击**远程访问管理**以打开远程访问管理控制台。  
  
2.  在**配置**下，单击 **Web 应用程序代理**。  
  
3.  在**任务**下，单击**发布**。 “发布新应用程序向导”将打开。  
  
4.  在“欢迎”页上，单击**下一步**。  
  
5.  在**预身份验证**页上，选择 **Active Directory 联合身份验证服务 (AD FS)**，然后单击**下一步**。  
  
6.  在**支持客户端**页上，选择 **OAuth2**，然后单击**下一步**。

7.  在**依赖方**页上，选择**工作文件夹**，然后单击**下一步**。 该列表从 AD FS 发布到 Web 应用程序代理。  
  
8.  在**发布设置**页上输入以下内容，然后单击**下一步**：  
  
    -   你要用于 Web 应用程序的名称  
  
    -   工作文件夹的外部 URL  
  
    -   工作文件夹证书的名称  
  
    -   工作文件夹的后端 URL  
  
    默认情况下，向导会使后端 URL 与外部 URL 相同。  
  
    对于测试示例，请使用以下值：  
  
    名称：**WorkFolders**  
  
    外部 URL: **https://workfolders.contoso.com**  
  
    外部证书：**前面安装的工作文件夹证书**  
  
    后端服务器 URL: **https://workfolders.contoso.com**  
  
9.  确认页显示将执行以发布应用程序的 Windows PowerShell 命令。 单击“发布” 。  
  
10. 在**结果**页上，你应该看到该应用程序已成功发布。
   >[!NOTE]
   > 如果拥有多个工作文件夹服务器，则需要为每个工作文件夹服务器发布一个工作文件夹 Web 应用程序（重复步骤 1-10）。  
  
下一步：[部署工作文件夹使用 AD FS 和 Web 应用程序代理：步骤 5 中，设置客户端](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
  


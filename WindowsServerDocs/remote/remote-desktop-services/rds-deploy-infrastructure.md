---
title: 部署远程桌面环境
ms.custom: na
ms.prod: windows-server-threshold
description: 若要部署远程桌面环境的基本步骤。
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 5b9ce1bb87a7a2ad8819235edc412fd095bc2985
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805128"
---
# <a name="deploy-your-remote-desktop-environment"></a>部署远程桌面环境

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

使用以下步骤来部署你的环境中的远程桌面服务器。 可以在物理计算机或虚拟机，具体取决于您在创建的本地基于云或混合环境来安装服务器角色。 

如果将虚拟机的任何远程桌面服务服务器，请确保您具有[准备好这些虚拟机](rds-prepare-vms.md)。
  
  
1.  添加了您将使用远程桌面服务服务器管理器的所有服务器：  
    1.  在服务器管理器中，单击**管理** > **添加服务器**。  
    2.  单击**立即查找**。  
    3.  单击部署 （例如，Contoso Cb1、 Contoso-WebGw1 和 Contoso Sh1） 中的每个服务器，然后单击**确定**。  
2.  创建基于会话的部署来部署远程桌面服务组件：  
    1.  在服务器管理器中，单击**管理** > **添加角色和功能**。  
    2.  单击**远程桌面服务安装**，**标准部署**，并**基于会话的桌面部署**。  
    3.  选择用于 RD 连接代理服务器、 RD Web 访问服务器和 RD 会话主机服务器的相应服务器 (例如，Contoso Cb1、 Contoso-WebGw1 和 Contoso SH1，分别)。  
    4.  选择**如果需要自动重新启动目标服务器**，然后单击**部署**。  
    5.  等待部署成功完成  
3.  添加远程桌面许可证服务器：  
    1.  在服务器管理器中，单击**远程桌面服务 > 概述 > + RD 授权**。  
    2.  选择将其中安装远程桌面许可证服务器 （例如，Contoso Cb1） 的虚拟机。  
    3.  单击**下一步**，然后单击**添加**。  
4.  启用远程桌面许可证服务器并将其添加到 License Servers 组：  
    1.  在服务器管理器中，单击**工具 > 终端服务 > 远程桌面授权管理器**。  
    2.  在 RD 授权管理器中，选择服务器，然后单击**操作 > 激活服务器**。  
    3.  接受默认值在服务器激活向导接受默认设置，直到你达到**公司信息**页。 然后，输入你的公司信息。  
    4.  之前的最后一页接受其余页的默认值。 清除**立即启动许可证安装向导**，然后单击**完成**。  
    5.  单击**操作 > 查看配置 > 将添加到组 > 确定**。 为 AAD DC 管理员组中的用户输入凭据并将注册为 SCP。 如果你正在使用 Azure AD 域服务，但你可以忽略任何警告或错误，此步骤可能无法工作。  
5.  添加 RD 网关服务器和证书名称：  
    1.  在服务器管理器中，单击**远程桌面服务 > 概述 > + RD 网关**。  
    2.  在添加 RD 网关服务器向导中，选择你想要安装 RD 网关服务器 (例如，Contoso WebGw1) 的虚拟机。  
    3.  输入使用 RD 网关服务器外部完全限定 DNS 名称 (FQDN) 在 RD 网关服务器的 SSL 证书名称。 在 Azure 中，这是**DNS 名称**标签，并使用格式 servicename.location.cloudapp.azure.com。 例如，contoso.westus.cloudapp.azure.com。  
    4.  单击**下一步**，然后单击**添加**。
6.  创建并安装自签名的证书用于 RD 网关和 RD 连接代理服务器。

       > [!NOTE]
       > 如果您是提供并从受信任的证书颁发机构安装证书，从步骤 h，单步 k 为每个角色执行的过程。 您需要为每个这些证书具有可用的.pfx 文件。
       
    1.  在服务器管理器中，单击**远程桌面服务 > 概述 > 任务 > 编辑部署属性**。  
    2.  展开**证书**，然后向下滚动到表。 单击**RD 网关 > 创建新证书**。  
    3.  输入证书名称，使用的 RD 网关服务器 (例如，contoso.westus.cloudapp.azure.com) 外部 FQDN，然后输入密码。  
    4.  选择**将此证书存储**，然后浏览到为上一步中的证书创建的共享文件夹。 （例如 \Contoso Cb1\Certificates。）  
    5.  输入证书 (例如，ContosoRdGwCert) 的文件名称，然后单击**保存**。  
    6.  选择**允许证书添加到目标计算机上的受信任的根证书颁发机构证书存储**，然后单击**确定**。  
    7.  单击**应用**，然后等待要成功应用于 RD 网关服务器的证书。  
    8.  单击**RD Web 访问 > 选择现有证书**。  
    9.  浏览到为 RD 网关服务器 (例如，ContosoRdGwCert) 创建的证书，然后单击**打开**。  
    10. 输入选择的证书的密码**允许证书添加到目标计算机上的受信任的根证书存储**，然后单击**确定**。  
    11. 单击**应用**，然后等待要成功应用于 RD Web 访问服务器的证书。  
    12. 重复 substeps 1-11 for **RD 连接代理-启用单一登录**并**RD 连接代理-发布服务**，使用新的 RD 连接代理服务器的内部 FQDN证书的名称 (例如，Contoso Cb1.Contoso.com)。  
7.  导出自签名公共证书并将它们复制到客户端计算机。 如果要使用从受信任的证书颁发机构的证书，可以跳过此步骤。  
    1.  启动 certlm.msc。  
    2.  展开**个人**，然后单击**证书**。  
    3.  在右侧窗格中右键单击适用于客户端身份验证，例如的 RD 连接代理证书**Contoso Cb1.Contoso.com**。  
    4.  单击**的所有任务 > 导出**。  
    5.  接受证书导出向导中的默认选项接受默认值，直到你达到**导出的文件**页。  
    6.  浏览到证书，例如 \Contoso-Cb1\Certificates 创建的共享文件夹。  
    7.  输入文件名，例如 ContosoCbClientCert，然后依次**保存**。  
    8.  单击“下一步”  ，然后单击“完成”  。  
    9.  重复子步骤 1-8 RD 网关和 Web 证书，(例如 contoso.westus.cloudapp.azure.com)，为导出的证书相应的文件的名称，例如**ContosoWebGwClientCert**。  
    10. 在文件资源管理器，导航到的文件夹，其中存储的证书，例如 \Contoso-Cb1\Certificates。  
    11. 选择两个导出的客户端证书，然后右键单击它们，然后单击**复制**。  
    12. 将证书粘贴本地客户端计算机上。  
8.  配置 RD 网关和 RD 许可的部署属性：  
    1.  在服务器管理器中，单击**远程桌面服务 > 概述 > 任务 > 编辑部署属性**。  
    2.  展开**RD 网关**并清除**对于本地地址绕过 RD 网关服务器**选项。  
    3.  展开**RD 授权**，然后选择**每个用户**  
    4.  单击 **“确定”** 。  
10. 创建会话集合。 这些步骤创建一个基本集合。 请查看[创建适用于桌面和应用程序以运行远程桌面服务集合](rds-create-collection.md)有关集合的详细信息。
 
    1.  在服务器管理器中，单击**远程桌面服务 > 集合 > 任务 > 创建会话集合**。  
    2.  输入集合名称 (例如，ContosoDesktop)。  
    3.  选择 RD 会话主机服务器 (Contoso-Sh1)，接受默认用户组 （Contoso\Domain 用户），然后输入上面 (\Contoso-Cb1\UserDisks) 创建的用户配置文件磁盘的通用命名约定 (UNC) 路径。  
    4.  设置最大大小，然后单击**创建**。  
  

你现在已创建一个基本的远程桌面服务基础结构。 如果您需要创建高度可用的部署，您可以添加[连接代理群集](rds-connection-broker-cluster.md)或[第二个 RD 会话主机服务器](rds-scale-rdsh-farm.md)。


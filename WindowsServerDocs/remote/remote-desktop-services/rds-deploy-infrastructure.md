---
title: 部署远程桌面环境
ms.custom: na
ms.prod: windows-server-threshold
description: 部署远程桌面环境的基本步骤。
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805128"
---
# <a name="deploy-your-remote-desktop-environment"></a>部署远程桌面环境

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

使用以下步骤在你的环境中部署远程桌面服务器。 可以在物理计算机或虚拟机上安装服务器角色，具体取决于是创建本地环境、基于云的环境还是混合环境。 

如果要将虚拟机用于任何远程桌面服务服务器，请确保已[让这些虚拟机做好准备](rds-prepare-vms.md)。
  
  
1.  向服务器管理器添加要用于远程桌面服务的所有服务器：  
    1.  在服务器管理器中，单击“管理” > “添加服务器”   。  
    2.  单击“立即查找”  。  
    3.  单击部署中的每个服务器（例如，Contoso-Cb1、Contoso-WebGw1 和 Contoso-Sh1），然后单击“确定”  。  
2.  创建基于会话的部署，以部署远程桌面服务组件：  
    1.  在服务器管理器中，单击“管理” > “添加角色和功能”   。  
    2.  单击“远程桌面服务安装”、“标准部署”和“基于会话的桌面部署”    。  
    3.  为 RD 连接代理服务器、RD Web 访问服务器和 RD 会话主机服务器（例如，分别为 Contoso-Cb1、Contoso-WebGw1 和 Contoso-SH1）选择相应的服务器。  
    4.  选择“如果需要，自动重新启动目标服务器”，然后单击“部署”   。  
    5.  等待部署成功完成  
3.  添加 RD 许可证服务器：  
    1.  在服务器管理器中，单击“远程桌面服务”>“概述”>“+ RD 许可证”  。  
    2.  选择将安装 RD 许可证服务器的虚拟机（例如，Contoso-Cb1）。  
    3.  单击“下一步”，然后单击“添加”   。  
4.  激活 RD 许可证服务器并将其添加到“许可证服务器”组：  
    1.  在服务器管理器中，单击“工具”>“终端服务”>“远程桌面许可管理器”  。  
    2.  在 RD 许可管理器中，选择服务器，然后单击“操作”>“激活服务器”  。  
    3.  接受“激活服务器”向导中的默认值，直至到达“公司信息”页  。 然后，输入公司信息。  
    4.  接受剩余页面的默认值，直至最后一页。 清除“立即启动许可证安装向导”复选框，然后单击“完成”   。  
    5.  单击“操作”>“查看配置”>“添加到组”>“确定”  。 在 AAD DC 管理员组中输入用户的凭据，并注册为 SCP。 如果使用 Azure AD 域服务，则此步骤可能不起作用，但可以忽略任何警告或错误。  
5.  添加 RD 网关服务器和证书名称：  
    1.  在服务器管理器中，单击“远程桌面服务”>“概述”>“+ RD 网关”  。  
    2.  在“添加 RD 网关服务器”向导中，选择想要在其中安装 RD 网关服务器的虚拟机（例如，Contoso-WebGw1）。  
    3.  使用 RD 网关服务器的外部完全限定 DNS 名称 (FQDN) 输入 RD 网关服务器的 SSL 证书名称。 在 Azure 中，这是 **DNS 名称**标签，并使用 servicename.location.cloudapp.azure.com 的格式。 例如，contoso.westus.cloudapp.azure.com。  
    4.  单击“下一步”，然后单击“添加”   。
6.  为 RD 网关和 RD 连接代理服务器创建并安装自签名证书。

       > [!NOTE]
       > 如果要从受信任的证书颁发机构提供并安装证书，请对每个角色执行步骤 h 到步骤 k 的过程。 需要为其中每个证书提供 .pfx 文件。
       
    1.  在服务器管理器中，单击“远程桌面服务”>“概述”>“任务”>“编辑部署属性”  。  
    2.  展开“证书”，然后向下滚动到表  。 单击“RD 网关”>“创建新证书”  。  
    3.  使用 RD 网关服务器的外部 FQDN 输入证书名称（例如，contoso.westus.cloudapp.azure.com），然后输入密码。  
    4.  选择“存储此证书”，然后浏览到在上一步中为证书创建的共享文件夹  。 （例如，\Contoso-Cb1\Certificates。）  
    5.  输入证书的文件名（例如，ContosoRdGwCert），然后单击“保存”  。  
    6.  选择“允许向目标计算机上受信任的根证书颁发机构证书存储中添加证书”，然后单击“确定”   。  
    7.  单击“应用”，然后等待证书成功应用于 RD 网关服务器  。  
    8.  单击“RD Web 访问”>“选择现有证书”  。  
    9.  浏览到为 RD 网关服务器创建的证书（例如，ContosoRdGwCert），然后单击“打开”  。  
    10. 输入证书的密码，选择“允许向目标计算机上受信任的根证书存储中添加证书”，然后单击“确定”   。  
    11. 单击“应用”，然后等待证书成功应用于 RD Web 访问服务器  。  
    12. 对**RD 连接代理 - 启用单一登录**和 **RD 连接代理 - 发布服务**重复子步骤 1-11，并将 RD 连接代理服务器的内部 FQDN 用于新证书的名称（例如，Contoso-Cb1.Contoso.com）。  
7.  导出自签名公共证书并将其复制到客户端计算机。 如果使用受信任的证书颁发机构的证书，则可以跳过此步骤。  
    1.  启动 certlm.msc。  
    2.  展开“个人”，然后单击“证书”   。  
    3.  在右侧窗格中，右键单击要用于客户端身份验证的 RD 连接代理证书，例如 **Contoso-Cb1.Contoso.com**。  
    4.  单击“所有任务”>“导出”  。  
    5.  接受“证书导出”向导中的默认选项，并一直接受默认值，直至到达“要导出的文件”页  。  
    6.  浏览到为证书创建的共享文件夹，例如，\Contoso-Cb1\Certificates。  
    7.  输入文件名（例如，ContosoCbClientCert），然后单击“保存”  。  
    8.  单击“下一步”  ，然后单击“完成”  。  
    9.  对 RD 网关和 Web 证书（例如，contoso.westus.cloudapp.azure.com）重复子步骤 1-8，为导出的证书提供相应的文件名，例如，**ContosoWebGwClientCert**。  
    10. 在文件资源管理器中，导航到存储证书的文件夹，例如，\Contoso-Cb1\Certificates。  
    11. 选择两个导出的客户端证书，右键单击它们，然后单击“复制”  。  
    12. 将证书粘贴到本地客户端计算机。  
8.  配置 RD 网关和 RD 许可部署属性：  
    1.  在服务器管理器中，单击“远程桌面服务”>“概述”>“任务”>“编辑部署属性”  。  
    2.  展开“RD 网关”并清除“不对本地地址使用 RD 网关服务器”选项   。  
    3.  展开“RD 许可”并选择“每用户”    
    4.  单击“确定”  。  
10. 创建会话集合。 这些步骤可创建基本集合。 查看[创建远程桌面服务集合以便桌面和应用运行](rds-create-collection.md)，以获取有关集合的详细信息。
 
    1.  在服务器管理器中，单击“远程桌面服务”>“集合”>“任务”>“创建会话集合”  。  
    2.  输入集合名称（例如，ContosoDesktop）。  
    3.  选择 RD 会话主机服务器 (Contoso-Sh1)，接受默认用户组 (Contoso\Domain Users)，然后输入上面创建的用户配置文件磁盘的通用命名约定 (UNC) 路径 (\Contoso-Cb1\UserDisks)。  
    4.  设置最大大小，然后单击“创建”  。  
  

现在已经创建一个基本的远程桌面服务基础结构。 如果需要创建高度可用的部署，则可以添加[连接代理群集](rds-connection-broker-cluster.md)或[第二个 RD 会话主机服务器](rds-scale-rdsh-farm.md)。


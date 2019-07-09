---
title: 向 RD Web 和网关 Web 前端添加高可用性
description: 提供在 RDS 部署中安装 RD Web 和网关服务器的步骤。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: 4e185e51b09d2e2f8ac4527f9de339de27e02f24
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805146"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>向 RD Web 和网关 Web 前端添加高可用性

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016


可以部署远程桌面 Web 访问（RD Web 访问）和远程桌面网关（RD 网关）场以提高 Windows Server 远程桌面服务 (RDS) 部署的可用性并扩大其规模 

使用以下步骤向现有远程桌面服务基本部署添加 RD Web 和网关服务器。  

## <a name="pre-requisites"></a>先决条件

设置用于充当附加 RD Web 和 RD 网关的服务器，这可以是物理服务器，也可以是 VM。 此操作包括将服务器加入域并启用远程管理。

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>第 1 步：将新服务器配置为 RDS 环境的一部分

1. 使用远程桌面连接客户端连接到 Azure 门户中的 RDMS 服务器。
2. 向服务器管理器添加新的 RD Web 和网关服务器：
    1. 启动服务器管理器，单击“管理”>“添加服务器”  。   
    2. 在“添加服务器”对话框中，单击“立即查找”  。   
    3. 选择新创建的 RD Web 和网关服务器（例如，Contoso-WebGw2），然后单击“确定”  。
3. 向部署添加 RD Web 和网关服务器  
    1. 启动服务器管理器。  
    2. 单击“远程桌面服务”>“概述”>“部署服务器”>“任务”>“添加 RD Web 访问服务器”  。   
    3. 选择新创建的服务器（例如，Contoso-WebGw2），然后单击”下一步“  。  
    4. 在“确认”页上，选择“根据需要重启远程计算机”，然后单击“添加”   。  
    5. 重复这些步骤以添加 RD 网关服务器，但在步骤 b 中选择“RD 网关服务器”  。
4. 重新安装 RD 网关服务器的证书：
   1. 在 RDMS 服务器上的服务器管理器中，单击“远程桌面服务”>“概述”>“任务”>“编辑部署属性”  。  
   2. 展开“证书”  。  
   3. 向下滚动到表。 单击“RD 网关角色服务”>“选择现有证书”  。  
   4. 单击“选择其他证书”，然后浏览到证书位置  。 例如，\Contoso-CB1\Certificates。 选择在先决条件检查期间创建的 RD Web 和网关服务器的证书文件（例如，ContosoRdGwCert），然后单击“打开”  。  
   5. 输入证书的密码，选择“允许向目标计算机上受信任的根证书颁发机构证书存储中添加证书”，然后单击“确定”   。  
   6. 单击 **“应用”** 。
      > [!NOTE] 
      > 可能需要通过服务器管理器或任务管理器来手动重启在每个 RD 网关服务器上运行的 TSGateway 服务。
   7. 对 RD Web 访问角色服务重复步骤 a 到 f。

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>步骤 2：在新服务器上配置 RD Web 和 RD 网关属性
1. 将服务器配置为 RD 网关场的一部分：
    1.  在 RDMS 服务器上的服务器管理器中，单击“所有服务器”  。 右键单击其中一个 RD 网关服务器，然后单击“远程桌面连接”  。
    2.  使用域管理员帐户登录 RD 网关服务器。  
    3.  在 RD 网关服务器上的服务器管理器中，单击“工具”>“远程桌面服务”>“RD 网关管理器”  。  
    4.  在导航窗格中，单击本地计算机（例如，Contoso-WebGw1）。  
    5.  单击“添加 RD 网关服务器场成员”  。  
    6.  在“服务器场”选项卡上，输入每个 RD 网关服务器的名称，然后单击“添加”和“应用”    。  
    7.  在每个 RD 网关服务器上重复步骤 a 到 f，以便它们相互识别为服务器场中的 RD 网关服务器。 如果出现警告，请不要惊慌，因为 DNS 设置可能需要一段时间才能传播。
2. 将服务器配置为 RD Web 访问场的一部分。 以下步骤在两个 RDWeb 站点上配置相同的验证和解密计算机密钥。
    1.  在 RDMS 服务器上的服务器管理器中，单击“所有服务器”  。 右键单击第一个 RD Web 访问服务器（例如，Contoso-WebGw1），然后单击“远程桌面连接”  。  
    2.  使用域管理员帐户登录 RD Web 访问服务器。  
    3.  在 RD Web 访问服务器上的服务器管理器中，单击“工具”>“Internet Information Services (IIS) 管理器”  。  
    4.  在 IIS 管理器的左侧窗格中，展开“服务器”（例如，Contoso-WebGw1）>“站点”>“默认网站”，然后单击“RDWeb”   。  
    5.  右键单击“计算机密钥”，然后单击“打开功能”   。
    6.  在“计算机密钥”页的“操作”窗格中，选择“生成密钥”，然后单击“应用”    。
    7.  复制验证密钥（可右键单击该密钥，然后单击“复制”）  。
    8.  在 IIS 管理器的“默认网站”下，依次选择“Feed”、“FeedLogon”和“Pages”     。
    9. 对于每个选项：
        1.  右键单击“计算机密钥”，然后单击“打开功能”   。
        2.  对于验证密钥，清除“运行时自动生成”复选框，然后粘贴在步骤 g 中复制的密钥  。
    10.  最小化此 RD Web 服务器的“RD 连接”窗口。  
    11.  对第二个 RD Web 访问服务器重复步骤 b 到 e，在“计算机密钥”功能视图结束操作  。
    12. 对于验证密钥，清除“运行时自动生成”复选框，然后粘贴在步骤 g 中复制的密钥  。
    13. 单击 **“应用”** 。
    14. 对“RDWeb”、“Feed”、“FeedLogon”和“Pages”页完成此过程     。
    15. 最小化第二个 RD Web 访问服务器的“RD 连接”窗口，然后最大化第一个 RD Web 访问服务器的“RD 连接”窗口。  
    16. 重复步骤 g 到 n 以复制解密密钥。
    17. 两个 RD Web 访问服务器上的“RDWeb”、“Feed”、“FeedLogon”和“Pages”页的验证密钥和解密密钥相同时，请注销所有“RD”连接窗口     。

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>步骤 3:配置 RD Web 和 RD 网关服务器的负载均衡

如果使用 Azure 基础结构，则可以创建外部 Azure 负载均衡器；如果未使用，则可以设置单独的硬件或软件负载均衡器。 负载均衡是关键所在，可使流量在从远程桌面客户端到 RD 网关，再到用户将运行其工作负荷的服务器的长期连接中均匀分布。

> [!NOTE] 
> 如果之前运行 RD Web 和 RD 网关的服务器已在外部负载均衡器后进行了设置，请跳至步骤 4，选择现有后端池，然后将新服务器添加到池中。

1.  创建 Azure 负载均衡器：  
    1.  在 Azure 门户中，单击“浏览”>“负载均衡器”>“添加”  。  
    2.  输入名称，例如，**WebGwLB**。  
    3.  为“方案”选择“公共”，并为“公共 IP 地址”选择一个**公共 IP 地址**    。 可以选择现有的公共 IP 地址，也可以创建新的 IP 地址。 
    4.  选择相应的“订阅”、“资源组”和“位置”    。
    5.  单击“创建”  。  
2. 创建[探测](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) 用于监视处于活动状态的服务器：  
    1.  在 Azure 门户中，单击“浏览”>“负载均衡器”，然后单击刚才创建的负载均衡器（例如，WebGwLB）和“设置”  。  
    2.  单击“探测”>“添加”  。  
    3.  为探测输入名称，例如，**HTTPS**。 选择“TCP”作为“协议”，为“端口”输入 **443**，然后单击“确定     。   
3.  创建 HTTPS 和 UDP 负载均衡规则：  
    1.  在“设置”中，单击“负载均衡规则”   。  
    2.  为“HTTPS 规则”选择“添加”   。  
    3.  输入规则的名称（例如，HTTPS），然后为“协议”选择“TCP”   。 为“端口”和“后端端口”输入 **443**，然后单击“确定”    。  
    4.  在“负载均衡规则”中，单击“添加”以获取“UDP 规则”    。  
    5.  输入规则的名称（例如，**UDP**），然后为“协议”选择“UDP”   。 为“端口”和“后端端口”输入 **3391**，然后单击“确定”    。  
4. 为 RD Web 和 RD 网关服务器创建后端池：
      1. 在“设置”中，单击“后端地址池”>“添加”   。   
      2. 输入名称（例如，**WebGwBackendPool**），然后单击“添加虚拟机”  。  
      3. 选择可用性集（例如，WebGwAvSet），然后单击“确定”  。   
      4. 单击“选择虚拟机”，选择各个虚拟机，然后单击“选择”>“确定”>“确定”   。

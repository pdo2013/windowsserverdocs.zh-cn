---
title: 添加 RD 连接代理服务器用于在 RDS 中配置高可用性
description: 了解如何将 RD 连接代理添加到 RDS 部署以实现高可用性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: b1e5726e3976527278b11f105007a32548da0bc4
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805153"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>添加 RD 连接代理服务器以部署和配置高可用性

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以部署一个远程桌面连接代理（RD 连接代理）群集，用于提高远程桌面服务基础结构的可用性和可伸缩性。 

## <a name="pre-requisites"></a>先决条件

设置一个服务器充当另一个 RD 连接代理 - 可以是物理服务器或 VM。

设置连接代理的数据库。 可以在本地环境中使用 [Azure SQL 数据库](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database)实例或 SQL Server。 下面讨论如何使用 Azure SQL，但这些步骤也适用于 SQL Server。 需要找到该数据库的连接字符串，并确保已安装适当的 ODBC 驱动程序。

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>第 1 步：配置连接代理的数据库

1. 找到所创建的数据库的连接字符串 - 稍后在配置连接代理本身时（步骤 3），需要使用该连接字符串来识别所需 ODBC 驱动程序的版本，因此，请将该字符串保存到可方便参考的位置。 下面说明了如何查找 Azure SQL 的连接字符串：  
    1. 在 Azure 门户中，单击“浏览”>“资源组”，然后单击部署的资源组。    
    2. 选择刚刚创建的 SQL 数据库（例如 CB-DB1）。   
    3. 单击“设置” > “属性” > “显示数据库连接字符串”。      
    4. 复制“ODBC (包括 Node.js)”的连接字符串，如下所示：    
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. 请将“your_password_here”替换为实际密码。 连接到数据库时，将使用此整个字符串以及包含的密码。 
2. 在新的连接代理上安装 ODBC 驱动程序： 
   1. 如果将 VM 用于连接代理，请为第一个 RD 连接代理创建公共 IP 地址。 （仅当 RDMS 虚拟机尚不包含用于建立 RDP 连接的公共 IP 地址时，才需要执行此操作。）
       1. 在 Azure 门户中，单击“浏览” > “资源组”，单击部署的资源组，然后单击第一个 RD 连接代理虚拟机（例如 Contoso-Cb1）。  
       2. 单击“设置”>“网络接口”，然后单击相应的网络接口。 
       3. 单击“设置”>“IP 地址”。 
       4. 对于“公共 IP 地址”，请选择“已启用”，然后单击“IP 地址”。   
       5. 若要使用现有的公共 IP 地址，请从列表中选择它。 否则，请单击“新建”并输入名称，然后依次单击“确定”、“保存”。   
   2. 连接到第一个 RD 连接代理：
       1. 在 Azure 门户中，单击“浏览” > “资源组”，单击部署的资源组，然后单击第一个 RD 连接代理虚拟机（例如 Contoso-Cb1）。  
       2. 单击“连接”>“打开”以打开远程桌面客户端。 
       3. 在客户端中单击“连接”，然后单击“使用另一个用户帐户”   。 输入域管理员帐户的用户名和密码。
       4. 收到有关证书的警告时，请单击“是”  。
   3. 下载与 ODBC 连接字符串中的版本匹配的[适用于 SQL Server 的 ODBC 驱动程序](https://www.microsoft.com/download/confirmation.aspx?id=50420)。 对于上面的示例字符串，我们需要安装 ODBC 驱动程序版本 13。
   4. 将 Sqlincli.msi 文件复制到第一个 RD 连接代理服务器。   
   5. 打开 sqlincli.msi 文件并安装本机客户端。  
   6. 针对其他每个 RD 连接代理（例如，Contoso-Cb2）重复步骤 1-5。
   7. 在要运行连接代理的每个服务器上安装 ODBC 驱动程序。

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>步骤 2：在 RD 连接代理上配置负载均衡 

如果使用 Azure 基础结构，可以创建 [Azure 负载均衡器](#create-a-load-balancer)；否则，可以设置 [DNS 轮循机制](#configure-dns-round-robin)。

### <a name="create-a-load-balancer"></a>创建负载均衡器  
1. 创建 Azure 负载均衡器   
      1. 在 Azure 门户中，单击“浏览”>“负载均衡器”>“添加”。    
      2. 输入新负载均衡器的名称（例如 hacb）。   
      3. 为“方案”选择“内部”，选择部署的**虚拟网络**（例如 Contoso-VNet），并选择包含所有资源的**子网**（例如 default）。     
      4. 为“IP 地址分配”选择“静态”，并输入当前未使用的**专用 IP 地址**（例如 10.0.0.32）。     
      5. 选择适当的**订阅**、包含所有资源的**资源组**，以及适当的**位置**。   
      6. 选择“创建”。    
2. 创建一个[探测](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)用于监视哪些服务器处于活动状态：   
      1. 在 Azure 门户中，单击“浏览”>“负载均衡器”，然后单击刚刚创建的负载均衡器（例如 CBLB）。  单击“设置”  。   
      2. 单击“探测”>“添加”。    
      3. 输入探测的名称（例如 **RDP**），选择“TCP”作为**协议**，输入 **3389** 作为**端口**，然后单击“确定”。     
3. 创建连接代理的后端池：   
      1. 在“设置”中，单击“后端地址池”>“添加”。     
      2. 输入名称（例如 CBBackendPool），然后单击“添加虚拟机”。   
      3. 选择可用性集（例如 CbAvSet），然后单击“确定”。    
      3. 单击“选择虚拟机”，选择每个虚拟机，然后单击“选择”>“确定”>“确定”。     
4. 创建 RDP 负载均衡规则：   
      1. 在“设置”中单击“负载均衡规则”，然后单击“添加”。      
      2. 输入名称（例如 RDP），为“协议”选择“TCP”作为协议，为“端口”和“后端端口”输入 **3389**，然后单击“确定”。        
5. 添加负载均衡器的 DNS 记录：   
      1. 连接到 RDMS 服务器虚拟机（例如 Contoso-CB1）。 有关连接到 VM 的步骤，请查看[准备 RD 连接代理 VM](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) 一文。   
      2. 在服务器管理器中，单击“工具”>“DNS”。    
      3. 在左侧窗格中展开“DNS”，单击 DNS 计算机，单击“正向查找区域”，然后单击你的域名（例如 Contoso.com）。   （可能需要花费几秒钟时间来处理对 DNS 服务器发出的信息查询。）  
      4. 单击“操作”>“新建主机(A 或 AAAA)”。    
      9. 输入前面指定的名称（例如 hacb）和 IP 地址（例如 10.0.0.32）。   

### <a name="configure-dns-round-robin"></a>配置 DNS 轮循机制  
  
下面是创建 Azure 内部负载均衡器的替代步骤。   
  
1. 在 Azure 门户中连接到 RDMS 服务器。 使用远程桌面连接客户端   
2. 创建 DNS 记录：   
      1. 在服务器管理器中，单击“工具”>“DNS”。    
      2. 在左侧窗格中展开“DNS”，单击 DNS 计算机，单击“正向查找区域”，然后单击你的域名（例如 Contoso.com）。   （可能需要花费几秒钟时间来处理对 DNS 服务器发出的信息查询。）  
      3. 依次单击“操作”、“新建主机(A 或 AAAA)”。     
      4. 输入 RD 连接代理群集的 **DNS 名称**（例如 hacb），然后输入第一个 RD 连接代理的 **IP 地址**。   
      5. 针对其他每个 RD 连接代理重复步骤 3-4，并提供其他每条记录的唯一 IP 地址。


例如，如果两个 RD 连接代理虚拟机的 IP 地址分别是 10.0.0.8 和 10.0.0.9，请创建以下两条 DNS 主机记录：
 - 主机名：hacb.contoso.com，IP 地址：10.0.0.8
 - 主机名：hacb.contoso.com，IP 地址：10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>步骤 3:对连接代理进行高可用性配置

1. 将新的 RD 连接代理服务器添加到服务器管理器：
   1. 在服务器管理器中，单击“管理”>“添加服务器”。 
   2. 单击“立即查找”。 
   3. 单击新建的 RD 连接代理服务器（例如 Contoso-Cb2），然后单击“确定”。 
2. 对 RD 连接代理进行高可用性配置：
   1. 在服务器管理器中，单击“远程桌面服务”>“概述”。 
   2. 右键单击“RD 连接代理”，然后单击“配置高可用性”。  
   3. 完成向导的每个页面，直到进入“配置类型”部分。 选择“共享数据库服务器”，然后单击“下一步”。  
   4. 输入 RD 连接代理群集的 DNS 名称。
   5. 输入 SQL 数据库的连接字符串，然后完成向导的每个页面以建立高可用性。
3. 将新的 RD 连接代理添加到部署
   1. 在服务器管理器中，单击“远程桌面服务”>“概述”。 
   2. 右键单击“RD 连接代理”，然后单击“添加 RD 连接代理服务器”。 
   3. 完成向导的每个页面，直到进入“服务器选择”，然后选择新建的 RD 连接代理服务器（例如 Contoso-CB2）。
   4. 完成向导并接受默认值。
4. 在 RD 连接代理服务器和客户端上配置受信任的证书。


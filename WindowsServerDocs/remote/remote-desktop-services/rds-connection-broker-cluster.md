---
title: 添加 RD 连接代理服务器在 RDS 中配置高可用性
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
ms.openlocfilehash: e20b4960faac0ef40ad68271fa907394344e9c47
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034429"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>添加 RD 连接代理服务器以部署和配置高可用性

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

可以部署远程桌面连接代理 （RD 连接代理） 群集来提高可用性和可扩展性的远程桌面服务基础结构。 

## <a name="pre-requisites"></a>先决条件

设置服务器以充当第二个 RD 连接代理-这可以是物理服务器或 VM。

设置连接代理的数据库。 可以使用[Azure SQL 数据库](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database)实例或在本地环境中的 SQL Server。 我们讨论有关使用 Azure SQL，但步骤仍适用于 SQL Server。 您需要找到数据库的连接字符串，同时确保具有正确的 ODBC 驱动程序。

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>第 1 步：数据库配置连接代理

1. 找到你创建的数据库的连接字符串-需要它这两个版本的 ODBC 驱动程序需要和更高版本时要配置连接代理本身 （步骤 3），, 因此将字符串保存的位置，您可以引用它轻松地识别。 下面是如何为 Azure SQL 中查找连接字符串：  
    1. 在 Azure 门户中，单击**浏览 > 资源组**单击部署的资源组。   
    2. 选择刚刚创建 （例如 CB DB1） 的 SQL 数据库。   
    3. 单击**设置 > 属性 > 显示数据库连接字符串**。   
    4. 复制的连接字符串**ODBC （包括 Node.js）** ，这应如下所示：   
      
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;   
  
    5. "Your_password_here"替换为实际密码。 连接到数据库时，将使用包含密码，使用此整个字符串。 
2. 在新的连接代理上安装的 ODBC 驱动程序： 
   1. 如果 VM 将用于连接代理，则为第一个 RD 连接代理创建一个公共 IP 地址。 （您仅需要执行此操作，如果 RDMS 虚拟机尚不包含公共 IP 地址，以便建立 RDP 连接。）
       1. 在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击第一个 RD 连接代理虚拟机 (例如，Contoso Cb1)。
       2. 单击**设置 > 网络接口**，然后单击相应的网络接口。
       3. 单击**设置 > IP 地址**。
       4. 有关**公共 IP 地址**，选择**已启用**，然后单击**IP 地址**。
       5. 如果你有想要使用现有公共 IP 地址，它从列表中选择。 否则，请单击**新建**，输入一个名称，然后单击**确定**，然后**保存**。
   2. 连接到第一个 RD 连接代理：
       1. 在 Azure 门户中，单击**浏览 > 资源组**，单击部署的资源组，然后单击第一个 RD 连接代理虚拟机 (例如，Contoso Cb1)。
       2. 单击**Connect > 打开**打开远程桌面客户端。
       3. 在客户端，单击**Connect**，然后单击**使用的其他用户帐户**。 输入域管理员帐户的用户名和密码。
       4. 单击**是**时收到有关证书的警告。
   3. 下载[适用于 SQL Server 的 ODBC 驱动程序](https://www.microsoft.com/download/confirmation.aspx?id=50420)与 ODBC 连接字符串中的版本相匹配。 对于上面的示例字符串中，我们需要安装版本 13 ODBC 驱动程序。
   4. Sqlincli.msi 文件复制到第一个 RD 连接代理服务器。   
   5. 打开 sqlincli.msi 文件并安装本机客户端。  
   6. 对每个其他的 RD 连接代理 (例如，Contoso Cb2) 重复步骤 1-5。
   7. 在将运行连接代理的每个服务器上安装的 ODBC 驱动程序。

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>步骤 2：RD 连接代理上配置负载均衡 

如果使用的 Azure 基础结构，则可以创建[Azure 负载均衡器](#create-a-load-balancer); 如果没有，则可以设置向上[DNS 轮循机制](#configure-dns-round-robin)。

### <a name="create-a-load-balancer"></a>创建负载均衡器  
1. 创建 Azure 负载均衡器   
      1. 在 Azure 门户中，单击**浏览 > 负载均衡器 > 添加**。   
      2. 输入新负载均衡器 (例如，hacb) 的名称。   
      3. 选择**内部**有关**方案**，**虚拟网络**为你的部署 (例如，Contoso-VNet)，以及**子网**所有您的资源 （例如，默认值）。   
      4. 选择**静态**有关**IP 地址分配**，然后输入**专用 IP 地址**，它是不在当前正在使用 (例如，10.0.0.32)。   
      5. 选择适当**订阅**，则**资源组**您的资源，然后使用适当的所有**位置**。   
      6. 选择“创建”  。   
2. 创建[探测](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/)到哪些服务器处于活动状态的监视器：   
      1. 在 Azure 门户中，单击**浏览 > 负载平衡器**，然后单击负载均衡器只需创建 (例如，CBLB)。 单击“设置”  。   
      2. 单击**探测 > 添加**。   
      3. 输入探测的名称 (例如， **RDP**)，选择**TCP**作为**协议**，输入**3389**为**端口**，然后单击**确定**。   
3. 创建连接代理的后端池：   
      1. 在中**设置**，单击**后端地址池 > 添加**。   
      2. 输入名称 (例如，CBBackendPool)，然后单击**将虚拟机添加**。  
      3. 选择可用性集 (例如，CbAvSet)，然后依次**确定**。   
      3. 单击**选择虚拟机**，选择每个虚拟机，，然后单击**选择 > 确定 > 确定**。   
4. 创建 RDP 负载均衡规则：   
      1. 在中**设置**，单击**负载均衡规则**，然后单击**添加**。   
      2. 输入名称 (例如，RDP)，选择**TCP**有关**协议**，输入**3389**同时**端口**和**后端端口**，然后单击**确定**。   
5. 添加负载均衡器的 DNS 记录：   
      1. 连接到 RDMS server 虚拟机 (例如，Contoso CB1)。 请查看[准备 RD 连接代理 VM](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md)一文，了解如何连接到 VM 的步骤。   
      2. 在服务器管理器中，单击**工具 > DNS**。   
      3. 在左侧窗格中，展开**DNS**，单击 DNS 计算机，单击**正向查找区域**，然后单击你的域名 (例如，Contoso.com)。 （它可能需要几秒钟来处理对 DNS 服务器的信息的查询）。  
      4. 单击**操作 > 新建主机 （A 或 AAAA）** 。   
      9. 输入名称 (例如，hacb) 和前面指定的 IP 地址 (例如，10.0.0.32)。   

### <a name="configure-dns-round-robin"></a>配置 DNS 轮循机制  
  
以下步骤，创建 Azure 内部负载均衡器的替代方法。   
  
1. 连接到 Azure 门户中的 RDMS 服务器。 使用远程桌面连接客户端   
2. 创建 DNS 记录：   
      1. 在服务器管理器中，单击**工具 > DNS**。   
      2. 在左侧窗格中，展开**DNS**，单击 DNS 计算机，单击**正向查找区域**，然后单击你的域名 (例如，Contoso.com)。 （它可能需要几秒钟来处理对 DNS 服务器的信息的查询）。  
      3. 单击**操作**并**新主机 （A 或 AAAA）** 。   
      4. 输入**DNS 名称**对于 RD 连接代理群集 （例如 hacb），并输入**IP 地址**的第一个 RD 连接代理。   
      5. 对每个其他的 RD 连接代理，为每个其他记录提供每个唯一的 IP 地址重复步骤 3-4。


例如，如果两个 RD 连接代理虚拟机的 IP 地址 10.0.0.8 和 10.0.0.9，您将创建两个 DNS 主机记录：
 - 主机名： hacb.contoso.com、 IP 地址：10.0.0.8
 - 主机名： hacb.contoso.com、 IP 地址：10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>步骤 3:配置高可用性连接代理

1. 添加新的 RD 连接代理服务器到服务器管理器：
   1. 在服务器管理器中，单击**管理 > 添加服务器**。
   2. 单击**立即查找**。
   3. 单击新创建的 RD 连接代理服务器 (例如，Contoso Cb2)，然后单击**确定**。
2. 配置 RD 连接代理高可用性：
   1. 在服务器管理器中，单击**远程桌面服务 > 概述**。
   2. 右键单击**RD 连接代理**，然后单击**配置高可用性**。
   3. 通过该向导，直到转到配置类型部分的页。 选择**共享数据库服务器**，然后单击**下一步**。
   4. 输入 RD 连接代理群集的 DNS 名称。
   5. SQL DB 中，输入连接字符串，然后完成用于建立高可用性的向导页上。
3. 向部署添加新的 RD 连接代理
   1. 在服务器管理器中，单击**远程桌面服务 > 概述**。
   2. 右键单击 RD 连接代理，然后单击**添加 RD 连接代理服务器**。
   3. 通过向导，直到到达服务器选择，然后选择新创建的 RD 连接代理服务器 (例如，Contoso CB2) 页。
   4. 完成向导，接受默认值。
4. RD 连接代理服务器和客户端上配置受信任的证书。


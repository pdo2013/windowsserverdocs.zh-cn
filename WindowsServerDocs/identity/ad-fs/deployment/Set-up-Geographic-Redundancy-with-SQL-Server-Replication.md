---
title: "设置地理冗余的 SQL Server 复制"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>设置地理冗余的 SQL Server 复制

>适用于：Windows Server 2016，Windows Server 2012 R2

> [!IMPORTANT]  
> 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，您可以使用 SQL Server 2008 或更高版本。
  
如果使用的与你的广告 FS 配置数据库的 SQL Server，你可以使用 SQL Server 复制广告 FS 场设置 geo\ 冗余。 Geo\ 冗余复制两个遥远站点之间数据，以便应用从一个网站可以切换到另一种。 这样一来，出现故障的一个网站，你仍可以提供的所有配置数据第二个地点。 有关详细信息，在中看到"SQL Server 地理冗余部分"[联盟服务器电场的日落使用 SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md)。  
  
## <a name="prerequisites"></a>先决条件  
安装和配置 SQL server 场。 有关详细信息，请参阅[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 在初始 SQL Server，确保 SQL Server 代理服务正在运行，并设置为自动启动。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>创建用于 geo\ 冗余第二个 \(replica\) SQL Server  
  
1.  安装 SQL Server \ (有关详细信息，请参阅[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 复制到副本 SQL server 生成的 CreateDB.sql 和 SetPermissions.sql 脚本文件。  
  
2.  确保 SQL Server 代理服务正在运行，并将设置为自动启动  
  
3.  运行**Export\ AdfsDeploymentSQLScript**主广告 FS 节点以创建 CreateDB.sql 和 SetPermissions.sql 文件上。  例如：`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  将这些脚本复制到第二服务器。  打开中的 CreateDB.sql 脚本**SQL 管理 Studio**单击**执行**。
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. 打开的中 SetPermissions.sql 脚本**SQL 管理 Studio**单击**执行**。
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>你还可以使用以下命令行中。 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>在初始 SQL Server 上创建发布设置  
  
1.  SQL Server 管理 studio 中，从下**复制**，右键单击**本地发布**选择**新发布 …**
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  在新发布的向导屏幕上，单击**下一步**。</br>
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  在**分销商**页上，选择与分销商本地服务器，请单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  在**快照**文件夹页中，输入 \\\SQL1\repldata 来替代默认文件夹。 \ (请注意： 你可能需要创建此共享 yourself\)。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  选择**AdfsConfigurationV3**作为发布数据库单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  在**发布类型**，选择**合并发布**单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  在**订户类型**，选择**SQL Server 2008 或更高版本**单击**下一步**。  
 ![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  在**文章**页面上选择**表格**节点以选择所有表格，然后依次都选择**un\ 检查 SyncProperties**表 \ （这一不应 replicated\）</br>
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  在**文章**页上，选择**用户定义的函数**节点以选择所有用户定义的函数，然后单击**下一步**..  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. 在**文章问题**页面单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. 在**筛选器表格行**页上，单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. 在**快照代理**页上，选择默认值的即时和 14 天后，单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
你可能需要为 SQL 代理创建域帐户。 使用中的步骤[域帐户 CONTOSO\\sqlagent 配置 SQL 登录](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)创建 SQL 此新广告用户登录并分配特定的权限。  
  
13. 在**代理安全**页上，单击**安全设置**输入域帐户 username\/密码 \ (不 GMSA\) 创建 SQL 代理和单击**确定**。  单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. 在**向导操作**页上，单击**下一步**。   
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. 在**完成向导**页上，输入你发布的名称，请单击**完成**。 
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. 创建发布后，你应该看到成功状态。  单击**关闭**。
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. 重新在 SQL Server 管理 Studio 中，右键单击新的发布，然后单击**启动复制监视器**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>副本 SQL Server 上创建的订阅设置  
请确保你创建的发布者设置与初始的 SQL Server 上如上所述，然后完成以下过程：  
  
1.  上的副本 SQL Server、SQL Server 管理 studio 中，从下**复制**，右键单击**本地订阅**，然后选择**新订阅…**.![地理冗余设置](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  在**新订阅向导**页上，单击**下一步**。
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  在**发布**页上，从下拉列表中选择发布者。  展开**AdfsConfigurationV3**和选择发布创建上方的名称，然后单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  在**合并代理位置**页上，选择**在其订户 \(pull subscriptions\) 运行每个代理**\(the default\) 单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> 此操作，请在下面，订阅键入以及确定冲突分辨率逻辑。 \ (有关详细信息，请参阅[检测并解决合并复制冲突](https://technet.microsoft.com/library/ms151191.aspx)。 </br>
 
5.  在**订户**页上，选择**AdfsConfigurationV3**作为订户数据库单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  在**合并代理安全**页上，单击**…** ，输入用户名和密码的是域帐户 \ (不 GMSA\) 为 SQL 代理创建使用省略号框并单击**下一步**。
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  在**同步计划**，选择**运行持续**单击**下一步**。 
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  在**初始化订阅**，单击**下一步**。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  在**订阅类型**，选择**客户端**单击**下一步**。  
  
    记录含义[此处](https://technet.microsoft.com/library/ms151191.aspx)和[此处](https://technet.microsoft.com/library/ms151170.aspx)。  本质上是，我们需要简单"发布 wins 到第一个"冲突分辨率，并且我们不需要重新发布到其他订户。  
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. 在**向导操作**页上，确保**创建该订阅**进行检查并单击**下一步**。 
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. 在**完成向导**页上，单击**完成**。 
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. 完成该订阅已创建过程，你应该看到成功。 单击**关闭**。 
![设置地理冗余](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>验证过程初始化和复制  
  
1.  主要的 SQL server、 上 right\ 单击**复制**节点，然后单击**启动复制监视器**。  
  
2.  在**复制监视器**，单击发布。  
  
3.  在**所有订阅**选项卡上，右键单击和**查看详细信息**。  
  
    你应该能够看到下的很多项**操作**初始复制。  
  
4.  此外，你可以查看下**SQL Server Agent\\Jobs**节点以查看 job\(s\) 安排执行 publication\ 订阅的操作。  仅显示本地作业，因此请确保疑难解答检查发布者和订阅。  Right\ 单击一个作业和选择**查看历史记录**查看的执行历史记录和结果。  
  
## <a name="sqlagent"></a>配置域帐户 CONTOSO\\sqlagent SQL 登录  
  
1.  创建新的登录，主要和副本称为 CONTOSO\\sqlagent SQL Server \ (创建新的域用户的名称，并上配置**代理安全**上方的过程中的页面。 \)  
  
2.  SQL Server、 right\ 单击登录你创建的并且在下，依次选择属性中**用户映射**选项卡上，映射到此登录**AdfsConfiguration**和**AdfsArtifact**具有公众和 db\_genevaservice 角色数据库。 此外将映射到分发数据库此登录，并添加分发和 adfsconfiguration 表格 db\_owner 角色。  （如果适用），主要和副本 SQL server 上执行此操作。 有关详细信息，请参阅[复制代理安全模型](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  提供相应域帐户读取和写入上配置为分销商的共享权限。  请确保你设置读取和写入同时上的共享权限和的本地文件权限的权限。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>配置广告 FS node\(s\) 指向 SQL Server 副本电场的日落  
现在，你已设置了地理冗余，可配置广告 FS 电场的日落节点以指向你副本 SQL Server 场使用标准广告 FS"加入"电场的日落功能，可以从广告 FS 配置向导 UI 或使用 Windows PowerShell。  
  
如果你使用的广告 FS 配置向导 UI，请选择**添加到联合身份验证的服务器场联合服务器**。 **未起效的**选择**联合身份验证的服务器场中创建的第一个联盟服务器**。  
  
如果你使用 Windows PowerShell，运行**Add\ AdfsFarmNode**。 **未起效的**运行**Install\ AdfsFarm**。  
  
看到提示后，当提供副本 SQL Server 主机和实例名称**不**初始 SQL server。  

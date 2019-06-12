---
title: 使用 SQL Server 复制设置地理冗余
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 00880d06835fad08538f634fdf2868146fc23b1a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442926"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>使用 SQL Server 复制设置地理冗余


> [!IMPORTANT]  
> 如果你想要创建的 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 或更高版本。
  
如果使用的作为你的 AD FS 配置数据库的 SQL Server，则可以设置异地\-冗余，以便使用 SQL Server 复制 AD FS 场。 异地\-冗余在以便应用程序可以从一个站点切换到另一个地理位置相距遥远的两个站点之间复制数据。 这样一来，如果出现一个站点的故障，你仍可以让所有可用的配置数据的第二个站点上。 详细信息，请参阅"SQL Server 的地理冗余部分"中[联合服务器场使用 SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md)。  
  
## <a name="prerequisites"></a>先决条件  
安装和配置 SQL server 场。 有关详细信息，请参阅[ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 在初始的 SQL Server，请确保正在运行的 SQL Server 代理服务并设置为自动启动。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>创建第二个\(副本\)异地的 SQL Server\-冗余  
  
1. 安装 SQL Server\(的详细信息，请参阅[ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 将生成的 CreateDB.sql 和 SetPermissions.sql 脚本文件复制到副本的 SQL server。  
  
2. 请确保 SQL Server 代理服务正在运行，并将设置为自动启动  
  
3. 运行**导出\-AdfsDeploymentSQLScript**创建 CreateDB.sql 和 SetPermissions.sql 文件在主 AD FS 节点上。  例如：`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. 将脚本复制到辅助服务器。  打开中的 CreateDB.sql 脚本**SQL Management Studio**然后单击**Execute**。
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. 打开中的 SetPermissions.sql 脚本**SQL Management Studio**然后单击**Execute**。
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> 此外可以使用以下命令行中。 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>初始的 SQL Server 上创建发布服务器设置  
  
1. 从 SQL Server Management studio 下**复制**，右键单击**本地发布**，然后选择**新发布...** 
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. 在新建发布向导屏幕上单击**下一步**。</br>
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. 上**分发服务器上**页上，选择本地服务器作为分发服务器上，单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. 上**快照**文件夹页面上，输入\\\SQL1\repldata 代替默认文件夹。 \(注意：可能需要自行创建此共享\)。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. 选择**AdfsConfigurationV3**作为发布数据库，然后单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. 上**发布类型**，选择**合并发布**然后单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. 上**订阅服务器类型**，选择**SQL Server 2008 或更高版本**然后单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. 上**文章**页上，选择**表**节点以选择所有表，然后**取消\-检查 SyncProperties**表\(这个不应为复制\)</br>
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. 上**文章**页上，选择**用户定义函数**节点以选择所有用户定义函数，然后单击**下一步**...  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. 上**项目问题**页上，单击**下一步**。  
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. 上**筛选表行**页上，单击**下一步**。  
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. 上**快照代理**页上，选择即时和 14 天的默认值，单击**下一步**。  
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    您可能需要为 SQL 代理创建的域帐户。 使用中的步骤[CONTOSO 的域帐户配置 SQL 登录名\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)若要创建此新的 AD 用户的 SQL 登录名并分配特定权限。  
  
13. 上**代理安全性**页上，单击**安全设置**并输入用户名\/域帐户的密码\(不 GMSA\)创建 SQL 代理和单击**确定**。  单击“下一步”  。  
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. 上**向导操作**页上，单击**下一步**。   
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. 上**完成该向导**页上，输入你发布的名称，单击**完成**。 
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. 创建发布后，应看到成功的状态。  单击 **“关闭”** 。
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. 返回在 SQL Server Management Studio 中，右键单击新发布，然后单击**启动复制监视器**。  
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>SQL Server 副本上创建订阅设置  
请确保您创建的发布服务器设置初始的 SQL Server 上按上文所述，然后完成以下过程：  
  
1. 从 SQL Server Management studio 的 SQL Server 副本上下**复制**，右键单击**本地订阅**，然后选择**新订阅...** .![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. 上**新建订阅向导**页上，单击**下一步**。
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. 上**发布**页上，从下拉列表中选择发布服务器。  展开**AdfsConfigurationV3**并选择上面创建的发布的名称，然后单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. 上**合并代理位置**页上，选择**在其订阅服务器上运行每个代理\(请求订阅\)** \(默认\)单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> 此操作，请订阅类型，以及确定冲突解决逻辑。 \(有关详细信息，请参阅[检测和解决合并复制冲突](https://technet.microsoft.com/library/ms151191.aspx)。 </br>
 
5. 上**订户**页上，选择**AdfsConfigurationV3**作为订阅服务器数据库，然后单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. 上**合并代理安全性**页上，单击 **...** 并输入用户名和域帐户的密码\(不 GMSA\)使用省略号框，然后单击创建的 SQL 代理**下一步**。
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. 上**同步计划**，选择**连续运行**然后单击**下一步**。 
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. 上**初始化订阅**，单击**下一步**。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. 上**订阅类型**，选择**客户端**然后单击**下一步**。  
  
   记录此含义[这里](https://technet.microsoft.com/library/ms151191.aspx)并[此处](https://technet.microsoft.com/library/ms151170.aspx)。  从根本上讲，我们需要简单的"第一个到发布服务器入选"冲突解决和我们无需重新发布到其他订阅服务器。  
   ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. 上**向导操作**页上，确保**创建订阅**检查，并单击**下一步**。 
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. 上**完成该向导**页上，单击**完成**。 
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. 在创建过程完成订阅后，应会看到成功。 单击 **“关闭”** 。 
    ![设置地理冗余](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>验证初始化和复制的过程  
  
1.  在主 SQL 服务器上，右键\-单击**复制**节点，然后单击**启动复制监视器**。  
  
2.  在中**复制监视器**，单击发布。  
  
3.  上**的所有订阅**选项卡上，右键单击并**查看详细信息**。  
  
    您应该能够看到下的许多条目**操作**用于初始复制。  
  
4.  此外，你可以查看下**SQL Server 代理\\作业**节点以查看作业\(s\)计划执行的操作的发布的\/订阅。  仅显示本地作业，因此请务必检查发布服务器和订阅服务器上进行故障排除。  右\-单击一个作业，然后选择**查看历史记录**查看执行历史记录和结果。  
  
## <a name="sqlagent"></a>配置 CONTOSO 的域帐户的 SQL 登录名\\sqlagent  
  
1.  在主和副本名为 CONTOSO 的 SQL Server 上创建新的登录名\\sqlagent\(创建新的域用户的名称，并在配置**代理安全性**上述过程中的页。\)  
  
2.  在 SQL Server 中，右键\-单击你创建的登录名，然后选择属性，然后在**用户映射**选项卡上，将映射到此登录名**AdfsConfiguration**和**AdfsArtifact**通过公共和 db 数据库\_genevaservice 角色。 此外将此登录名映射到分发数据库并添加 db\_分发和 adfsconfiguration 表的所有者角色。  根据主和副本的 SQL server 上执行此操作。 有关详细信息，请参阅[Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  授予相应的域帐户读取和写入共享配置为分发服务器上的权限。  请确保您设置读取和写入权限的共享权限和本地文件权限。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>配置 AD FS 节点\(s\)为指向 SQL Server 副本场  
现在，已设置了异地冗余，可以配置 AD FS 场节点为点到副本的 SQL Server 场使用的标准 AD FS"联接"场功能，从 AD FS 配置向导用户界面或使用 Windows PowerShell。  
  
如果使用 AD FS 配置向导用户界面，选择**将联合身份验证服务器添加到联合服务器场**。 **不这样做**选择**在联合服务器场中创建第一个联合身份验证服务器**。  
  
如果使用 Windows PowerShell，运行**外\-AdfsFarmNode**。 **不这样做**运行**安装\-AdfsFarm**。  
  
出现提示时，提供副本 SQL Server 的主机和实例名称**不**初始的 SQL server。  

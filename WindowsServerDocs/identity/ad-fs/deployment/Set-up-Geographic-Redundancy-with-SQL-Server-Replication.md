---
title: 设置 SQL Server 复制的地理冗余
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 16cf1a237043aa546d4fc24164045aa9f9a1e6ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359821"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>设置 SQL Server 复制的地理冗余


> [!IMPORTANT]  
> 如果要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 或更高版本。
  
如果使用 SQL Server 作为 AD FS 配置数据库，则可以使用 SQL Server 复制为 AD FS 场设置异地 @ no__t-0redundancy。 Geo @ no__t-0redundancy 在两个地理位置遥远的站点之间复制数据，以便应用程序可以从一个站点切换到另一个站点。 这样一来，如果一个站点发生故障，你仍可以在第二个站点上提供所有配置数据。 有关详细信息，请参阅[使用 SQL Server 的联合服务器场](../design/Federation-Server-Farm-Using-SQL-Server.md)中的 "SQL Server 地理冗余部分"。  
  
## <a name="prerequisites"></a>先决条件  
安装和配置 SQL server 场。 有关详细信息, 请[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)参阅。 在初始 SQL Server 上，确保 SQL Server 代理服务正在运行，并设置为自动启动。  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>为 geo @ no__t 创建第二个 \(replica @ no__t-1 SQL Server  
  
1. 安装 SQL Server @no__t 详细信息，请参阅[https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)。 将生成的 CreateDB 和 SetPermissions 脚本文件复制到副本 SQL server。  
  
2. 确保 SQL Server 代理服务正在运行并且设置为自动启动  
  
3. 在主 AD FS 节点上运行**Export @ no__t-1AdfsDeploymentSQLScript** ，以创建 CreateDB 和 SetPermissions 文件。  例如： `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`。  
   ![Set 地域冗余 @ no__t-1
  
4. 将脚本复制到辅助服务器。  在**sql Management Studio**中打开 CreateDB 脚本，然后单击 "**执行**"。
   ![Set 地域冗余 @ no__t-1

5. 在**sql Management Studio**中打开 SetPermissions 脚本，然后单击 "**执行**"。
   ![Set 地域冗余 @ no__t-1 

   

> [!NOTE]
> 还可以从命令行使用以下命令。 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>在初始 SQL Server 上创建发布服务器设置  
  
1. 在 SQL Server Management studio 中的 "**复制**" 下，右键单击 "**本地发布**"，然后选择 "**新建发布 ...** "
    @ no__t-4Set up 地域冗余 @ no__t-5 </br>  

2. 在新建发布向导屏幕上，单击 "**下一步**"。</br>
   ![Set 地域冗余 @ no__t-1 </br> 
  
3. 在 "**分发服务器**" 页上，选择本地服务器作为分发服务器，然后单击**下一步**。  
   ![Set 地域冗余 @ no__t-1 </br>   

4. 在 "**快照**文件夹" 页上，输入 \\ \ SQL1\repldata 代替默认文件夹。 \(注意：你可能需要自行创建此共享，@ no__t。  
   ![Set 地域冗余 @ no__t-1 </br>   
  
5. 选择**AdfsConfigurationV3**作为发布数据库，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br>
  
6. 在 "**发布类型**" 上，选择 "**合并发布**" 并单击 "**下一步**"  
   ![Set 地域冗余 @ no__t-1 </br>
  
7. 在**订阅服务器**上，选择**SQL Server 2008 或更高版本**，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br> 

8. 在 "**项目**" 页上，选择 "**表**" 节点以选择所有表，然后**取消 @ no__t-3check SyncProperties**表 \(this 不应复制 @ no__t-5</br>
   ![Set 地域冗余 @ no__t-1 </br>    
  
9. 在 "**项目**" 页上，选择 "**用户定义函数**" 节点以选择所有用户定义的函数，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br>    

10. 在 "**问题**" 页上，单击 "**下一步**"。  
    ![Set 地域冗余 @ no__t-1 </br>   

11. 在 "**筛选表行**" 页上，单击 "**下一步**"。  
    ![Set 地域冗余 @ no__t-1 </br>   
12. 在 "**快照代理**" 页上，选择 "即时" 和 "14 天"，然后单击 "**下一步**"。  
    ![Set 地域冗余 @ no__t-1 </br>   
    可能需要为 SQL 代理创建域帐户。 使用为[域帐户 CONTOSO @ no__t-1Sqlagent 配置 sql 登录名](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent)中的步骤为此新 AD 用户创建 sql 登录名并分配特定权限。  
  
13. 在 "**代理安全性**" 页上，单击 "**安全设置**"，然后输入域帐户的用户名 @ no__t-2password \(not 为 SQL 代理创建的 GMSA @ no__t-4，并单击 **"确定"** 。  单击“下一步”。  
    ![Set 地域冗余 @ no__t-1 </br>  

14. 在 "**向导操作**" 页上，单击 "**下一步**"。   
    ![Set 地域冗余 @ no__t-1 </br>

15. 在 "**完成向导**" 页上，输入发布的名称，然后单击 "**完成**"。 
    ![Set 地域冗余 @ no__t-1 </br>  

16. 创建发布后，应会看到 "成功" 状态。  单击 **“关闭”** 。
    ![Set 地域冗余 @ no__t-1 </br>  

17. 返回 SQL Server Management Studio，右键单击新发布，然后单击 "**启动复制监视器**"。  
    ![Set 地域冗余 @ no__t-1 </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>在副本 SQL Server 上创建订阅设置  
请确保已按照上述步骤在初始 SQL Server 上创建发布服务器设置，然后完成以下过程：  
  
1. 在副本 SQL Server SQL Server 上的 "**复制**" 下，右键单击 "**本地订阅**"，然后选择 "**新建订阅 ...** "。![Set 地域冗余 @ no__t-1 </br>  

2. 在 "**新建订阅向导**" 页上，单击 "**下一步**"。
   ![Set 地域冗余 @ no__t-1 </br>   
  
3. 在 "**发布**" 页上，从下拉端中选择发布服务器。  展开 " **AdfsConfigurationV3** "，选择上面创建的发布的名称，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br> 
  
4. 在 "**合并代理位置**" 页上，选择 "**在其订阅服务器上运行每个代理 \(pull 订阅 @ no__t-3** \(the 默认 @ no__t-5"，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br> 这与下面的订阅类型一起确定冲突解决逻辑。 \(For 详细信息，请参阅[检测并解决合并复制冲突](https://technet.microsoft.com/library/ms151191.aspx)。 </br>
 
5. 在 "**订阅服务器**" 页上，选择**AdfsConfigurationV3**作为订阅服务器数据库，然后单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br> 
  
6. 在 "**合并代理安全**" 页上，单击 " **...** " 并输入域帐户的用户名和密码 @no__t 为 SQL 代理创建的 GMSA @ no__t，并单击 "**下一步**"。
   ![Set 地域冗余 @ no__t-1 </br> 
  
7. 在**同步计划**中，选择 "**连续运行**"，然后单击 "**下一步**"。 
   ![Set 地域冗余 @ no__t-1 </br> 
 
8. **初始化订阅**时，单击 "**下一步**"。  
   ![Set 地域冗余 @ no__t-1 </br> 
  
9. 在 "**订阅类型**" 上，选择**客户端**，然后单击 "**下一步**  
  
   [这里](https://technet.microsoft.com/library/ms151191.aspx)介绍了这种情况的[含义。](https://technet.microsoft.com/library/ms151170.aspx)  实质上，我们采用简单的 "首先发布服务器入选" 冲突解决方法，而不需要将其重新发布到其他订阅服务器。  
   ![Set 地域冗余 @ no__t-1 </br>
   
10. 在 "**向导操作**" 页上，确保选中 **"创建订阅"** ，然后单击 "**下一步**"。 
    ![Set 地域冗余 @ no__t-1 </br>

11. 在 "**完成向导**" 页上，单击 "**完成**"。 
    ![Set 地域冗余 @ no__t-1 </br>

12. 订阅完成创建过程后，应会看到 "成功"。 单击 **“关闭”** 。 
    ![Set 地域冗余 @ no__t-1 </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>验证初始化和复制的过程  
  
1.  在主 SQL server 上，右键 @ no__t-0click "**复制**" 节点，然后单击 "**启动复制监视器**"。  
  
2.  在**复制监视器**中，单击发布。  
  
3.  在 "**所有订阅**" 选项卡上，右键单击并**查看详细信息**。  
  
    在初始复制的 "**操作**" 下，应该可以看到许多条目。  
  
4.  此外，还可以在 " **SQL Server 代理 @ no__t-1Jobs** " 节点下查看作业 @ no__t-2 @ no__t，计划执行发布 @ no__t-4subscription 的操作。  仅显示本地作业，因此请确保在发布服务器和订阅服务器上进行故障排除。  右 @ no__t-0click a 作业，然后选择 "**查看历史记录**" 以查看执行历史记录和结果。  
  
## <a name="sqlagent"></a>为域帐户配置 SQL 登录 CONTOSO @ no__t-1sqlagent  
  
1.  在上述过程中的 "**代理安全性**" 页上，在名为 "CONTOSO @ no__t-0sqlagent \(the" 的新域用户的主副本和副本 SQL Server 上创建新的登录名。 \)  
  
2.  在 SQL Server 中，右键 @ no__t-0click 你创建的登录名，然后选择 "属性"，然后在 "**用户映射**" 选项卡下，将此登录名映射到**AdfsConfiguration** ，并将**AdfsArtifact**数据库映射为 public 和 db @ no__t-4genevaservice 角色。 还需要将此登录名映射到分发数据库，并为分发和 adfsconfiguration 表添加 db @ no__t-0owner 角色。  在主 SQL server 和副本 SQL server 上执行此操作。 有关详细信息，请参阅[复制代理安全模式](https://technet.microsoft.com/library/ms151868.aspx)。  
  
3.  向相应的域帐户授予对配置为分发服务器的共享的读取和写入权限。  请确保在 "共享" 权限和 "本地文件" 权限上都设置 "读写" 权限。  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>将 AD FS node @ no__t-0 @ no__t 设置为指向 SQL Server 副本场  
设置异地冗余后，可以使用标准 AD FS "加入" 场功能，将 AD FS 场节点配置为指向副本 SQL Server 场，无论是通过 AD FS 配置向导 UI 还是使用 Windows PowerShell。  
  
如果使用 AD FS 配置向导用户界面，则选择 "**将联合服务器添加到联合服务器场**"。 **不要**选择 **"在联合服务器场中创建第一个联合服务器"** 。  
  
如果使用 Windows PowerShell，请运行**Add @ no__t-1AdfsFarmNode**。 **不要**运行**Install @ no__t-2AdfsFarm**。  
  
出现提示时，提供副本 SQL Server 的主机名和实例名，而**不**是初始 SQL Server。  

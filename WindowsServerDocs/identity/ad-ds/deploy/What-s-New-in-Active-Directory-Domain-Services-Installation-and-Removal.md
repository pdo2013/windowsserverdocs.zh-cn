---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: "什么测验 Active Directory 域中的新增功能 s 服务安装和删除"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>什么测验 Active Directory 域中的新增功能 s 服务安装和删除

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍：  
  
-   [Active Directory 域服务配置向导](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Adprep.exe 集成](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [广告 DS 安装先决条件验证](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [系统要求](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [已知的问题](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
在 Windows Server 2012 的 active Directory 域服务 (广告 DS) 部署更简单且比以前版本的 Windows Server 更快。 广告 DS 安装过程现在内置于 Windows PowerShell 和是已集成使用服务器管理器。 减少现有的 Active Directory 环境中引入域控制器所需的步数。 这使得的过程更简单且更高效创建新的 Active Directory 环境。 新的广告 DS 部署过程最小化会另行已阻止安装的错误的机会。  
  
此外，你可以安装广告 DS server 角色二进制文件 （即广告 DS 服务器角色） 在多个服务器上一次。 你还可以单独的服务器上远程运行广告 DS 安装向导。 这些改进提供更大的灵活性用于部署运行 Windows Server 2012，特别是对于大规模、 全球部署许多域控制器需要将其部署到办公室于不同地区的域控制器。  
  
广告 DS 安装包含以下功能：  
  
-   **Adprep.exe 集成到广告 DS 安装过程。** 所需准备现有的 Active Directory，如需使用不同的凭据各种、 复制 Adprep.exe 文件或登录到特定的域控制器上的麻烦步骤所有简化或自动进行。 这减少了安装广告 DS 所需的时间，并减少否则可能会阻止域控制器升级的错误的机会。  
  
    对于是最好运行 adprep.exe 命令提前新的域控制器安装环境，仍可以执行 adprep.exe 命令分别从广告 DS 安装。 Adprep.exe 的 Windows Server 2012 版本远程，运行，因此你可以从运行 64 位版本的 Windows Server 2008 或更高版本的服务器执行所有必要的命令。  
  
-   **新的广告 DS 安装已内置于 Windows PowerShell 和可以远程调用。** 新的广告 DS 安装是已集成使用服务器管理器中，以便你可以使用相同的界面安装安装其他服务器角色时，使用你的广告 DS。 为 Windows PowerShell 用户广告 DS 部署 cmdlet 提供更好的功能和的灵活性。 还有功能奇偶校验之间命令行和 GUI 安装选项。  
  
-   **新的广告 DS 安装包括先决条件验证。** 在开始安装之前标识任何潜在的错误。 发生没有问题导致从部分完成升级之前，您可以更正错误情况。 例如，如果需要运行 adprep /domainprep，验证安装向导用户具有足够执行该操作的权限。  
  
-   **配置页面镜像的相关选项更少向导页中，分组最常见的升级选项要求顺序分组。** 这使安装选择提供更好的上下文。  
  
-   **你可以将导出一个包含所有图形安装过程中指定的选项的 Windows PowerShell 脚本。** 在安装或删除结束时，你可以将设置导出到自动执行相同操作搭配使用的 Windows PowerShell 脚本。  
  
-   **仅关键复制早重新启动。** 允许不重要的数据，重新启动前复制的新切换。 有关详细信息，请参阅[ADDSDeployment cmdlet 参数](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)。  
  
### <a name="BKMK_ADConfigurationWizard"></a>Active Directory 域服务配置向导  
开始与 Windows Server 2012、 Active Directory 域服务配置向导替换作为用户界面 (UI) 选项，当你安装了域控制器指定设置传统 Active Directory 域服务安装向导。 添加角色向导完毕后，将开始 Active Directory 域服务配置向导。  
  
> [!WARNING]  
> 旧版 Active Directory 域服务安装向导 (dcpromo.exe) 不再支持与 Windows Server 2012 开始。  
  
在[安装 Active Directory 域服务 & #40;级别 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)的 UI 的过程显示角色二进制文件的方法以开始添加角色向导安装广告 DS 服务器，，然后运行 Active Directory 域服务配置向导完成域控制器安装。 Windows PowerShell 示例显示了如何完成使用广告 DS 部署 cmdlet 这两个步骤。  
  
### <a name="BKMK_NewAdprep"></a>Adprep.exe 集成  
开始与 Windows Server 2012，没有 Adprep.exe 只有一个版本 (没有 32 位版本，adprep32.exe)。 根据需要安装到现有 Active Directory 域或森林运行 Windows Server 2012 域控制器时自动运行 Adprep 命令。  
  
尽管 adprep 操作将自动运行，但可以分别运行 Adprep.exe。 例如，如果安装广告 DS 的用户不属于企业管理员组中，以便运行 Adprep /forestprep 成功要求，你可能需要单独运行该命令。 但是，你仅有运行 adprep.exe，如果你打算就地升级你的第一个 Windows Server 2012 域控制器 （即您套餐在当前位置升级操作系统的运行 Windows Server 2012 的域控制器）。  
  
在 Windows Server 2012 安装光盘 \support\adprep 文件夹位于 Adprep.exe。Adprep 的 Windows Server 2012 版本是可执行远程。  
  
Windows Server 2012 版本 adprep.exe 可以运行任何运行 64 位版本的 Windows Server 2008 或更高版本的服务器上。 服务器森林和你想要添加的域控制器域的基础结构主需要架构主机到的网络连接。 如果这些角色任一托管运行 Windows Server 2003 的服务器上，然后必须远程运行 adprep。 不必域控制器 adprep 你运行的服务器。 它可以是域连接或在工作组中。  
  
> [!NOTE]  
> 如果你尝试在运行 Windows Server 2003 的服务器上运行的 adprep.exe Windows Server 2012 版本，下面错误显示：  
>   
> Adprep.exe 不是一个有效的 Win32 应用。  
  
![新增功能](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
有关如何解决返回 Adprep.exe 的其他错误的信息，请参阅[已知问题](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)。  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>对 Windows Server 2003 操作主机角色组成员检查  
对于每个命令 (/ forestprep，/domainprep 或 /rodcprep)，Adprep 执行组成员检查，以确定是否指定的凭据表示某些组中的帐户。 若要执行此检查，Adprep 联系人操作主机角色所有者。 如果在操作主机运行的 Windows Server 2003，你需要指定 /user 和 /userdomain 命令行参数，如果你运行的 Adprep.exe 以确保组成员检查执行在所有的情况下。  
  
/User 和 /userdomain 是在 Windows Server 2012 Adprep.exe 的新参数。 这些参数用户的帐户名称和用户域分别指定的用户的运行 adprep 命令。 Adprep.exe 命令行实用程序将阻止指定 /userdomain 和 /user 但省略另一个。  
  
但是，也可以使用 Windows PowerShell 或服务器管理器广告 DS 安装过程中运行 Adprep 操作。 这些体验共享作为 adprep.exe 的相同基础实现 (adprep.dll)。 Windows PowerShell 和 Server 管理器中的体验有其单独的凭据，输入，这没有按 adprep.exe 限制相同的要求。 使用 Windows PowerShell 或服务器管理器，则可能传递到 adprep.dll /user，但不是 /userdomain 的值。 指定 /user，但未指定 /userdomain，如果使用本地计算机域执行检查。 如果计算机未加入域，无法签组成员。  
  
组成员无法检查、 Adprep adprep 日志文件中显示一条警告消息，以及持续：  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
如果你运行而无需指定的 /user 和 /userdomain 参数 Adprep.exe 操作主机运行的 Windows Server 2003，Adprep.exe 联系人的域中的当前登录用户域控制器。 如果当前登录用户不是域帐户，Adprep.exe 无法执行组成员检查。 Adprep.exe 也无法执行组成员检查如果使用智能卡凭据，即使您 /user 和 /userdomain 指定。  
  
Adprep 成功完成后，不需要任何操作。 如果 Adprep 失败访问错误执行期间，请正确的会员与提供的帐户。 有关详细信息，请参阅[凭据要求运行 Adprep.exe 并安装 Active Directory 域服务](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>在 Windows Server 2012 Adprep 语法  
使用以下语法单独从广告 DS 安装运行 adprep:  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
在命令使用 /logdsid，以生成的详细的日志。 位于 %windir%\system32\debug\adprep\logs 日志。  
  
#### <a name="running-adprep-using-smartcard"></a>运行 adprep 使用智能卡  
Adprep.exe 的 Windows Server 2012 版本的工作原理使用智能卡作为凭据，但无法轻松指定智能卡凭据通过命令行。 若要执行此操作的一种方法是获取通过 PowerShell cmdlet 获取凭据智能卡凭据。 然后使用的用户名返回 PSCredential 对象，将显示为`@@...`。 密码已智能卡的 PIN。  
  
如果 /user 指定，Adprep.exe 要求 /userdomain。 智能卡凭据 /userdomain 应该由智能卡基础用户帐户的域。  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>不会自动运行 Adprep /domainprep /gpprep 命令  
广告 DS 安装过程中并非运行 adprep /domainprep /gpprep 命令。 此命令设置所需的结果设置的策略 (RSOP) 权限计划模式的功能。 有关此命令的详细信息，请参阅[Microsoft 知识库文章 324392](https://support.microsoft.com/kb/324392)。 如果命令需要运行 Active Directory 域中，你可以从广告 DS 安装单独运行它。 如果已准备部署运行 Windows Server 2003 SP1 的域控制器在运行该命令或更高版本，该命令不需要重新运行。  
  
你可以放心地添加到现有域运行 adprep /domainprep /gpprep，不运行 Windows Server 2012 的域控制器，但 RSOP 计划模式将无法正常工作。  
  
### <a name="BKMK_PrereqCheck"></a>广告 DS 安装先决条件验证  
广告 DS 安装向导将检查在开始安装之前，满足以下先决条件。 这为您提供有机会更正可以可能阻止安装的问题。  
  
例如，包括 Adprep 相关的先决条件：  
  
-   Adprep 凭据验证： 如果 adprep 需要运行，可验证安装向导用户具有足够执行所需的 Adprep 操作的权限。  
  
-   方案主可用性检查： 如果安装向导确定该 adprep /forestprep 成功需要运行，验证架构主机处于联机状态，否则失败。  
  
-   基础结构主可用性检查： 如果安装向导确定该 adprep /domainprep 需要运行，验证基础主机处于联机状态，否则失败。  
  
从传统的 Active Directory 安装向导 (dcpromo.exe) 传送其他先决条件检查包括：  
  
-   林名称验证： 确保森林名称有效，并且当前不存在。  
  
-   NetBIOS 命名验证： 检查之外 NetBIOS 名称有效，并且不冲突附带现有的名称。  
  
-   组件路径验证： 验证的 Active Directory 的数据库、 日志和 SYSVOL 的路径是否有效，并且，没有足够的磁盘空间可供它们。  
  
-   儿童域名称验证： 确保家长和新的孩子域名有效且，他们不会与冲突现有的域。  
  
-   树域名称验证： 确保指定的树名称有效，并且，它当前不存在。  
  
## <a name="BKMK_SystemReqs"></a>系统要求  
Windows Server 2012 系统要求是 Windows Server 2008 R2 从不变。 有关详细信息，请参阅[Windows Server 2008 R2 SP1 满足系统要求](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)(https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx)。  
  
某些功能可以让其他要求。 例如，则虚拟域控制器克隆功能要求 PDC 模拟器运行 Windows Server 2012 和一台运行 Windows Server 2012 安装了 HYPER-V 角色计算机。  
  
## <a name="BKMK_KnownIssues"></a>已知的问题  
此部分中列出了一些影响广告 DS 安装在 Windows Server 2012 的已知问题。 有关其他的已知问题，请参阅[疑难解答域控制器部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  
  
-   当远程运行 adprep /forestprep 成功 WMI 架构主机访问被 Windows 防火墙阻止，如果在 %systemroot%\system32\debug\adprep adprep 日志中记录以下错误：  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    在此情况下，可以解决该错误由两运行 adprep /forestprep 成功直接在方案主机上，也可以运行以下命令以允许通过 Windows 防火墙 WMI 交通之一。  
  
    对于 Windows Server 2008 或更高版本：  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    适用于 Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Adprep 完成后，你可以运行以下命令再次阻止 WMI 交通之一：  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   你可以键入取消安装 ADDSForest cmdlet Ctrl + C。 取消阻止安装，并且将在还原到服务器的状态所做的任何更改。 但是取消命令发出后，控件不会返回到 Windows PowerShell cmdlet 可以无限期挂起。  
  
-   **如果目标服务器未加入域安装之前，使用智能卡凭据其他域控制器安装将失败。**  
  
    在本例中为返回的错误消息：  
  
    无法连接到复制源域控制器*源控制器域名*。 (例外： Logonfailure： 用户名未知或密码错误)  
  
    如果你加入域的目标服务器，然后执行使用智能卡进行安装时，安装成功。  
  
-   **在 32 位进程下 ADDSDeployment 模块不运行。** 如果你正在自动化部署和配置 Windows Server 2012 使用脚本包含 ADDSDeployment cmdlet 和不支持本机 64 位过程的其他任何 cmdlet，脚本可失败的错误消息，指出找不到 ADDSDeployment cmdlet。  
  
    在此情况下，你需要从不支持本机 64 位进程 cmdlet 单独运行 ADDSDeployment cmdlet。  
  
-   Windows Server 2012 名为复原文件系统没有新的文件系统。 不要使用复原文件系统 (ReFS) 格式化的卷数据上存储 Active Directory 的数据库、 日志文件或 SYSVOL。 有关 ReFS 的详细信息，请参阅[构建适用于 Windows 的下一代文件系统： ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx)。  
  
-   在服务器管理器中，服务器上的服务器 Core 安装运行广告 DS 或其他服务器角色并已升级到 Windows Server 2012，服务器角色可以与红色状态，显示，即使屏幕按预期收集的事件以及状态。 运行 Windows Server 2012 可能还会影响预发行版本的服务器 Core 安装的服务器。  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>如果错误阻止关键复制 active Directory 域服务安装挂起  
如果广告 DS 安装的关键复制阶段遇到了错误，可能会无限期挂起安装。 例如，如果网络错误阻止完成关键复制，不会继续安装。  
  
如果你安装使用服务器管理器，你可能会看到安装进度页仍保持打开状态，但未显示在屏幕上，报告错误和进度可能无法更改大约 15 分钟。 如果你使用的 Windows PowerShell，不会 15 分钟以上更改 Windows PowerShell 窗口中显示的进度。  
  
如果你遇到此问题，请检查 dcpromo.log 文件夹的文件中 %systemroot%\debug 目标服务器上。 日志文件通常表示重复的失败复制。 此问题的一些已知的原因包括：  
  
-   网络问题阻止在提升目标服务器和复制源域控制器之间的关键复制。  
  
    例如，dcpromo.log 显示：  
  
    ```  
  
      05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
      Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    500  
    Reported error information:  
    Error value:   
    Could not find the domain controller for this domain. (1908)  
    directory service:   
    <domain>.com  
    Extensive error information:  
    Error value:   
    A security package specific error occurred. 1825  
    directory service:   
    <DC Name>  
    ```  
  
    因为在安装过程重试关键复制无限期，域控制器安装继续基础网络问题是否解决。 调查根据需要使用工具，例如 ipconfig、 nslookup 和网络监视器网络问题。 确保您提升了域控制器之间存在连接和广告 DS 安装期间选中复制合作伙伴。 还要确保名称分辨率正在工作。  
  
    在开始安装之前期间先决条件检查验证广告网络连接和名称分辨率的 DS 安装要求。 但是，某些的错误情况可以的时间先决条件验证发生并安装完成前，例如如果复制合作伙伴不可用时在安装期间后就会出现。  
  
-   副本域控制器在安装期间，为安装凭据指定目标服务器的本地管理员帐户，并密码的本地管理员帐户匹配域管理员帐户的密码。 这种情况下，你可以完成安装向导中，并开始安装之前你遇到"拒绝访问"失败。  
  
    例如，dcpromo.log 显示：  
  
    ```  
  
    03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
    03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    508  
    Reported error information:  
    Error value:   
    Access is denied. (5)  
    directory service:   
    DC2.contoso.com  
  
    ```  
  
    如果该错误由指定的本地管理员帐户和密码，以便恢复你需要重新安装操作系统，[执行元数据清理](https://technet.microsoft.com/library/cc816907(WS.10).aspx)无法完成安装，然后重新尝试使用域管理员凭据广告 DS 安装了域控制器的帐户。 重新启动服务器将解决此错误，因为服务器指示即使安装未成功完成安装的广告 DS。  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>指定 DNS 非标准化名称时，将警告 active Directory 域服务配置向导  
如果你创建一个新的域，或者林和你指定 DNS 域名称包含国际化尚未标准化的字符，Active Directory 域服务配置向导将显示一个警告，可以失败 DNS 查询的问题的名称。 尽管部署配置页中指定 DNS 域名称，稍后向导中的先决条件检查页面上会显示警告。  
  
如果使用的未标准化的名称，如 füßball.com 或 'ΣΤ.com 指定 DNS 域名称 (标准化版本： füssball.com 和 βστα.com)，调用名称分辨率 Api 前，客户端应用程序尝试访问该与了 winhttp 所将标准化名称。 如果用户在某些对话框中键入"'ΣΤ.com"，因为"βστα.com"和没有 DNS 服务器将匹配它使用"'ΣΤ.com"资源记录将发送 DNS 查询。 用户将无法解决的名称。  
  
下面的示例解释使用尚未标准化 IDN 名称时，会发生问题之一引起：  
  
1.  使用非标准化名称域是创建和 dns 服务器上注册： füßball.com  
  
2.  计算机"nps"已连接到域，并获取注册项目名称： nps.füßball.com  
  
3.  客户端应用程序尝试连接到服务器 nps.füßball.com  
  
4.  尝试解决呼叫名称分辨率 Api 名称 nps.füßball.com 客户端应用程序。  
  
5.  由于标准化，名称获取转换为 nps.füssball.com 和作为 nps.füßball.com 路上查询  
  
6.  客户端应用程序不能解决名称，因为注册的名称是 nps.füßball.com  
  
如果 Active Directory 域服务配置向导中的先决条件检查页面显示警告，返回到部署配置页，并指定标准化的 DNS 域名称。 如果你安装的使用 Windows PowerShell 新域，指定-域名选项标准化的 DNS 名称。  
  
有关 idn，则的详细信息，请参阅[处理国际域名 （idn，则）](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx)。  
  


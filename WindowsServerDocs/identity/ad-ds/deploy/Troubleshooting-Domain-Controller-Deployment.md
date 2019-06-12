---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: 域控制器部署疑难解答
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 15ba8b9d17dfcaf5893086c83811d864c019dac6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443119"
---
# <a name="troubleshooting-domain-controller-deployment"></a>域控制器部署疑难解答

>适用于：Windows Server 2016

本主题介绍有关疑难解答域控制器配置和部署的详细方法。  

## <a name="introduction-to-troubleshooting"></a>疑难解答简介

![疑难解答](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  

## <a name="built-in-logs-for-troubleshooting"></a>内置日志进行故障排除

内置日志是用于解决域控制器升级和降级问题的最重要的工具。 默认情况下，所有这些日志将处于启用状态并配置为最大详细级别。  

|阶段|日志|  
|---------|-------|  
|服务器管理器或 ADDSDeployment Windows PowerShell 操作|-   %systemroot%\debug\dcpromoui.log<br /><br />-   %systemroot%\debug\dcpromoui*.log|  
|域控制器的安装/升级|-   %systemroot%\debug\dcpromo.log<br /><br />-   %systemroot%\debug\dcpromo*.log<br /><br />-事件 viewer\Windows logs\System<br /><br />-事件 viewer\Windows 日志 \ 应用程序<br /><br />-事件 viewer\Applications and services logs\Directory 服务<br /><br />-事件 viewer\Applications and services logs\File 复制服务<br /><br />-事件 viewer\Applications and services logs\DFS 复制|  
|林或域升级|-   %systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-   %systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|服务器管理器 ADDSDeployment Windows PowerShell 部署引擎|-事件 viewer\Applications 和服务日志 \microsoft\windows\directoryservices-deployment\ 可|  
|Windows 服务|-   %systemroot%\Logs\CBS\\*<br /><br />-   %systemroot%\servicing\sessions\sessions.xml<br /><br />-   %systemroot%\winsxs\poqexec.log<br /><br />-   %systemroot%\winsxs\pending.xml|  

### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>用于对域控制器配置进行故障排除的工具和命令

要解决日志未说明的问题，请从使用以下工具开始：  

-   Dcdiag.exe  

-   Repadmin.exe  

-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx)、任务管理器和 MSInfo32.exe  

-   [Network Monitor 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865)（或第三方网络捕获和分析工具）  

### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>域控制器配置疑难解答的常规方法  

1.  是否是一个简单的语法问题导致了该错误？  

    1.  你是否打错了或忘记向 ADDSDeployment Windows PowerShell 提供某个参数？ 例如，如果使用 ADDSDeployment Windows PowerShell，你是否忘记添加所需的带有有效名称的参数 **-domainname**？  

    2.  仔细检查 Windows PowerShell 控制台输出，以查看它未能解析提供的命令行的确切原因。  

2.  该错误是否是先决条件故障？  

    1.  许多曾经好像是致命升级结果的错误现在可以通过先决条件检查器阻止。  

    2.  仔细检查先决条件错误的文本，它们提供用于解决大部分问题的必要指南，因为它们是受控制的方案。  

3.  该错误是否发生在升级中，并因此成为致命错误？  

    1.  仔细检查结果：许多错误有简单的解释，例如错误密码、网络名称解析或关键脱机域控制器。  

    2.  检查 Dcpromoui.log 和 dcpromo.log 以查找显示在输出中的错误，然后从它们往回追溯以查看指示故障原因的迹象。  

        1.  始终与工作示例日志进行比较  

        2.  仅在结果显示出现有关扩展架构或者准备林或域的问题的情况下，才检查 ADPrep 日志中的错误。  

        3.  仅当 Dcpromoui.log 缺少详细信息或由于配置过程中未处理的异常任意结束时，才检查 DirectoryServices-Deployment 事件日志中的错误。  

    3.  检查目录服务、系统和应用程序事件日志以查找配置问题的其他指示。 通常情况下，域控制器升级只是可能影响所有分布式系统的其他网络错误配置的一个症状。  

    4.  使用 dcdiag.exe 和 repadmin.exe 验证林的总体运行状况，并指示可能会阻止进一步域控制器升级的细微配置错误。  

    5.  使用 AutoRuns.exe、任务管理器或 MSinfo32.exe 检查计算机中可能造成干扰的第三方软件。  

        1.  删除第三方软件（不仅仅禁用该软件，这样不会阻止驱动程序加载）。  

    6.  在未能升级的计算机以及复制伙伴域控制器上安装 NetMon 3.4，并使用双向网络捕获分析升级过程。  

        1.  将其与你的工作实验室环境进行比较以了解运行状况良好的升级的样子以及出现故障的位置。  

        2.  此时，错误可能出现在林对象、非默认安全更改或网络中，而且此新域控制器会受到 DNS、防火墙、主机入侵防护软件或其他外部因素中的错误配置的负面影响。  

## <a name="troubleshooting-events-and-error-messages"></a>故障排除的事件和错误消息

域控制器升级和降级始终在操作的最后返回代码，而且和大部分程序不同，它在成功时不返回零。 要查看域控制器配置末尾处的代码，有几个选择：  

1. 当使用服务器管理器时，请在自动重新启动前十秒内检查升级结果。  

2. 在使用 ADDSDeployment Windows PowerShell 时，在自动重新启动前十秒内检查升级结果。 此外，选择不在完成时自动重新启动。 应该添加 **Format-List** 管道以使输出更易于读取。 例如：  

   ```  
   Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  

   ```  

   先决条件审核和验证中的错误不会在重新启动后继续存在，因此它们在所有情况下均可见。 例如：  

   ![疑难解答](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  

3. 在任何方案中，检查 dcpromo.log 和 dcpromoui.log。  

   > [!NOTE]  
   > 由于更高版本操作系统中的操作系统和域控制器配置更改，以下列出的一些错误将不再可能出现。 新的 ADDSDeployment Windows PowerShell 代码也会阻止某些错误，但是 dcpromo.exe /unattend 不会；这是另一个将你当前所有自动化从已弃用的 DCPromo 切换到 ADDSDeployment Windows PowerShell 的令人信服的理由。  

### <a name="promotion-and-demotion-success-codes"></a>升级和降级成功代码

|错误代码|说明|注意|  
|--------------|---------------|--------|  
|1|退出，成功|你仍然必须重新启动，这仅指出已删除自动重新启动标志。|  
|2|退出，成功，需要重新启动||  
|3|退出，成功，带有非关键故障|通常在返回 DNS 委派警告时出现。 如果不配置 DNS 委派，请使用：<br /><br />-creatednsdelegation:$false|  
|4|退出，成功，带有非关键故障，需要重新启动|通常在返回 DNS 委派警告时出现。 如果不配置 DNS 委派，请使用：<br /><br />-creatednsdelegation:$false|  

### <a name="promotion-and-demotion-failure-codes"></a>升级和降级失败代码

升级和降级返回以下失败消息代码。 还可能存在扩展的错误消息；始终仔细阅读整个错误，而不只是数字部分。  


| 错误代码 |                                                           说明                                                            |                                                                                                                            建议的解决方法                                                                                                                            |
|------------|----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     11     |                                          域控制器升级已在运行                                          |                                                                                 不要在同一时间对同一计算机运行域控制器升级的多个实例                                                                                  |
|     12     |                                                    用户必须是管理员                                                    |                                                                                        作为内置管理员组的成员登录并确保你正在使用 UAC 进行提升                                                                                        |
|     13     |                                               已安装证书颁发机构                                               | 你无法使此域控制器降级，因为它同样是证书颁发机构。 在仔细清查其用途之前不要删除 CA - 如果它正在颁发证书，删除该角色将导致中断。 不建议在域控制器上运行 CA |
|     14     |                                                    在安全启动模式下运行                                                     |                                                                                                                      将服务器启动到正常模式                                                                                                                      |
|     15     |                                            角色更改在进行中或需要重新启动                                            |                                                                                             必须在升级前重新启动服务器（由于之前的配置更改）                                                                                              |
|     16     |                                                    在错误的平台上运行                                                     |                                                                                                                       *不太可能出现此错误*                                                                                                                       |
|     17     |                                                      不存在 NTFS 5 驱动器                                                      |                                                                            此错误在 Windows Server 2012 中不可能出现，这至少需要使用 NTFS 格式化 %systemdrive%                                                                             |
|     18     |                                                    windir 中空间不足                                                    |                                                                                                        使用 cleanmgr.exe 释放 %systemdrive% 卷上的空间                                                                                                        |
|     19     |                                                命名更改挂起，需要重新启动                                                 |                                                                                                                             重新启动服务器                                                                                                                              |
|     20     |                                                 计算机名称采用的语法无效                                                  |                                                                                                                   使用有效的名称重命名该计算机                                                                                                                    |
|     21     |                             此域控制器保留 FSMO 角色，它是 GC 和/或 DNS 服务器                             |                                                                                                      添加 **-demoteoperationmasterrole**使用时 **-forceremoval**。                                                                                                      |
|     22     |                                        需要安装 TCP/IP 或不能正常运行                                         |                                                                                                    验证计算机已配置和绑定 TCP/IP，并可以正确工作                                                                                                     |
|     23     |                                             需要先配置 DNS 客户端                                              |                                                                                                  在将新的域控制器添加到域时，设置主 DNS 服务器                                                                                                  |
|     24     |                                  提供的凭据无效或缺少所需的元素                                   |                                                                                                               验证你的用户名和密码正确无误                                                                                                                |
|     25     |                                 无法找到指定域的域控制器                                  |                                                                                                                验证 DNS 客户端设置、防火墙规则                                                                                                                |
|     26     |                                        域列表无法从林中读取                                         |                                                                                                      验证 DNS 客户端设置、LDAP 功能和防火墙规则                                                                                                      |
|     27     |                                                       缺少域名                                                        |                                                                                                                升级或降级时指定域                                                                                                                 |
|     28     |                                                         域名错误                                                          |                                                                                                          升级时选择另一个有效的 DNS 域名                                                                                                          |
|     29     |                                                   父域不存在                                                   |                                                                                             验证在新建子域或树域时指定的父域                                                                                             |
|     30     |                                                       域不在林中                                                       |                                                                                                                      验证提供的域名                                                                                                                       |
|     31     |                                                   子域已存在                                                    |                                                                                                                      指定不同的域名                                                                                                                       |
|     32     |                                                     NetBIOS 域名错误                                                      |                                                                                                                    指定有效的 NetBIOS 域名                                                                                                                     |
|     33     |                                                 指向 IFM 文件的路径无效                                                 |                                                                                                            验证你指向“从媒体安装”文件夹的路径                                                                                                             |
|     34     |                                                     IFM 数据库已损坏                                                      |                                                          针对此操作系统和角色使用正确的“从媒体安装”（相同的操作系统版本，相同类型的域控制器 - RODC 与 RWDC）                                                          |
|     35     |                                                          缺少 SYSKEY                                                          |                                                                                             “从媒体安装”已加密，必须提供有效的 SYSKEY 才能使用它                                                                                              |
|     37     |                                          NTDS 数据库或其日志的路径无效                                           |                                                                                          将数据库和日志的路径更改为固定的 NTFS 卷，不是映射的驱动器或 UNC 路径                                                                                           |
|     38     |                                   卷没有足够的空间用于 NTDS 数据库或日志                                    |                                                                              使用 cleanmgr.exe 释放空间，添加更多磁盘空间，通过将不必要的数据移到别处来手动清理空间                                                                              |
|     39     |                                                    SYSVOL 的路径无效                                                    |                                                                                            将 SYSVOL 文件夹的路径更改为固定的 NTFS 卷，而不是映射驱动器或 UNC 路径                                                                                             |
|     40     |                                                        无效的站点名称                                                         |                                                                                                                      提供存在的站点名称                                                                                                                       |
|     41     |                                             需要为安全模式指定密码                                             |                                                                                为 DSRM 帐户提供密码，无论密码策略如何配置，它不能为空白                                                                                 |
|     42     |                                    安全模式密码不符合条件（仅限升级）                                    |                                                                                         为 DSRM 帐户提供符合密码策略所配置的规则的密码                                                                                          |
|     43     |                                      管理员密码不符合条件（仅限降级）                                       |                                                                                  为本地管理员帐户提供符合密码策略所配置的规则的密码                                                                                  |
|     44     |                                           为林指定的名称无效                                           |                                                                                                                指定有效的目录林根级 DNS 域名                                                                                                                 |
|     45     |                                         带有指定名称的林已经存在                                          |                                                                                                               选择不同的目录林根级 DNS 域名                                                                                                               |
|     46     |                                            树的指定名称无效                                            |                                                                                                                    指定有效的树 DNS 域名                                                                                                                    |
|     47     |                                          带有指定名称的树已经存在                                           |                                                                                                                  选择不同的树 DNS 域名                                                                                                                   |
|     48     |                                       树名不适合林结构                                       |                                                                                                                  选择不同的树 DNS 域名                                                                                                                   |
|     49     |                                               指定的域不存在                                                |                                                                                                                       验证你键入的域名                                                                                                                        |
|     50     | 在降级期间，检测到最后一个域控制器（即使它并不是最后一个），或者指定了最后一个域控制器（但是它并不是最后一个） |        不要指定**域中的最后一个域控制器** ( **-lastdomaincontrollerindomain**)，除非它真的是最后一个。 使用 **-ignorelastdcindomainmismatch**重写如果这是最后一个域控制器，且存在虚拟域控制器元数据        |
|     51     |                                          此域控制器上存在应用分区                                          |                                                                                              指定到**删除应用程序分区** ( **-removeapplicationpartitions**)                                                                                               |
|     52     |            缺少必要的命令行参数（即，必须在命令行上指定应答文件）             |                                                                                              *只能看到与 dcpromo /unattend 一同已弃用的。请参阅早期文档*                                                                                              |
|     53     |                               升级/降级失败，必须重新启动计算机才能清理                                |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     54     |                                                  升级/降级失败                                                   |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     55     |                                         升级/降级已由用户取消                                          |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     56     |                      升级/降级已由用户取消，必须重新启动计算机以进行清理                       |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     58     |                                       必须在 RODC 升级期间指定站点名称                                        |                                                                                           必须为 RODC 指定站点，它将不会像 RWDC 一样自动检测站点                                                                                           |
|     59     |                        在降级期间，此域控制器是它的一个区域的最后一个 DNS 服务器                         |                                                                                    指定这是**域中的最后一个 DNS 服务器**或使用 **-ignorelastdnsserverfordomain**                                                                                     |
|     60     |         域中必须存在运行 Windows Server 2008 或更高版本的域控制器，才能升级 RODC          |                                                                                             升级至少一个 Windows Server 2008 或更高版本的模型可写域控制器                                                                                             |
|     61     |        无法在尚未托管 DNS 的现有域中安装带有 DNS 的 Active Directory 域服务         |                                                                                                                      *不可能出现此错误*                                                                                                                      |
|     62     |                                         应答文件没有 [DCInstall] 部分                                          |                                                                                             *只能看到与 dcpromo /unattend 一同已弃用的。请参阅早期文档。*                                                                                              |
|     63     |                                       林功能级别低于 windows server 2003                                       |                                                            将林功能级别至少提高到 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再是受支持的操作系统                                                             |
|     64     |                                      由于组件二进制检测失败，所以升级失败                                      |                                                                                                                           安装 AD DS 角色                                                                                                                           |
|     65     |                                    由于组件二进制安装失败，所以升级失败                                     |                                                                                                                           安装 AD DS 角色                                                                                                                           |
|     66     |                                      由于操作系统检测失败，所以升级失败                                      |                                  检查展开的错误和日志；服务器未能返回其操作系统版本。 可能需要重新安装计算机，因为其总体运行状况十分可疑                                   |
|     68     |                                                  复制伙伴无效                                                  |                                                                             使用 repadmin.exe 或**Get ADReplication\\**  \* Windows PowerShell 验证伙伴域控制器运行状况                                                                              |
|     69     |                                    所需的端口正在由某些其他应用程序使用                                     |                                                                                    使用 **netstat.exe -anob** 查找错误地分配到保留的 AD DS 端口的进程                                                                                     |
|     70     |                                          目录林根级域控制器必须是 GC                                          |                                                                                              *只能看到与 dcpromo /unattend 一同已弃用的。请参阅早期文档*                                                                                              |
|     71     |                                                 DNS 服务器已安装                                                  |                                                                                          如果已安装 DNS 服务，不要指定安装 DNS ( **-installDNS**)                                                                                           |
|     72     |                                  计算机在非管理员模式下运行远程桌面服务                                   |        无法升级此域控制器，因为它还是针对两个以上管理员用户配置的 RDS 服务器。 在仔细清查其使用情况之前不要删除 RDS - 如果它正在被应用程序或最终用户使用，删除它将导致中断         |
|     73     |                                        指定的林功能级别无效。                                         |                                                                                                                  指定有效的林功能级别                                                                                                                   |
|     74     |                                        指定的域功能级别无效。                                         |                                                                                                                  指定有效的域功能级别                                                                                                                   |
|     75     |                                   无法确定默认的密码复制策略。                                   |                                                                                                验证 RODC 密码复制策略是否存以及是否可访问                                                                                                 |
|     76     |                                 指定的复制/非复制安全组无效                                  |                                                                                验证你在指定密码复制策略时已键入有效的域和用户帐户                                                                                |
|     77     |                                                指定的参数无效                                                 |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     78     |                                            未能检查 Active Directory 林                                             |                                                                                                                    检查展开的错误和日志                                                                                                                     |
|     79     |                                 RODC 无法升级，因为未执行 rodcprep                                  |                                                                                               使用 Windows Server 2012 准备林或使用 **adprep.exe /rodcprep**                                                                                                |
|     80     |                                                未执行 Domainprep                                                 |                                                                                              使用 Windows Server 2012 准备域或使用 **adprep.exe /domainprep**                                                                                               |
|     81     |                                                未执行 Forestprep                                                 |                                                                                              使用 Windows Server 2012 准备林或使用 **adprep.exe /forestprep**                                                                                               |
|     82     |                                                      林架构不匹配                                                      |                                                                                              使用 Windows Server 2012 准备林或使用 **adprep.exe /forestprep**                                                                                               |
|     83     |                                                         不受支持的 SKU                                                          |                                                                                                                       *不太可能出现此错误*                                                                                                                       |
|     84     |                                           无法检测域控制器帐户                                           |                                                                                         验证现有的域控制器是否具有正确的用户帐户控制属性集。                                                                                         |
|     85     |                                     无法为阶段 2 选择域控制器帐户                                     |                                                 如果指定“使用现有帐户”，但没有找到帐户或在帐户查找期间发生错误，则返回该问题。 确保已提供正确的 RODC 分步帐户                                                 |
|     86     |                                                  需要运行阶段 2 升级                                                   |                                                                       如果升级其他域控制器，但是存在现有帐户且未指定“允许重新安装”，则返回该问题                                                                       |
|     87     |                                      存在冲突类型的域控制器帐户                                      |   如果不尝试附加到未占用的域控制器，则在升级前重命名计算机。 必须将附加到未占用的域控制器帐户使用 **-useexistingaccount**和正确的只读或可写参数，具体取决于帐户类型    |
|     88     |                                             指定的服务器管理员无效                                              |                                                                           你为 RODC 管理员委派指定了无效的帐户。 验证指定的帐户是有效的用户或组                                                                           |
|     89     |                                         指定域的 RID 的主机处于脱机状态。                                          |                                                                 使用 **netdom.exe query fsmo** 检测 RID 主机。 使其联机，并使其可供你正在升级的域控制器访问                                                                  |
|     90     |                                                 域命名主机处于脱机状态。                                                 |                                                            使用 **netdom.exe query fsmo** 检测域命名主机。 使其联机，并使其可供你正在升级的域控制器访问                                                             |
|     91     |                                             未能检测进程是否为 wow64                                             |                                                                                                  *不可能出现此错误，操作系统都是 64 位*                                                                                                  |
|     92     |                                                  Wow64 进程不受支持                                                  |                                                                                                  *不可能出现此错误，操作系统都是 64 位*                                                                                                  |
|     93     |                                域控制器服务不为非强制降级运行                                |                                                                                                                          启动 AD DS 服务                                                                                                                           |
|     94     |                           本地管理员密码不符合要求：空白或不需要                           |                                                                                         提供非空白密码并确保本地密码策略需要密码                                                                                         |
|     95     |              在存在动态 RODC 的域中，无法降级最后一个 Windows Server 2008 或更高版本的域控制器              |                                                                             在将降级所有 Windows Server 2008 或更高版本的可写域控制器之前，必须先降级所有 RODC                                                                             |
|     96     |                                                 无法卸载 DS 二进制文件                                                  |                                                                                              *只能看到与 dcpromo /unattend 一同已弃用的。请参阅早期文档*                                                                                              |
|     97     |                      林功能级别版本高于子域操作系统的版本                       |                                                                                           提供版本等于或高于林功能级别的子域功能                                                                                            |
|     98     |                                        正在进行组件二进制安装/卸载。                                        |                                                                                              *只能看到与 dcpromo /unattend 一同已弃用的。请参阅早期文档*                                                                                              |
|     99     |                              林功能级别过低（错误仅限 Windows Server 2012）                              |                                                            将林功能级别至少提高到 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再是受支持的操作系统                                                             |
|    100     |                              域功能级别过低（错误仅限 Windows Server 2012）                              |                                                            将域功能级别至少提高到 Windows Server 2003 本机。 Windows 2000 和 Windows NT 4.0 不再是受支持的操作系统                                                             |

## <a name="known-issues-and-common-support-scenarios"></a>已知的问题和常见的支持方案

以下是 Windows Server 2012 开发过程中的常见问题。 所有这些问题都是“特意设计的”，它们具有有效的解决方法或具有更适合的技术以在第一时间避免它们。 许多这些行为在 Windows Server 2008 R2 和较早版本的操作系统中相同，但是 AD DS 部署的重写使系统对问题更为敏感。  

|问题|降级域控制器会使 DNS 在无区域的情况下运行|  
|---------|-----------------------------------------------------------------|  
|症状|服务器仍然响应 DNS 请求，但是没有区域信息|  
|解析和注释|删除 AD DS 角色时，还会删除 DNS 服务器角色或将 DNS 服务器服务设置为禁用。 记得将 DNS 客户端指向它本身之外的另一个服务器。 如果使用 Windows PowerShell，在降级服务器后运行以下内容：<br /><br />代码-卸载 windowsfeature dns<br /><br />或<br /><br />代码-设置服务 dns starttype 禁用<br />停止服务 dns|  

|问题|在将 Windows Server 2012 升级到现有单标签域中时，不会配置 updatetopleveldomain=1 或 allowsinglelabeldnsdomain=1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|症状|DNS 动态记录注册不会发生|  
|解析和注释|使用 Netlogon 和 DNS 组策略设置这些值。 Microsoft 在 Windows Server 2008 中开始阻止创建单标签域；可使用 ADMT 或域重命名工具更改为已批准的 DNS 域结构。|  

|问题|如果存在预创建且未占用的 RODC 帐户，域中最后一个域控制器降级将失败|  
|---------|------------------------------------------------------------------------------------------------------------|  
|症状|降级失败并显示消息：<br /><br />**Dcpromo.General.54**<br /><br />Active Directory 域服务找不到另一个 Active Directory 域控制器以传输目录分区中的剩余数据，CN=Schema、CN=Configuration、DC=corp、DC=contoso、DC=com。<br /><br />“指定域名的格式无效。”|  
|解析和注释|在降级域前，使用 **Dsa.msc** 或 **Ntdsutil.exe 元数据清理**删除任何剩余的预创建 RODC 帐户。|  

|问题|自动化林和域准备不会运行 GPPREP|  
|---------|---------------------------------------------------------------|  
|症状|组策略的跨域计划功能、策略的结果集 (RSOP) 计划模式，需要更新的文件系统和现有 GP 的 Active Directory 权限。 没有 Gpprep，你无法跨域使用 RSOP 计划。|  
|解析和注释|为之前没有针对 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 准备的所有域手动运行 **adprep.exe /gpprep** 。 管理员应当在域的历史记录中仅运行一次 GPPrep，而不是每次升级都运行。 它并非由自动的 adprep 运行，因为如果你已设置足够的自定义权限，它将导致所有 SYSVOL 内容在所有域控制器上重新复制。|  

|问题|当指向 UNC 路径时，从媒体安装未能验证|  
|---------|------------------------------------------------------------------|  
|症状|返回的错误：<br /><br />代码-无法验证媒体路径。 使用"2"的参数调用"GetDatabaseInfo"的异常。 文件夹不是有效的。|  
|解析和注释|必须在本地磁盘而不是远程 UNC 路径上存储 IFM 文件。 这次目的性的阻止防止了由网络中断导致的部分服务器升级。|  

|问题|在域控制器升级期间显示了两次的 DNS 委派警告|  
|---------|-------------------------------------------------------------------------|  
|症状|返回警告*两次*使用 ADDSDeployment Windows PowerShell 进行升级时：<br /><br />代码-"无法创建此 DNS 服务器的委派，因为找不到权威的父区域或不运行 Windows DNS 服务器。 如果您要与现有 DNS 基础结构集成，应以确保来自域之外的可靠名称解析在父区域中手动创建对此 DNS 服务器的委派。 否则，不不需要任何操作。"|  
|解析和注释|忽略。 ADDSDeployment Windows PowerShell 在先决条件检查期间首次显示警告，然后在配置域控制器期间再次显示。 如果不希望配置 DNS 委派，则使用参数：<br /><br />Code - -creatednsdelegation:$false<br /><br />*不*要跳过先决条件检查以禁止显示此消息|  

|问题|在配置期间指定 UPN 或非域凭据将返回误导性错误|  
|---------|--------------------------------------------------------------------------------------------|  
|症状|服务器管理器返回错误：<br /><br />代码的调用"DNSOption"与"6"的参数异常<br /><br />ADDSDeployment Windows PowerShell 返回错误：<br /><br />代码-用户权限验证失败。 必须提供此用户帐户所属的域的名称。|  
|解析和注释|确保以**域\用户**的格式提供有效的域凭据。|  

|问题|使用 Dism.exe 删除 DirectoryServices-DomainController 角色将导致无法启动服务器|  
|---------|---------------------------------------------------------------------------------------------------|  
|症状|如果在正常降级域控制器前使用 Dism.exe 删除 AD DS 角色，则服务器不再正常启动并显示错误：<br /><br />代码-状态：0x000000000<br />信息：发生意外的错误。|  
|解析和注释|使用 *Shift+F8* 启动到目录服务修复模式。 添加回 AD DS 角色，然后强制降级域控制器。 此外，从备份还原系统状态。 不要将为 AD DS 角色删除使用 Dism.exe；该实用工具不了解域控制器。|  

|问题|在将林模式设置为 Win2012 时，无法安装新林|  
|---------|--------------------------------------------------------------------|  
|症状|使用 ADDSDeployment Windows PowerShell 的升级将返回错误：<br /><br />Code -  Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />域控制器升级的先决条件验证失败。 指定的域功能级别无效|  
|解析和注释|如果没有*同时*指定 Win2012 的域功能模式，就不要指定 Win2012 的林功能模式。 以下是一个可以正确工作的示例：<br /><br />代码--forestmode Win2012 的域模式 Win2012]|  

|||  
|-|-|  
|问题|在“从媒体安装”选择区域中单击“验证”似乎不会执行任何操作|  
|症状|指定到 IFM 文件夹的路径时，单击**验证**按钮从不返回任何消息或似乎不会执行任何操作。|  
|解析和注释|**验证**按钮仅在有问题时返回错误。 否则，如果已提供 IFM 路径，它将使**下一步**按钮可供选择。 如果已选择 IFM，必须单击**验证**以继续操作。|  

|||  
|-|-|  
|问题|使用服务器管理器进行升级在完成之前不会提供反馈。|  
|症状|使用服务器管理器删除 AD DS 角色并降级域控制器时，不提供持续的反馈，直到降级完成或失败。|  
|解析和注释|这是服务器管理器的限制。 要获取反馈，请使用 ADDSDeployment Windows PowerShell cmdlet：<br /><br />Code - Uninstall-addsdomaincontroller|  

|||  
|-|-|  
|问题|“从媒体安装”验证不会检测为可写域控制器提供的该 RODC 媒体，反之亦然。|  
|症状|在使用 IFM 升级新域控制器和向 IFM 提供不正确的媒体时（例如用于可写域控制器的 RODC 媒体，或用于 RODC 的 RWDC 媒体），“验证”按钮不返回错误。 稍后，升级失败并显示错误：<br /><br />代码-尝试将此计算机配置为域控制器时出错。 <br />从媒体安装的提升的只读 DC 无法启动，因为不允许指定的源数据库。 仅从其他 Rodc 的数据库可用于 RODC 的 IFM 升级。|  
|解析和注释|“验证”仅验证 FIM 的总体完整性。 不向服务器提供错误的 IFM 类型。 在尝试使用正确的媒体再次进行升级前重新启动服务器。|  

|||  
|-|-|  
|问题|无法将 RODC 升级到预创建的计算机帐户中|  
|症状|使用 ADDSDeployment Windows PowerShell 升级新的带有分步计算机帐户的 RODC 时，返回错误：<br /><br />代码-不能为使用指定的命名参数解析参数集。    <br />InvalidArgument：ParameterBindingException<br />    + FullyQualifiedErrorId:AmbiguousParameterSet,Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|解析和注释|不要提供已在预创建的 RODC 帐户上定义的参数。 这些问题包括：<br /><br />Code - -readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-sitename<br />-installdns|  

|||  
|-|-|  
|问题|取消选中/选中“必要时自动重新启动每个目标服务器”时，没有任何效果|  
|症状|如果选中 （或未选中） 服务器管理器选项**如果需要自动重新启动每个目标服务器**whendemoting 通过角色删除域控制器，该服务器始终重新启动，而不考虑选择。|  
|解析和注释|这是有目的性的。 无论此设置如何，降级进程都将重新启动服务器。|  

|||  
|-|-|  
|问题|Dcpromo.log 显示“[error] setting security on server files failed with 2”|  
|症状|域控制器的降级在没有问题的情况下完成，但是 dcpromo 日志的检查显示错误：<br /><br />代码-[error] 的文件服务器上设置安全性失败，出现 2|  
|解析和注释|忽略，错误符合预期并且属于修饰。|  

|||  
|-|-|  
|问题|先决条件 adprep 检查失败并显示错误“无法执行 Exchange 架构冲突检查”|  
|症状|在尝试将 Windows Server 2012 域控制器升级到现有 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 林中时，先决条件检查失败并显示错误：<br /><br />代码-AD 准备的先决条件验证失败。 无法为域执行 Exchange 架构冲突检查 *<domain name>* (异常： RPC 服务器不可用)<br /><br />adprep.log 显示错误：<br /><br />代码-Adprep 无法检索数据，从服务器 *<domain controller>*<br /><br />通过 Windows Management Instrumentation (WMI)。|  
|解析和注释|新域控制器无法通过 DCOM/RPC 协议根据现有域控制器访问 WMI。 到目前为止，此现象有三个原因：<br /><br />的向现有的域控制器防火墙规则阻止访问<br /><br />-网络服务帐户缺少"作为服务登录"(SeServiceLogonRight) 特权，现有的域控制器上<br /><br />-禁用了 NTLM 域控制器上使用安全策略中所述[NTLM 身份验证限制简介](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  

|||  
|-|-|  
|问题|创建新 AD DS 林始终显示 DNS 警告|  
|症状|在新建 AD DS 林并在新域控制器上为其自身的创建 DNS 区域时，你始终会收到警告消息：<br /><br />DNS 配置过程中检测代码的错误。 <br />无使用此计算机的 DNS 服务器在超时间隔内响应。<br />(错误代码 0x000005B4"ERROR_TIMEOUT")|  
|解析和注释|忽略。 此警告将在新林根域的第一个域控制器中有目的性地出现，以防你要指向现有的 DNS 服务器和区域。|  

|||  
|-|-|  
|问题|Windows PowerShell -whatif 参数会返回不正确的 DNS 服务器信息|  
|症状|使用隐式或显式 **-installdns:$true** 配置域控制器时，如果使用 **-whatif** 参数，得到的输出显示如下：<br /><br />代码-"DNS 服务器：No"|  
|解析和注释|忽略。 将正确安装并配置 DNS。|  

|||  
|-|-|  
|问题|升级后，登录失败并显示“没有足够的存储空间来处理此命令”|  
|症状|在升级新的域控制器，然后注销并尝试以交互方式登录后，你将收到错误：<br /><br />代码-没有足够的存储空间来处理此命令|  
|解析和注释|由于出现错误或你已指定 ADDSDeployment Windows PowerShell 参数 **-norebootoncompletion**，所以在升级后没有重新启动域控制器。 重新启动域控制器。|  

|||  
|-|-|  
|问题|“域控制器选项”页上不提供“下一步”按钮|  
|症状|即使已设置密码，服务器管理器中**域控制器选项**页上的**下一步**按钮仍不可用。 **站点名称**菜单中未列出站点。|  
|解析和注释|你有多个 AD DS 站点且至少一个缺少子网；将来的这个域控制器属于这些子网之一。 必须从“站点名称”下拉菜单手动选择子网。 还应该检查使用 DSSITE.MSC 的所有 AD 站点或使用以下 Windows PowerShell 命令查找所有缺少子网的站点：<br /><br />Code - get-adreplicationsite -filter \* -property subnets &#124; where-object {!$_.subnets -eq "\*"} &#124; format-table name|  

|||  
|-|-|  
|问题|升级或降级失败并显示消息“无法启动服务”|  
|症状|如果尝试升级、降级或克隆域控制器，将收到错误：<br /><br />代码-服务无法启动，因为它是已禁用或它具有与之关联的设备没有启动"(0x80070422)<br /><br />错误可能是交互式、一个事件或者可能写入日志（如 dcpromoui.log 或 dcpromo.log）|  
|解析和注释|DS 角色服务器服务 （DsRoleSvc) 处于禁用状态。 默认情况下，此服务在 AD DS 角色安装期间安装并设置为手动启动类型。 不要禁用此服务。 将其重新设置为手动，并允许 DS 角色操作按需启动和停止它。 此行为是设计使然。|  

|||  
|-|-|  
|问题|服务器管理器仍然警告你需要升级 DC|  
|症状|如果使用已弃用的 dcpromo.exe /unattend 升级域控制器或将现有的 Windows Server 2008 R2 域控制器就地升级到 Windows Server 2012，服务器管理器仍然显示部署后配置任务**将此服务器升级为域控制器**。|  
|解析和注释|单击部署后警告链接，该消息将永久消失。 此行为是修饰性的并且符合预期。|  

|||  
|-|-|  
|问题|缺少角色安装的服务器管理器部署脚本|  
|症状|如果你使用服务器管理器升级域控制器并保存 Windows PowerShell 部署脚本，它不包括角色安装 cmdlet 和参数 (install-windowsfeature -name ad-domain-services -includemanagementtools)。 在没有角色的情况下无法配置 DC。|  
|解析和注释|将该 cmdlet 和参数手动添加到任何脚本。 此行为符合预期并且是设计使然。|  

|||  
|-|-|  
|问题|服务器管理器部署脚本名称不是 PS1|  
|症状|如果使用服务器管理器升级域控制器并保存 Windows PowerShell 部署脚本，文件以随机临时名称命名并且不作为 PS1 文件。|  
|解析和注释|手动重命名文件。 此行为符合预期并且是设计使然。|  

|问题|Dcpromo /unattend 允许不受支持的功能级别|  
|-|-|  
|症状|如果使用带有以下示例应答文件的 dcpromo /unattend 升级域控制器：<br /><br />Code -<br /><br />[DCInstall]<br />NewDomain=Forest<br /><br />ReplicaOrNewDomain=Domain<br /><br />NewDomainDNSName=corp.contoso.com<br /><br />SafeModeAdminPassword=Safepassword@6<br /><br />DomainNetbiosName=corp<br /><br />DNSOnNetwork = Yes<br /><br />AutoConfigDNS=Yes<br /><br />RebootOnSuccess=NoAndNoPromptEither<br /><br />RebootOnCompletion=No<br /><br />*DomainLevel=0*<br /><br />*ForestLevel=0*<br /><br />升级失败并在 dcpromoui.log 中显示以下错误：<br /><br />Code - dcpromoui EA4.5B8 0089 13:31:50.783       Enter CArgumentsSpec::ValidateArgument DomainLevel<br /><br />dcpromoui EA4.5B8 008A 13:31:50.783 DomainLevel 的值为 0<br /><br />dcpromoui EA4.5B8 008B 13:31:50.783 退出代码为 77<br /><br />dcpromoui EA4.5B8 008 C 13:31:50.783 指定的参数无效。<br /><br />dcpromoui EA4.5B8 008 D 13:31:50.783 结束日志<br /><br />dcpromoui EA4.5B8 0032 13:31:50.830 退出代码为 77<br /><br />级别 0 是 Windows 2000，它在 Windows Server 2012 中不受支持。|  
|解析和注释|不要使用已弃用的 dcpromo /unattend，并了解它会让你指定稍后将失败的无效设置。 此行为符合预期并且是设计使然。|  

|问题|升级在正在创建 NTDS 设置对象的"挂起"永远不会完成|  
|-|-|  
|症状|如果升级副本 DC 或 RODC，升级达到"创建 NTDS 设置对象"和永远不会继续执行或完成。 日志也停止更新。|  
|解析和注释|这是一个已知问题，原因在于向内置域管理员帐户提供带有匹配密码的内置本地管理员帐户的凭据。 这会导致核心安装引擎中的故障，此引擎不发生错误但会无限期等待（准循环）。 这预期行为-虽然不可取的行为。<br /><br />若要修复服务器：<br /><br />1.重新启动它。<br /><br />1.在 AD 中，删除该服务器的成员计算机帐户 （它将不尚未是 DC 帐户）<br /><br />1.在该服务器上，使其强制脱离该域<br /><br />1.在该服务器上，删除 AD DS 角色。<br /><br />1.重新启动<br /><br />1.重新添加 AD DS 角色并重新尝试升级，确保始终向 DC 升级提供 ***域\管理员***格式的凭据，而不只是内置本地管理员帐户|  

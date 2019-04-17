---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: "计划工作文件夹部署"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: "如何计划工作文件夹部署（包括系统要求）和如何准备网络环境。"
ms.openlocfilehash: 877b418439e77e39cbdc6821808e296f977c26dd
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="planning-a-work-folders-deployment"></a>计划工作文件夹部署

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows 10、Windows 8.1、Windows 7

本主题介绍工作文件夹实施方案的设计过程，这些内容假设你已拥有以下知识背景：  
  
-   基本了解工作文件夹（参阅[工作文件夹](work-folders-overview.md)中的介绍）  
  
-   基本了解 Active Directory 域服务 (AD DS) 的概念  
  
-   基本了解 Windows 文件共享和相关技术  
  
-   基本了解 SSL 证书用法  
  
-   基本了解如何通过 Web 反向代理启用对内部资源的 Web 访问  
  
 下列部分将会帮助你设计工作文件夹实施方案。 下一主题[部署工作文件夹](deploy-work-folders.md)中介绍了工作文件夹的部署。  
  
##  <a name="BKMK_SOFT"></a> 软件要求  

工作文件夹要求在文件服务器和网络基础结构方面符合以下软件要求：  
  
-   运行 Windows Server 2012 R2 或 Windows Server 2016 的服务器，用于托管与用户文件的同步共享  
  
-   一个使用 NTFS 文件系统格式化的卷，用于存储用户文件  
  
-   若要强制在 Windows 7 电脑上执行密码策略，则必须使用组策略密码策略。 你还需要从工作文件夹密码策略（如果你使用它们）排除 Windows 7 电脑。

-   为将要托管工作文件夹的每个文件服务器提供一个服务器证书。 这些证书应该来自用户信任的证书颁发机构 (CA) - 最好是公共 CA。

-   （可选）在 Windows Server 2012 R2 中提供一个包含架构扩展的 Active Directory 域服务林，以便在使用多个文件服务器时支持将电脑和设备自动定向到正确的文件服务器。 
  
若要使用户能够通过 Internet 进行同步，还需要满足以下要求：  
  
-   允许用户在组织的反向代理或网关中创建发布规则，以便通过 Internet 访问服务器  
  
-   （可选）提供一个公开注册的域名，并且能够为该域创建其他的公用 DNS 记录  
  
-   （可选）Active Directory 联合身份验证服务 (AD FS) 基础结构（如果使用 AD FS 身份验证）  
  
工作文件夹要求在客户端计算机方面符合以下软件要求：  
  
-   计算机必须运行下列操作系统之一：  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   Android 4.4 KitKat 及更高版本  
  
    -   iOS 10.2 及更高版本  
  
-   Windows 7 电脑必须运行下列 Windows 版本操作系统之一：  
  
    -   Windows 7 专业版  
  
    -   Windows 7 旗舰版  
  
    -   Windows7 企业版  
  
-   Windows 7 电脑都必须加入到你的组织域中（它们不能将加入到工作组）。  
  
-   NTFS 格式化的本地驱动器中有足够的可用空间用于存储工作文件夹中的所有用户文件。此外，如果工作文件夹位于系统驱动器中（默认设置），则还需要额外提供 6 GB 可用空间。 默认情况下，工作文件夹使用以下位置：**%USERPROFILE%\Work Folders**  
  
     但是，在设置过程中，用户可以更改该位置（使用 NTFS 文件系统格式化的 microSD 卡和 USB 驱动器是受支持的位置，不过，如果移除这些驱动器，同步将会停止）。  
  
     默认情况下，单个文件的最大大小为 10 GB。 每个用户的存储没有限制，不过，管理员可以使用文件服务器资源管理器的配额功能来实施配额。  
  
-   工作文件夹不支持回滚客户端虚拟机的虚拟机状态。 但允许使用系统映像备份或其他备份应用从客户端虚拟机内部执行备份和还原操作。  
  
> [!NOTE]
>  请确保在所有工作文件夹服务器和任何运行 Windows 8.1 或 Windows Server 2012 R2 的客户端计算机上安装 Windows 8.1 和 Windows Server 2012 R2 通用版本更新汇总。 有关详细信息，请参阅 Microsoft 知识库中的文章 [2883200](http://support.microsoft.com/kb/2883200)。  
  
## <a name="deployment-scenarios"></a>部署方案  
 可以在客户环境中任意数目的文件服务器上实施工作文件夹。 这样，便可以根据客户的需求调整工作文件夹实施方案的规模，提高部署的个性化程度。 但是，大多数部署采用下列三种基本方案之一。  
  
### <a name="single-site-deployment"></a>单站点部署  
 在单站点部署中，文件服务器托管在客户基础结构中的中央站点内。 使用高度集中化基础结构的客户，或者其小型分支机构未维护本地文件服务器的客户，经常采用此部署类型。 由于所有服务器资产都是本地的，同时，Internet 入口/出口在此位置很可能已集中化，因此，IT 工作人员可以更方便地管理这种部署模型。 但是，这种部署模型还依赖于中央站点与所有分支机构之间的良好 WAN 连接，出现网络状态问题时，位于分支机构的用户很容易遇到服务中断。  
  
### <a name="multiple-site-deployment"></a>多站点部署  
 在多站点部署中，文件服务器托管在客户基础结构中的多个位置。 这可能意味着需要建立多个数据中心，或者分支机构需要维护不同的文件服务器。 大型客户环境，或者其多个大型分支机构维护本地服务器资产的客户，经常采用此部署类型。 这种部署模型更复杂，IT 人员更难对它进行管理。另外，它还要求客户精心协调数据存储及维护 Active Directory 域服务 (AD DS)，以确保用户为工作文件夹使用正确的同步服务器。  
  
### <a name="hosted-deployment"></a>托管部署  
 在托管部署中，同步服务器部署在 Windows Azure VM 等 IAAS（基础结构即服务）解决方案中。 这种部署方法的优势是使提供的文件服务器对客户企业中的 WAN 连接的依赖性减小。 如果设备能够连接到 Internet，则可访问其同步服务器。 但是，为了对用户进行身份验证，部署在托管环境中的服务器仍然需要能够访问组织的 Active Directory 域，并且，由于维护这种连接带来的复杂性更高，客户必须实际权衡基础结构方面的要求。  
  
## <a name="deployment-technologies"></a>部署技术  
 工作文件夹部署融入了许多的技术，这些技术共同为内部和外部网络中的设备提供服务。 在设计工作文件夹部署之前，客户应该熟悉下列每种技术的要求。  
  
### <a name="active-directory-domain-services"></a>Active Directory 域服务  
 AD DS 在工作文件夹部署中提供两个重要的服务。 首先，作为 Windows 身份验证的后端，AD DS 提供安全和身份验证服务用于授予对用户数据的访问权限。 如果无法访问某个域控制器，文件服务器将无法对传入的请求进行身份验证，并且设备将无法访问该文件服务器的同步共享中存储的任何数据。  
  
 其次，AD DS（装有 Windows Server 2012 R2 架构更新）为每个用户维护 msDS-SyncServerURL 属性，使用该属性可将用户自动定向到相应的同步服务器。  
  
### <a name="file-servers"></a>文件服务器  
 运行 Windows Server 2012 R2 或 Windows Server 2016 的文件服务器托管了工作文件夹角色服务，并托管了用于存储用户工作文件夹数据的同步共享。 文件服务器还可以托管内部网络中运行的其他技术（例如文件共享）存储的数据，并可以组成群集以针对用户数据提供容错。  
  
###  <a name="GroupPolicy"></a>组策略  
 如果在你的环境中有 Windows 7 电脑，我们的建议如下：  
  
-   使用组策略控制使用工作文件夹的所有加入域电脑的密码策略。  
  
-   在未加入域的电脑上使用工作文件夹**自动锁定屏幕，并要求输入密码**策略。  
  
 同时可以使用组策略将工作文件夹服务器指定到已加入域的电脑。 这在一定程度上简化了工作文件夹的设置 - 否则，用户需要输入其工作电子邮件地址来查找设置（假设已正确设置工作文件夹），或者输入你通过电子邮件或其他通信方式明确提供给他们的工作文件夹 URL。  
  
 你还可以使用组策略强制基于用户或计算机设置工作文件夹，不过，这样做会导致工作文件夹在用户登录到的每台电脑上同步（使用按用户策略设置时），并且会阻止用户为其电脑上的工作文件夹指定备用位置（例如，在 microSD 卡上指定一个位置，以节省主驱动器上的空间）。 我们建议你在强制自动设置之前仔细评估用户的需求。  
  
### <a name="windows-intune"></a>Windows Intune  
 Windows Intune 还为未加入域的设备（这些设备只有在未加入域的情况下才能存在）提供了安全性与可管理性层。 你可以使用 Windows Intune 来配置和管理用户的个人设备，例如，通过 Internet 连接到工作文件夹的平板电脑。 Windows Intune 为设备提供要使用的同步服务器 URL - 否则，用户必须输入其工作电子邮件地址才能查找设置（如果你发布了 https://workfolders.*contoso.com* 格式的公用工作文件夹 URL），或者直接输入同步服务器 URL。  
  
 如果不部署 Windows Intune，用户必须手动配置外部设备，这可能会导致对客户技术支持工作人员的要求提高。  
  
 还可以使用 Windows Intune 有选择性地擦除用户设备上的工作文件夹中的数据，且不影响其他数据 - 如果某个用户离职或者其设备失窃，此功能可以提供便利。  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Web 应用程序代理/Azure AD 应用程序代理  
 工作文件夹的设计思路是使连接到 Internet 的设备能够从内部网络安全地检索业务数据，从而让用户借助其平板电脑以及一般情况下无法访问工作文件的设备“随身携带其数据”。 为此，必须使用反向代理发布同步服务器 URL 并将其提供给 Internet 客户端。 
 
工作文件夹支持使用 Web 应用程序代理、Azure AD 应用程序代理或第三方反向代理解决方案： 

-  Web 应用程序代理是一项本地反向代理解决方案。 若要了解详细信息，请参阅 [Windows Server 2016 中的 Web 应用程序代理](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。  
  
-  Azure AD 应用程序代理是一项云反向代理解决方案。 若要了解详细信息，请参阅[如何提供对本地应用程序的安全远程访问](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>其他设计注意事项  
 除了了解上述每个组件外，客户在设计时还需要花费时间来考虑要使用的同步服务器数目和运行的共享数目，以及是否利用故障转移群集在这些同步服务器上提供容错  
  
### <a name="number-of-sync-servers"></a>同步服务器数目  
 客户可以在一个环境中运行多个同步服务器。 出于下列多种原因，可能需要采用此配置：  
  
-   用户的地理位置分散 - 例如，存在多个分支机构文件服务器或区域数据中心  
  
-   数据存储要求 - 某些业务部门可能使用了特定的数据存储，或者需要使用专用服务器来更方便地达到处理要求  
  
-   负载平衡 - 在大型环境中，将用户数据存储在多个服务器上可以提高服务器性能和运行时间。  
  
 有关工作文件夹服务器调整和性能的信息，请参阅[有关工作文件夹部署的性能注意事项](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)。  
  
> [!NOTE]
>  使用多个同步服务器时，我们建议为用户设置自动服务器发现。 此过程依赖于 AD DS 中每个用户帐户的某个属性的配置。 该属性名为 **msDS-SyncServerURL**，将 Windows Server 2012 R2 域控制器添加到域或者应用 Active Directory 架构更新后，便可以在用户帐户上使用该属性。 应该为每个用户设置此属性，以确保用户连接到正确的同步服务器。 通过使用自动服务器发现，不管运行了多少台同步服务器，组织都可以在“友好的”URL（例如 *https://workfolders.contoso.com*）后面发布工作文件夹。  
  
### <a name="number-of-sync-shares"></a>同步共享数目  
 单个同步服务器可以维护多个同步共享。 这对于以下情况可能十分有用：  
  
-   审核和安全要求 - 如果特定部门使用的数据必须接受更严格的审核或者需要长时间保留，则独立的同步共享可以帮助管理员以不同的独立审核级别保留用户文件夹。  
  
-   不同的配额或文件屏蔽 - 如果你要针对不同的用户组设置不同的存储配额，或者限制工作文件夹中允许的文件类型（文件屏蔽），那么，独立的同步共享可以提供帮助。  
  
-   部门控制 - 如果管理职责是按部门分配的，则针对不同的部门利用独立的共享可帮助管理员强制配额或其他策略。  
  
-   不同的设备策略 - 如果组织需要为不同的用户组维护多个设备策略（例如加密工作文件夹），则使用多个共享可以实现此目的。  
  
-   存储容量 - 如果某个文件服务器有多个卷，则可以使用更多的共享来利用这些附加的卷。 单个共享只能访问它所在的卷，而无法利用文件服务器上的其他卷。  
  
#### <a name="access-to-sync-shares"></a>访问同步共享  

尽管用户访问的同步服务器由其客户端上输入的 URL（或者在使用服务器自动发现时为 AD DS 中的该用户发布的 URL）确定，但是，对单个同步共享的访问权限由共享中的权限确定。  
  
因此，如果客户在同一台服务器上托管多个同步共享，则必须保持谨慎，以确保单个用户仅有权访问其中的一个共享。 否则，当用户连接到服务器时，其客户端可能会连接到错误的共享。 可以通过为每个同步共享创建一个独立的安全组来实现此目的。  
  
此外，对同步共享中单个用户文件夹的访问权限由该文件夹的所有权确定。 创建同步共享时，工作文件夹将按默认授予用户对其文件的独占访问权（禁用继承，并使用户成为各自文件夹的所有者）。  
  
## <a name="design-checklist"></a>设计清单  

下面这组设计问题旨在帮助客户设计出最符合其环境需要的工作文件夹实施方案。 在尝试部署服务器之前，客户应解决此清单中的每个问题。  
  
-   目标用户  
  
    -   哪些用户要使用工作文件夹？  
  
    -   用户的组织方式是什么？ （按地理位置、按办公室、按部门等）  
  
    -   是否有任何用户对数据存储、安全或数据保留提出了特殊要求？  
  
    -   是否有任何用户提出了具体的设备策略要求，例如加密？  
  
    -   你需要支持哪些客户端计算机和设备？ （Windows 8.1、Windows RT 8.1、Windows 7）  
  
         若你正在支持 Windows 7 PC 并且想要使用密码策略，请从工作文件夹密码策略中排除存储的计算机帐户的域，并且为该域中加入的 PC 域改用组策略密码策略。  
  
    -   你是否需要与其他用户数据管理解决方案（例如文件夹重定向）互操作或者从中迁移？  
  
    -   多个域中的用户是否需要通过 Internet 与单个服务器同步？  
  
    -   是否需要支持那些不属于已加入域的电脑上的本地管理员组的用户？ （如果是，则需要从加密和密码策略等工作文件夹设备策略中排除相关的域）  
  
-   基础结构和容量规划  
  
    -   应该将同步服务器定位在网络中的哪些站点上？  
  
    -   是否有任何同步服务器由基础结构即服务 (IaaS) 提供程序（例如，Azure VM 中的提供程序）托管？  
  
    -   是否需要为任何特定的用户组使用专用服务器，如果是，每个专用服务器有多少用户？  
  
    -   Internet 入口/出口点在网络中的哪个位置？  
  
    -   是否建立同步服务器的群集以提供容错？  
  
    -   同步服务器是否需要保留多个数据卷用于托管用户数据？  
  
-   数据安全  
  
    -   是否需要在任意同步服务器上创建多个同步共享？  
  
    -   要使用哪些组来提供对同步共享的访问？  
  
    -   如果使用了多个同步服务器，你要为委派功能创建哪个安全组，以便修改用户对象的 msDS-SyncServerURL 属性？  
  
    -   单个同步共享是否有任何特殊的安全或审核要求？  
  
    -   是否需要多重身份验证 (MFA)？  
  
    -   是否需要有远程擦除电脑和设备中的工作文件夹数据的能力？  
  
-   设备访问  
  
    -   要使用哪个 URL 来提供对基于 Internet 的设备的访问（*基于电子邮件的自动服务器发现所需的默认 URL 为 workfolders.domainname*）？  
  
    -   如何将 URL 发布到 Internet？  
  
    -   是否使用自动服务器发现？  
  
    -   是否使用组策略来配置已加入域的电脑？  
  
    -   是否使用 Windows Intune 来配置外部设备？  
  
    -   连接设备是否需要 Device Registration？  
  
## <a name="next-steps"></a>后续步骤  
 设计工作文件夹实施方案后，便可以部署工作文件夹了。 有关详细信息，请参阅[部署工作文件夹](deploy-work-folders.md)。  
  
## <a name="see-also"></a>另请参阅  
 有关其他相关信息，请参阅以下资源。  
  
|内容类型|引用|  
|------------------|----------------|  
|**产品评估**|-   [工作文件夹](work-folders-overview.md)<br />-   [适用于 Windows 7 的工作文件夹](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx)（博客文章）|  
|**部署**|-   [设计工作文件夹实施方案](plan-work-folders.md)<br />-   [部署工作文件夹](deploy-work-folders.md)<br />-   [使用 AD FS 和 Web 应用程序代理 (WAP) 部署工作文件夹](deploy-work-folders-adfs-overview.md)<br />- [使用 Azure AD 应用程序代理部署工作文件夹](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />-   [有关工作文件夹部署的性能注意事项](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [适用于 Windows 7 的工作文件夹（64 位下载）](http://www.microsoft.com/download/details.aspx?id=42558)<br />-   [适用于 Windows 7 的工作文件夹（32 位下载）](http://www.microsoft.com/download/details.aspx?id=42559)<br />-   [工作文件夹测试实验室部署](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx)（博客文章）|

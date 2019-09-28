---
title: DirectAccess 脱机加入域
description: 本指南说明了使用 Windows Server 2016 中的 DirectAccess 执行脱机加入域的步骤。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 229e2955c7f382ff630829990a9dd6485d62652e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388884"
---
# <a name="directaccess-offline-domain-join"></a>DirectAccess 脱机加入域

>适用于：Windows Server（半年频道）、Windows Server 2016

本指南说明了使用 DirectAccess 执行脱机加入域的步骤。 在脱机加入域的过程中，计算机配置为加入域，而无需进行物理连接或 VPN 连接。  
  
本指南包含以下部分：  
  
- 脱机域加入概述  
  
- 脱机加入域的要求
  
- 脱机域加入过程
  
- 执行脱机加入域的步骤  
  
## <a name="offline-domain-join-overview"></a>脱机域加入概述  
Windows Server 2008 R2 中引入的域控制器包括一项称为 "脱机域加入" 的功能。 使用名为 Djoin.exe 的命令行实用程序，你可以将计算机加入域，而无需在完成域加入操作的情况下物理联系域控制器。 使用 Djoin.exe 的一般步骤如下：  
  
1.  运行**djoin.exe/provision**以创建计算机帐户元数据。 此命令的输出是一个 .txt 文件，其中包含一个64编码的 blob。  
  
2.  运行**Djoin.exe/requestODJ**将计算机帐户元数据从 .txt 文件插入目标计算机的 Windows 目录中。  
  
3.  重新启动目标计算机，该计算机将加入到域。  
  
### <a name="BKMK_ODJOverview"></a>与 DirectAccess 策略的脱机域加入方案概述  
DirectAccess 脱机加入域的过程中，运行 Windows Server 2016、Windows Server 2012、Windows 10 和 Windows 8 的计算机可以使用来加入域，而无需以物理方式加入公司网络或通过 VPN 进行连接。 这样，便可以将计算机从没有连接到公司网络的位置加入到域。 用于 DirectAccess 的脱机域加入向客户端提供 DirectAccess 策略，以允许进行远程设置。  
  
域加入会创建计算机帐户，并在运行 Windows 操作系统的计算机和 Active Directory 域之间建立信任关系。  
  
## <a name="BKMK_ODJRequirements"></a>准备脱机加入域  
  
1.  创建计算机帐户。  
  
2.  清点计算机帐户所属的所有安全组的成员身份。  
  
3.  收集要应用于新客户端的所需的计算机证书、组策略和组策略对象。  
  
. 以下部分介绍了使用 Djoin.exe 执行 DirectAccess 脱机加入域的操作系统要求和凭据要求。  
  
### <a name="operating-system-requirements"></a>操作系统要求  
只能在运行 Windows Server 2016、Windows Server 2012 或 Windows 8 的计算机上运行 DirectAccess 的 Djoin.exe。 运行 Djoin.exe 将计算机帐户数据预配到 AD DS 的计算机必须运行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。 要加入域的计算机还必须运行 Windows Server 2016、Windows 10、Windows Server 2012 或 Windows 8。  
  
### <a name="credential-requirements"></a>凭据要求  
若要执行脱机域加入，你必须拥有将工作站加入域所需的权限。 默认情况下，"域管理员" 组的成员具有这些权限。 如果你不是 Domain Admins 组的成员，则 Domain Admins 组的成员必须完成下列操作之一，才能使你将工作站加入到域：  
  
-   使用组策略授予所需的用户权限。 此方法允许你在默认的计算机容器和以后创建的任何组织单位（OU）中创建计算机（如果未添加拒绝访问控制项（Ace））。  
  
-   编辑域的 "默认计算机" 容器的访问控制列表（ACL），以将正确的权限委派给你。  
  
-   创建 OU，并编辑该 OU 上的 ACL，以向你授予**Create Child Allow**权限。 将 **/machineOU**参数传递给**djoin.exe/provision**命令。  
  
以下过程说明如何向用户授予组策略以及如何委托正确的权限。  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>授予用户将工作站加入域的权限  
你可以使用组策略管理控制台（GPMC）来修改域策略或创建新策略，该策略具有向用户授予向域中添加工作站的权限的设置。  
  
**Domain Admins**中的成员身份或同等身份是授予用户权限的最低要求。  有关使用适当帐户和组成员身份的详细信息，请参阅[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)（ https://go.microsoft.com/fwlink/?LinkId=83477) 。   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>授予将工作站加入域的权限  
  
1.  依次单击 "**开始**"、"**管理工具**"，然后单击**组策略管理**"。  
  
2.  双击林的名称，双击 "**域**"，双击要在其中加入计算机的域的名称，右键单击 "**默认域策略**"，然后单击 "**编辑**"。  
  
3.  在控制台树中，依次双击 "**计算机配置**"、"**策略**"、" **Windows 设置**"、"**安全设置**"、"**本地策略**"，然后双击**用户权限分配**。  
  
4.  在详细信息窗格中，双击 "**将工作站添加到域**中"。  
  
5.  选中 "**定义这些策略设置**" 复选框，然后单击 "**添加用户或组**"。  
  
6.  键入要向其授予用户权限的帐户的名称，然后单击 **"确定"** 两次。  
  
## <a name="BKMK_ODKSxS"></a>脱机域加入过程  
在提升的命令提示符下运行 Djoin.exe 以设置计算机帐户元数据。 当你运行 "设置" 命令时，将在你指定为命令的一部分的二进制文件中创建计算机帐户元数据。  
  
有关用于在脱机加入域期间设置计算机帐户的 NetProvisionComputerAccount 函数的详细信息，请参阅[NetProvisionComputerAccount 函数](https://go.microsoft.com/fwlink/?LinkId=162426)（ https://go.microsoft.com/fwlink/?LinkId=162426) 。 有关在目标计算机本地运行的 NetRequestOfflineDomainJoin 函数的详细信息，请参阅[NetRequestOfflineDomainJoin 函数](https://go.microsoft.com/fwlink/?LinkId=162427)（ https://go.microsoft.com/fwlink/?LinkId=162427) 。  
  
## <a name="BKMK_ODJSteps"></a>执行 DirectAccess 脱机加入域的步骤  
脱机加入域过程包括以下步骤：  
  
1.  为每个远程客户端创建新的计算机帐户，并使用企业网络中已加入域的计算机上的 Djoin.exe 命令生成预配包。  
  
2.  将客户端计算机添加到 DirectAccessClients 安全组  
  
3.  将设置包安全地传输到将加入域的远程计算机。  
  
4.  应用预配包，并将客户端加入域。  
  
5.  重新启动客户端以完成域加入并建立连接。  
  
为客户端创建预配数据包时，需要考虑两个选项。 如果使用入门向导来安装没有 PKI 的 DirectAccess，则应使用下面的选项1。 如果你使用了高级安装向导来安装包含 PKI 的 DirectAccess，则应使用下面的选项2。  
  
完成以下步骤以执行脱机加入域：  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>选项 1：为不带 PKI 的客户端创建预配包  
  
1.  在远程访问服务器的命令提示符下，键入以下命令以预配计算机帐户：  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>选项2使用 PKI 为客户端创建预配包  
  
1.  在远程访问服务器的命令提示符下，键入以下命令以预配计算机帐户：  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>将客户端计算机添加到 DirectAccessClients 安全组  
  
1.  在域控制器上的 "**开始**" 屏幕中，键入**Active** ，然后选择 "从**应用** **Active Directory 用户和计算机**" 屏幕。  
  
2.  展开域下的树，然后选择 "**用户**" 容器。  
  
3.  在详细信息窗格中，右键单击 " **DirectAccessClients**"，然后单击 "**属性**"。  
  
4.  在“成员” 选项卡上，单击“添加”。  
  
5.  单击 "**对象类型**"，选择 "**计算机**"，然后单击 **"确定"** 。  
  
6.  键入要添加的客户端名称，然后单击 **"确定"** 。  
  
7.  单击 **"确定"** 关闭 " **DirectAccessClients**属性" 对话框，然后关闭**Active Directory 用户和计算机**"。  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>复制预配包，然后将其应用于客户端计算机  
  
1.  将预配包从远程访问服务器上的 c:\files\provision.txt 复制到客户端计算机上的 c:\provision\provision.txt。  
  
2.  在客户端计算机上，打开提升的命令提示符，然后键入以下命令以请求加入域：  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  重新启动客户端计算机。 计算机将加入到域。 重新启动之后，客户端将加入到域中，并通过 DirectAccess 连接到公司网络。  
  
## <a name="see-also"></a>请参阅  
[NetProvisionComputerAccount 函数](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin 函数](https://go.microsoft.com/fwlink/?LinkId=162427)  
  



---
title: DirectAccess 脱机加入域
description: 本指南介绍了执行与 DirectAccess 脱机域加入 Windows Server 2016 中的步骤。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59b5933a81c7021e58ea14e6ea4c4da374ce35cb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283661"
---
# <a name="directaccess-offline-domain-join"></a>DirectAccess 脱机加入域

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南介绍的步骤执行与 DirectAccess 脱机域加入。 在脱机加入域，计算机配置为加入域而无需物理计算机或 VPN 连接。  
  
本指南包含以下部分：  
  
- 脱机域加入概述  
  
- 脱机加入域的要求
  
- 脱机域加入过程
  
- 执行脱机加入域的步骤  
  
## <a name="offline-domain-join-overview"></a>脱机域加入概述  
在 Windows Server 2008 R2 中引入，域控制器包含一个称为脱机域加入功能。 名为 Djoin.exe 命令行实用工具，可以将计算机加入到域，而无需以物理方式完成域加入操作时联系域控制器。 以便使用 Djoin.exe 的常规步骤如下：  
  
1.  运行**djoin /provision**来创建计算机帐户元数据。 此命令的输出是包含的 base-64 编码的 blob 的.txt 文件。  
  
2.  运行**djoin /requestODJ**从.txt 文件将计算机帐户元数据，插入的目标计算机的 Windows 目录。  
  
3.  重新启动目标计算机，计算机将加入到域。  
  
### <a name="BKMK_ODJOverview"></a>脱机加入域的 DirectAccess 策略方案概述  
DirectAccess 脱机加入域是一个过程，运行 Windows Server 2016、 Windows Server 2012、 Windows 10 和 Windows 8 的计算机可用于加入域，无需以物理方式要联接到公司网络，或通过 VPN 建立连接。 这样就可以从位置将计算机加入到域没有任何连接到公司网络。 脱机加入域的 DirectAccess 客户端以允许远程预配到提供 DirectAccess 策略。  
  
加入域的创建计算机帐户，并建立运行 Windows 操作系统和 Active Directory 域的计算机之间的信任关系。  
  
## <a name="BKMK_ODJRequirements"></a>准备以为脱机加入域  
  
1.  创建计算机帐户。  
  
2.  清点的计算机帐户所属的所有安全组的成员身份。  
  
3.  收集所需的计算机证书，组策略和组策略对象以应用于新的客户端。  
  
. 以下各节说明操作系统要求和用于执行 DirectAccess 脱机加入域的使用 Djoin.exe 凭据要求。  
  
### <a name="operating-system-requirements"></a>操作系统要求  
仅在运行 Windows Server 2016、 Windows Server 2012 或 Windows 8 的计算机上，可以为 DirectAccess 运行 Djoin.exe。 在其运行 Djoin.exe 到计算机帐户数据的预配到 AD DS 的计算机必须运行 Windows Server 2016、 Windows 10、 Windows Server 2012 或 Windows 8。 Windows Server 2016、 Windows 10、 Windows Server 2012 或 Windows 8，则还必须运行你想要加入到域的计算机。  
  
### <a name="credential-requirements"></a>凭据要求  
若要执行脱机加入域，必须具有所需的工作站加入到域的权限。 默认情况下，Domain Admins 组的成员具有这些权限。 如果你不是 Domain Admins 组的成员，Domain Admins 组的成员必须完成以下操作之一来使您能够加入到域的工作站：  
  
-   使用组策略来授予所需的用户权限。 此方法，可创建计算机位于默认计算机容器和更高版本 （如果没有拒绝访问控制项 (Ace) 添加） 创建任何组织单位 (OU) 中。  
  
-   编辑要委托给你的正确权限的域的默认计算机容器的访问控制列表 (ACL)。  
  
-   创建 OU 并编辑来向你授予该 OU 上的 ACL**创建子-允许**权限。 传递 **/machineOU**参数**djoin /provision**命令。  
  
以下过程演示如何授予使用组策略的用户权限以及如何委托的正确权限。  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>授予用户权限，以便将工作站加入到域  
可以使用组策略管理控制台 (GPMC) 来修改域策略或创建新的策略具有授予用户权限，将工作站添加到域的设置。  
  
中的成员身份**Domain Admins**，或等效身份是授予用户权限所需的最低。  查看详细了解如何使用适当帐户和组成员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)(https://go.microsoft.com/fwlink/?LinkId=83477) 。   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>若要授予权限以加入域的工作站  
  
1.  单击**启动**，单击**管理工具**，然后单击**组策略管理**。  
  
2.  双击林的名称中，双击**域**，双击想要将计算机加入，请右键单击域的名称**默认域策略**，然后单击**编辑**。  
  
3.  在控制台树中，双击**计算机配置**，双击**策略**，双击**Windows 设置**，双击**安全设置**，双击**本地策略**，然后双击**用户权限分配**。  
  
4.  在详细信息窗格中，双击**将工作站添加到域**。  
  
5.  选择**定义这些策略设置**复选框，然后依次**添加用户或组**。  
  
6.  键入你想要授予用户权限，然后单击帐户名称**确定**两次。  
  
## <a name="BKMK_ODKSxS"></a>脱机域加入过程  
若要预配计算机帐户元数据的提升命令提示符下运行 Djoin.exe。 运行预配命令时，指定为命令的一部分的二进制文件中创建计算机帐户元数据。  
  
有关 NetProvisionComputerAccount 函数，它用于在脱机域加入期间设置的计算机帐户的详细信息，请参阅[NetProvisionComputerAccount 函数](https://go.microsoft.com/fwlink/?LinkId=162426)(https://go.microsoft.com/fwlink/?LinkId=162426) 。 有关在目标计算机本地运行 NetRequestOfflineDomainJoin 函数的详细信息，请参阅[NetRequestOfflineDomainJoin 函数](https://go.microsoft.com/fwlink/?LinkId=162427)(https://go.microsoft.com/fwlink/?LinkId=162427) 。  
  
## <a name="BKMK_ODJSteps"></a>执行 DirectAccess 脱机加入域的步骤  
脱机域加入过程包括以下步骤：  
  
1.  为每个远程客户端创建新的计算机帐户，并生成预配包使用 Djoin.exe 命令从域已加入企业网络中的计算机。  
  
2.  将客户端计算机添加到 DirectAccessClients 安全组  
  
3.  将预配包安全地传输到将加入域的远程计算机 (s)。  
  
4.  应用预配包并将客户端加入到域。  
  
5.  重新启动客户端完成域加入，并建立连接。  
  
有两个客户端创建预配数据包时要考虑的选项。 如果使用开始向导来安装而无需 PKI 的 DirectAccess，您应使用选项 1 下面。 如果使用高级安装向导以使用 PKI 安装 DirectAccess，您应使用选项下面的 2。  
  
完成以下步骤来执行脱机域加入：  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>选项 1：如果没有 PKI 客户端创建预配包  
  
1.  在您的远程访问服务器的命令提示符下键入以下命令来预配的计算机帐户：  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>选项 2:为使用 PKI 客户端创建预配包  
  
1.  在您的远程访问服务器的命令提示符下键入以下命令来预配的计算机帐户：  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>将客户端计算机添加到 DirectAccessClients 安全组  
  
1.  在域控制器上，从**启动**屏幕上，键入**Active** ，然后选择**Active Directory 用户和计算机**从**应用**屏幕.  
  
2.  展开你的域下的树，然后选择**用户**容器。  
  
3.  在细节窗格中，右键单击**DirectAccessClients**，然后单击**属性**。  
  
4.  在“成员”  选项卡上，单击“添加”  。  
  
5.  单击**对象类型**，选择**计算机**，然后单击**确定**。  
  
6.  键入客户端名称以添加，然后单击**确定**。  
  
7.  单击**确定**以关闭**DirectAccessClients**属性对话框中，然后再关闭**Active Directory 用户和计算机**。  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>复制并将预配包应用到客户端计算机  
  
1.  从在远程访问服务器上，保存的位置，为客户端计算机上的 c:\provision\provision.txt c:\files\provision.txt 复制预配包。  
  
2.  在客户端计算机上，打开提升的命令提示符，然后键入以下命令，以请求加入域：  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  重新启动客户端计算机。 计算机将加入到域。 重新启动，客户端将加入到域且已连接到 DirectAccess 的企业网络。  
  
## <a name="see-also"></a>请参阅  
[NetProvisionComputerAccount 函数](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin 函数](https://go.microsoft.com/fwlink/?LinkId=162427)  
  



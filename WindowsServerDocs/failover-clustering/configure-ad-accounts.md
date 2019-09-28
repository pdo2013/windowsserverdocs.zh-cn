---
title: 在 Active Directory 中配置群集帐户
ms.date: 11/12/2012
ms.prod: windows-server
ms.technology: storage-failover-clustering
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 8a540361cdd07f6adfc1c929d77c510ef8433d6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369891"
---
# <a name="configuring-cluster-accounts-in-active-directory"></a>在 Active Directory 中配置群集帐户

适用于：Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，windows server 2008

在 Windows Server 中，当你创建故障转移群集并配置群集服务或应用程序时，故障转移群集向导会创建必需的 Active Directory 计算机帐户（也称为计算机对象）并向其授予特定权限。 向导将为群集本身创建计算机帐户（此帐户也称为群集名称对象或 CNO）和计算机帐户，适用于大多数类型的群集服务和应用程序，这是 Hyper-v 虚拟机例外。 故障转移群集向导会自动设置这些帐户的权限。 如果更改了权限，则需要将其更改回以匹配群集要求。 本指南介绍了这些 Active Directory 帐户和权限，提供了有关其重要性的原因，并介绍了用于配置和管理帐户的步骤。
      

## <a name="overview-of-active-directory-accounts-needed-by-a-failover-cluster"></a>故障转移群集所需 Active Directory 帐户的概述

本部分介绍对故障转移群集非常重要的 Active Directory 计算机帐户（也称为 Active Directory 计算机对象）。 这些帐户如下所示：

  - **用于创建群集的用户帐户。** 这是用于启动创建群集向导的用户帐户。 此帐户很重要，因为它提供了为群集本身创建计算机帐户的基础。  
      
  - **群集名称帐户。** （群集自身的计算机帐户，也称为群集名称对象或 CNO）。 此帐户由创建群集向导自动创建，并与群集同名。 群集名称帐户非常重要，因为通过此帐户，在群集上配置新服务和应用程序时，会自动创建其他帐户。 如果删除了群集名称帐户或从中删除了权限，则在还原群集名称帐户或恢复正确的权限之前，不能根据群集的要求创建其他帐户。  
      
    例如，如果创建名为 Cluster1 的群集，然后尝试在群集上配置名为 PrintServer1 的群集打印服务器，Active Directory 中的 Cluster1 帐户需要保留正确的权限，以便可以使用它来创建计算机名为 PrintServer1 的帐户。  
      
    在 Active Directory 中的计算机帐户的默认容器中创建群集名称帐户。 默认情况下，这是 "计算机" 容器，但域管理员可以选择将其重定向到另一个容器或组织单位（OU）。  
      
  - **群集服务或应用程序的计算机帐户（计算机对象）。** 高可用性向导会在创建大多数类型的群集服务或应用程序的过程中自动创建这些帐户，这是一个 Hyper-v 虚拟机。 群集名称帐户被授予控制这些帐户所需的权限。  
      
    例如，如果你有一个名为 Cluster1 的群集，然后创建一个名为 FileServer1 的群集文件服务器，则高可用性向导会创建一个名为 FileServer1 的 Active Directory 计算机帐户。 高可用性向导还为 Cluster1 帐户提供控制 FileServer1 帐户所需的权限。  
      

下表描述了这些帐户所需的权限。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>帐户</th>
<th>有关权限的详细信息</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>用于创建群集的帐户</p></td>
<td><p>需要将成为群集节点的服务器上的管理权限。 还需要在用于域中的计算机帐户的容器中<strong>创建计算机对象</strong>和<strong>读取所有属性</strong>权限。</p></td>
</tr>
<tr class="even">
<td><p>群集名称帐户（群集自身的计算机帐户）</p></td>
<td><p>在运行创建群集向导时，它会在默认容器中创建群集名称帐户，该帐户用于域中的计算机帐户。 默认情况下，群集名称帐户（如其他计算机帐户）最多可以在域中创建10个计算机帐户。</p>
<p>如果在创建群集之前创建群集名称帐户（群集名称对象）（即，预安排帐户），则必须在用于计算机的容器中为其提供 "<strong>创建计算机对象</strong>" 和 "<strong>读取所有属性</strong>" 权限域中的帐户。 您还必须禁用该帐户，并对安装该群集的管理员将使用的帐户进行<strong>完全控制</strong>。 有关详细信息，请参阅本指南后面的预<a href="#steps-for-prestaging-the-cluster-name-account" data-raw-source="[Steps for prestaging the cluster name account](#steps-for-prestaging-the-cluster-name-account)">安排群集名称帐户的步骤</a>。</p></td>
</tr>
<tr class="odd">
<td><p>群集服务或应用程序的计算机帐户</p></td>
<td><p>运行高可用性向导时（创建新的群集服务或应用程序），在大多数情况下，将在 Active Directory 中创建群集服务或应用程序的计算机帐户。 为群集名称帐户授予了控制此帐户所需的权限。 群集 Hyper-v 虚拟机例外：没有为此创建计算机帐户。</p>
<p>如果为群集服务或应用程序预留了计算机帐户，则必须使用所需的权限对其进行配置。 有关详细信息，请参阅本指南后面的<a href="#steps-for-prestaging-an-account-for-a-clustered-service-or-application" data-raw-source="[Steps for prestaging an account for a clustered service or application](#steps-for-prestaging-an-account-for-a-clustered-service-or-application)">为群集服务或应用程序预安排帐户的步骤</a>。</p></td>
</tr>
</tbody>
</table>


> [!NOTE]
> 在早期版本的 Windows Server 中，存在群集服务的帐户。 不过，从 Windows Server 2008 开始，群集服务会自动在特定的上下文中运行，该上下文提供了服务所需的特定权限和特权（与本地系统上下文相似，但权限减少）。 但是，还需要其他帐户，如本指南中所述。 
<br>


### <a name="how-accounts-are-created-through-wizards-in-failover-clustering"></a>如何通过故障转移群集中的向导创建帐户

下图说明了如何使用和创建上一小节中描述的计算机帐户（Active Directory 对象）。 当管理员运行创建群集向导，然后运行高可用性向导（以便配置群集服务或应用程序）时，这些帐户将开始运行。

![](media/configure-ad-accounts/Cc731002.e8a7686c-9ba8-4ddf-87b1-175b7b51f65d(WS.10).gif)

请注意，上面的关系图显示了运行创建群集向导和高可用性向导的单个管理员。 但是，如果两个帐户都具有足够的权限，则这可能是两个不同的管理员，它们使用两个不同的用户帐户。 本指南后面的与故障转移群集、Active Directory 域和帐户相关的要求中更详细地介绍了权限。

### <a name="how-problems-can-result-if-accounts-needed-by-the-cluster-are-changed"></a>群集所需的帐户发生更改时可能产生的问题

下图说明了在创建群集向导自动创建群集名称帐户（群集所需的帐户）后，是否会产生问题。

![](media/configure-ad-accounts/Cc731002.beecc4f7-049c-4945-8fad-2cceafd6a4a5(WS.10).gif)

如果出现在关系图中显示的问题类型，则将在事件查看器中记录某个事件（1193、1194、1206或1207）。 有关这些事件的详细信息，请[http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)参阅。

请注意，如果已达到用于创建计算机对象（默认为10）的全域配额，则创建群集服务或应用程序的帐户时，可能会出现类似的问题。 如果是这样，则可能需要咨询域管理员以增加配额，尽管这是一个域范围的设置，只应在仔细考虑后进行更改，并且仅在确认前面的关系图不描述您的情况。 有关详细信息，请参阅本指南后面的[解决与群集相关的 Active Directory 帐户中的更改导致的问题的步骤](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)。

## <a name="requirements-related-to-failover-clusters-active-directory-domains-and-accounts"></a>与故障转移群集、Active Directory 域和帐户相关的要求

如前面三部分所述，必须先满足某些要求，然后才能在故障转移群集上成功配置群集服务和应用程序。 最基本的要求涉及群集节点的位置（在单个域中）以及安装群集的人员的帐户的权限级别。 如果满足这些要求，则故障转移群集向导会自动创建群集所需的其他帐户。 以下列表提供了有关这些基本要求的详细信息。

  - **结点**所有节点必须位于同一个 Active Directory 域中。 （域不能基于不包含 Active Directory 的 Windows NT 4.0。）  
      
  - **安装群集的人员的帐户：** 安装群集的人员必须使用具有以下特征的帐户：  
      
      - 此帐户必须是域帐户。 它不一定是域管理员帐户。 如果它满足此列表中的其他要求，它可以是域用户帐户。  
          
      - 此帐户必须在将成为群集节点的服务器上具有管理权限。 提供此项的最简单方法是创建一个域用户帐户，然后将该帐户添加到将成为群集节点的每个服务器上的本地管理员组中。 有关详细信息，请参阅本指南后面的[为安装群集的人员配置帐户的步骤](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)。  
          
      - 必须为该帐户（或该帐户所在的组）提供容器中的 "**创建计算机对象**" 和 "**读取全部属性**" 权限，该容器用于域中的计算机帐户。 有关详细信息，请参阅本指南后面的[为安装群集的人员配置帐户的步骤](#steps-for-configuring-the-account-for-the-person-who-installs-the-cluster)。  
          
      - 如果你的组织选择预留群集名称帐户（一个与群集同名的计算机帐户），则预留的群集名称帐户必须向安装群集的人员的帐户授予 "完全控制" 权限。 有关如何预留群集名称帐户的其他重要详细信息，请参阅本指南后面的预[安排群集名称帐户的步骤](#steps-for-prestaging-the-cluster-name-account)。  
          

### <a name="planning-ahead-for-password-resets-and-other-account-maintenance"></a>预先规划密码重置和其他帐户维护

故障转移群集的管理员有时可能需要重置群集名称帐户的密码。 此操作需要特定权限、**重置密码**权限。 因此，最佳做法是编辑群集名称帐户的权限（通过使用 "Active Directory 用户和计算机" 管理单元）为群集的管理员提供群集名称帐户的 "**重置密码**" 权限。 有关详细信息，请参阅本指南后面的[针对群集名称帐户排查密码问题的步骤](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)。

## <a name="steps-for-configuring-the-account-for-the-person-who-installs-the-cluster"></a>为安装群集的人员配置帐户的步骤

安装群集的人员的帐户非常重要，因为它提供了为群集本身创建计算机帐户的基础。

完成下列过程所需的最低组成员身份取决于你是要创建域帐户并向其分配域中所需的权限，还是仅将帐户（由其他人创建）放入将作为故障转移群集中的节点的服务器上的本地**管理员**组。 如果以前的成员身份**操作员**或等效成员身份是完成此过程的最低要求。 如果后者为故障转移群集中作为节点的服务器上的本地**管理员**组中的成员身份或同等身份，则这一切都是必需的。 有关使用适当帐户和组成员身份[http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)的详细信息，请参阅。

#### <a name="to-configure-the-account-for-the-person-who-installs-the-cluster"></a>为安装群集的人员配置帐户

1.  为安装群集的人员创建或获取一个域帐户。 此帐户可以是域用户帐户，也可以是**帐户操作员**帐户。 如果你使用标准用户帐户，则必须在此过程中向其授予一些额外的权限。

2.  如果在步骤1中创建或获取的帐户不会自动包含在域中计算机上的本地**管理员**组中，请将该帐户添加到将成为故障转移中的节点的服务器上的本地**管理员**组中。聚集
    
    1.  依次单击“开始”、“管理工具”和“服务器管理器”。  
          
    2.  在控制台树中，展开 "**配置**"，展开 "**本地用户和组**"，然后展开 "**组**"。  
          
    3.  在中心窗格中，右键单击 "**管理员**"，单击 "**添加到组**"，然后单击 "**添加**"。  
          
    4.  在 "**输入要选择的对象名称**" 下，键入在步骤1中创建或获取的用户帐户的名称。 如果出现提示，请输入对此操作具有足够权限的帐户名和密码。 然后单击“确定”。  
          
    5.  在将成为故障转移群集中的节点的每个服务器上重复这些步骤。  

> [!IMPORTANT]
> 必须在将成为群集中的节点的所有服务器上重复这些步骤。 
<br>


3. 如果在步骤1中创建或获取的帐户是域管理员帐户，请跳过此过程的其余部分。 否则，为域中的计算机帐户授予帐户 "**创建计算机对象**" 和 "**读取所有属性**" 权限：
    
   1.  在域控制器上，依次单击 "**开始**"、"**管理工具**"，然后单击 " **Active Directory 用户和计算机**"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。  
          
   2.  在 "**视图**" 菜单上，确保已选择 "**高级功能**"。  
          
       选择 "**高级功能**" 时，可以在**Active Directory 用户和计算机**"中的" 帐户（对象） "的属性中查看"**安全**"选项卡。  
          
   3.  右键单击在域中创建计算机帐户的 "默认**计算机**" 容器或默认容器，然后单击 "**属性**"。 **计算机**位于<b>Active Directory 用户和计算机/</b><i>域节点</i><b>/Computers</b>中。  
          
   4.  在“安全” 选项卡中，单击“高级”。  
          
   5.  单击 "**添加**"，键入在步骤1中创建或获取的帐户的名称，然后单击 **"确定"** 。  
          
   6.  在 "*用于 * * * 的权限条目*" 对话框中，找到 "**创建计算机对象**" 和 "**读取全部属性**" 权限，并确保选中每个选项的 "**允许**" 复选框。  
          
       ![](media/configure-ad-accounts/Cc731002.0a863ac5-2024-4f9f-8a4d-a419aff32fa0(WS.10).gif)

## <a name="steps-for-prestaging-the-cluster-name-account"></a>用于预安排群集名称帐户的步骤

通常，如果不预留群集名称帐户，而是在运行创建群集向导时允许自动创建和配置帐户，则通常会更简单。 但是，如果由于组织中的要求而需要预留群集名称帐户，请使用以下过程。

**Domain Admins** 组中的成员身份或同等身份是完成此过程所需的最低要求。 有关使用适当帐户和组成员身份[http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)的详细信息，请参阅。 请注意，你可以对此过程使用同一帐户，因为你将在创建群集时使用。

#### <a name="to-prestage-a-cluster-name-account"></a>预留群集名称帐户

1.  请确保知道群集将具有的名称，以及创建群集的用户将使用的用户帐户的名称。 （请注意，你可以使用该帐户来执行此过程。）

2.  在域控制器上，依次单击 "**开始**"、"**管理工具**"，然后单击 " **Active Directory 用户和计算机**"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。

3.  在控制台树中，右键单击 "**计算机**" 或在域中创建计算机帐户的默认容器。 **计算机**位于<b>Active Directory 用户和计算机/</b><i>域节点</i><b>/Computers</b>中。

4.  单击 "**新建**"，然后单击 "**计算机**"。

5.  键入将用于故障转移群集的名称，换句话说，将在创建群集向导中指定的群集名称，然后单击 **"确定"** 。

6.  右键单击刚创建的帐户，然后单击 "**禁用帐户**"。 如果系统提示你确认你的选择，请单击 **"是"** 。
    
    此帐户必须处于禁用状态，以便在运行**创建群集**向导时，它可以确认域中的现有计算机或群集当前是否正在使用该帐户将用于群集的帐户。

7.  在 "**视图**" 菜单上，确保已选择 "**高级功能**"。
    
    选择 "**高级功能**" 时，可以在**Active Directory 用户和计算机**"中的" 帐户（对象） "的属性中查看"**安全**"选项卡。

8.  右键单击在步骤3中右键单击的文件夹，然后单击 "**属性**"。

9.  在“安全” 选项卡中，单击“高级”。

10. 单击 "**添加**"，单击 "**对象类型**"，确保已选择 "**计算机**"，然后单击 **"确定"** 。 然后，在 "**输入要选择的对象名称**" 下，键入刚才创建的计算机帐户的名称，然后单击 **"确定"** 。 如果出现一条消息，指出您要添加一个已禁用的对象，请单击 **"确定"** 。

11. 在 "**权限项目**" 对话框中，找到 "**创建计算机对象**和**读取所有属性**" 权限，并确保为每个对象选中 "**允许**" 复选框。
    
    ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

12. 单击 **"确定"** ，直到返回到 " **Active Directory 用户和计算机**" 管理单元。

13. 如果你使用的帐户与用于创建群集的过程相同，则跳过其余步骤。 否则，你必须配置权限，以便用于创建群集的用户帐户对你刚创建的计算机帐户具有完全控制权限：
    
    1.  在 "**视图**" 菜单上，确保已选择 "**高级功能**"。  
          
    2.  右键单击刚创建的计算机帐户，然后单击 "**属性**"。  
          
    3.  在“安全”选项卡上，单击“添加”。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。  
          
    4.  使用 "**选择用户、计算机或组**" 对话框指定创建群集时将使用的用户帐户。 然后单击“确定”。  
          
    5.  请确保已选择刚添加的用户帐户，然后在 "**完全控制**" 旁边选中 "**允许**" 复选框。  
          
        ![](media/configure-ad-accounts/Cc731002.fffaafe2-a494-498b-974c-8f9d70f7103b(WS.10).gif)

## <a name="steps-for-prestaging-an-account-for-a-clustered-service-or-application"></a>为群集服务或应用程序预安排帐户的步骤

如果不为群集服务或应用程序预留计算机帐户，而是允许在运行高可用性向导时自动创建和配置帐户，则通常会更简单。 但是，如果出于组织的要求而需要预先安排帐户，请使用以下过程。

**Account Operators**组中的成员身份或等效身份是完成此过程所需的最低要求。 有关使用适当帐户和组成员身份[http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)的详细信息，请参阅。

#### <a name="to-prestage-an-account-for-a-clustered-service-or-application"></a>为群集服务或应用程序预留帐户

1.  确保你知道群集的名称，以及群集服务或应用程序将具有的名称。

2.  在域控制器上，依次单击 "**开始**"、"**管理工具**"，然后单击 " **Active Directory 用户和计算机**"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。

3.  在控制台树中，右键单击 "**计算机**" 或在域中创建计算机帐户的默认容器。 **计算机**位于<b>Active Directory 用户和计算机/</b><i>域节点</i><b>/Computers</b>中。

4.  单击 "**新建**"，然后单击 "**计算机**"。

5.  键入将用于群集服务或应用程序的名称，然后单击 **"确定"** 。

6.  在 "**视图**" 菜单上，确保已选择 "**高级功能**"。
    
    选择 "**高级功能**" 时，可以在**Active Directory 用户和计算机**"中的" 帐户（对象） "的属性中查看"**安全**"选项卡。

7.  右键单击刚创建的计算机帐户，然后单击 "**属性**"。

8.  在“安全”选项卡上，单击“添加”。

9.  单击 "**对象类型**"，确保已选择 "**计算机**"，然后单击 **"确定"** 。 然后，在 "**输入要选择的对象名称**" 下，键入群集名称帐户，然后单击 **"确定"** 。 如果出现一条消息，指出您要添加一个已禁用的对象，请单击 **"确定"** 。

10. 确保选中 "群集名称" 帐户，然后选择 "**完全控制**" 旁边的 "**允许**" 复选框。
    
    ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

## <a name="steps-for-troubleshooting-problems-related-to-accounts-used-by-the-cluster"></a>解决与群集使用的帐户相关的问题的步骤

如本指南前面所述，当创建故障转移群集并配置群集服务或应用程序时，故障转移群集向导会创建必要的 Active Directory 帐户，并为其授予正确的权限。 如果删除了所需的帐户，或更改了必需的权限，则可能会导致问题。 以下小节提供了解决这些问题的步骤。

  - [排查群集名称帐户的密码问题的步骤](#steps-for-troubleshooting-password-problems-with-the-cluster-name-account)
      
  - [排查群集相关 Active Directory 帐户中的更改导致的问题的步骤](#steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts)
      

### <a name="steps-for-troubleshooting-password-problems-with-the-cluster-name-account"></a>排查群集名称帐户的密码问题的步骤

如果有关于计算机对象或包含以下文本的群集标识的事件消息，请使用此过程。 请注意，此文本将位于事件消息中，而不是位于事件消息的开头：

`Logon failure: unknown user name or bad password.`

符合前面说明的事件消息指示群集名称帐户的密码和群集软件存储的相应密码不再匹配。

有关确保群集管理员具有根据需要执行以下过程的正确权限的信息，请参阅本指南前面的为密码重置和其他帐户维护提前计划。

必须至少具有本地 **Administrators** 组中的成员身份或同等身份才能完成此过程。 此外，必须为你的帐户授予群集名称帐户的 "**重置密码**" 权限（除非你的帐户是**域管理员**帐户，或者是群集名称帐户的创建者所有者）。 安装群集的人员使用的帐户可用于此过程。 有关使用适当帐户和组成员身份[http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)的详细信息，请参阅。

#### <a name="to-troubleshoot-password-problems-with-the-cluster-name-account"></a>排查群集名称帐户的密码问题

1.  若要打开故障转移群集管理单元，请依次单击“开始”、“管理工具”，然后单击“故障转移群集管理”。 （如果出现 "**用户帐户控制**" 对话框，请确认它显示的是所需操作，然后单击 "**继续**"。）

2.  在故障转移群集管理单元中，如果未显示出要配置的群集，则在控制台树中，右键单击“故障转移群集管理”、单击“管理群集”，然后选择或指定所需群集。

3.  在中心窗格中，展开 "**群集核心资源**"。

4.  在 "**群集名称**" 下，右键单击**名称**项，指向 "**更多操作**"，然后单击 "**修复 Active Directory 对象**"。

### <a name="steps-for-troubleshooting-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>排查群集相关 Active Directory 帐户中的更改导致的问题的步骤

如果删除群集名称帐户或者从该帐户中删除权限，则当你尝试配置新的群集服务或应用程序时，将会出现问题。 若要解决导致此问题的问题，请使用 Active Directory 用户和计算机 "管理单元来查看或更改群集名称帐户以及其他相关帐户。 有关发生此类问题（事件1193、1194、1206或1207）时记录的事件的信息，请参阅[http://go.microsoft.com/fwlink/?LinkId=118271](http://go.microsoft.com/fwlink/?linkid=118271)。

**Domain Admins** 组中的成员身份或同等身份是完成此过程所需的最低要求。 有关使用适当帐户和组成员身份[http://go.microsoft.com/fwlink/?LinkId=83477](http://go.microsoft.com/fwlink/?linkid=83477)的详细信息，请参阅。

#### <a name="to-troubleshoot-problems-caused-by-changes-in-cluster-related-active-directory-accounts"></a>解决由群集相关的 Active Directory 帐户中的更改导致的问题

1.  在域控制器上，依次单击 "**开始**"、"**管理工具**"，然后单击 " **Active Directory 用户和计算机**"。 如果出现“用户帐户控制”对话框，请确认它显示的是所需操作，然后单击“继续”。

2.  展开 "默认**计算机**" 容器或群集名称帐户（群集的计算机帐户）所在的文件夹。 **计算机**位于<b>Active Directory 用户和计算机/</b><i>域节点</i><b>/Computers</b>中。

3.  检查群集名称帐户的图标。 它在其上不能有向下箭头，即不能禁用该帐户。 如果显示为 "已禁用"，请右键单击该命令，然后查找命令 "**启用帐户**"。 如果显示该命令，请单击该命令。

4.  在 "**视图**" 菜单上，确保已选择 "**高级功能**"。
    
    选择 "**高级功能**" 时，可以在**Active Directory 用户和计算机**"中的" 帐户（对象） "的属性中查看"**安全**"选项卡。

5.  右键单击 "默认**计算机**" 容器或群集名称帐户所在的文件夹。

6.  单击“属性”。

7.  在“安全” 选项卡中，单击“高级”。

8.  在具有权限的帐户列表中，单击群集名称帐户，然后单击 "**编辑**"。
    

> [!NOTE]
> 如果未列出群集名称帐户，请单击 "<STRONG>添加</STRONG>" 并将其添加到列表。 
<br>


9. 对于群集名称帐户（也称为群集名称对象或 CNO），请确保为 "**创建计算机对象**和**读取所有属性**" 权限选择 "**允许**"。
    
   ![](media/configure-ad-accounts/Cc731002.f5977c4d-a62e-4b17-81e3-8c19ddca2078(WS.10).gif)

10. 单击 **"确定"** ，直到返回到 " **Active Directory 用户和计算机**" 管理单元。

11. 查看与创建计算机帐户（对象）相关的域策略（咨询域管理员，如果适用）。 确保每次配置群集服务或应用程序时，群集名称帐户都可以创建计算机帐户。 例如，如果你的域管理员已配置了设置，这些设置会导致在专用容器而不是默认**计算机**容器中创建所有新计算机帐户，请确保这些设置允许群集名称帐户同时在该容器中创建新的计算机帐户。

12. 展开 "默认**计算机**" 容器或其中一个群集服务或应用程序所在的计算机帐户所在的容器。

13. 右键单击其中一个群集服务或应用程序的计算机帐户，然后单击 "**属性**"。

14. 在 "**安全**" 选项卡上，确认群集名称帐户在具有权限的帐户中列出，然后选择它。 确认群集名称帐户具有 "**完全控制**" 权限（已选中 "**允许**" 复选框）。 如果没有，请将群集名称帐户添加到列表中，并向其授予 "**完全控制**" 权限。
    
   ![](media/configure-ad-accounts/Cc731002.2e614376-87a6-453a-81ba-90ff7ebc3fa2(WS.10).gif)

15. 对于群集中配置的每个群集服务和应用程序，重复步骤13-14。

16. 检查是否未到达用于创建计算机对象（默认情况下为10）的全域配额（如果适用，请咨询域管理员）。 如果已查看并更正了此过程中的前一项，并已达到配额，请考虑增加配额。 更改配额：
    
   1.  以管理员身份打开命令提示符，然后运行**ADSIEdit**。  
          
   2.  右键单击**ADSI 编辑器**，单击 "**连接到**"，然后单击 **"确定"** 。 **默认的命名上下文**将添加到控制台树中。  
          
   3.  双击 "**默认命名上下文**"，右键单击其下的域对象，然后单击 "**属性**"。  
          
   4.  滚动到**MachineAccountQuota**，选择它，单击 "**编辑**"，更改值，然后单击 **"确定"** 。


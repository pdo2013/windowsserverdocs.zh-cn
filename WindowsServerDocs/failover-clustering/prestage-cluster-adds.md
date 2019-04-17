---
title: 预备群集在 Active Directory 域服务的计算机对象
description: 如何预备群集在 Active Directory 域服务的计算机对象。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/25/2018
ms.localizationpriority: medium
ms.openlocfilehash: 111969b074b33764dbbf72bfb24ad606f8314e41
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081878"
---
# <a name="prestage-cluster-computer-objects-in-active-directory-domain-services"></a>预备群集在 Active Directory 域服务的计算机对象

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主题演示如何预先设置 Active Directory 域服务 (AD DS) 中的群集计算机对象。 您可以使用此过程启用的用户或组，它们没有权限在 AD DS 中创建计算机对象时创建的故障转移群集。

当您使用创建群集向导或通过使用 Windows PowerShell 创建故障转移群集时，必须指定群集的名称。 如果您有足够的权限创建群集时，群集创建过程中会自动在群集名称相匹配的 AD DS 中创建计算机对象。 此对象调用 CNO 的*群集名称对象*。 通过 CNO，配置使用客户端访问点的群集的角色时自动创建虚拟计算机对象 (Vco)。 例如，如果您使用名为*FileServer1*客户端访问点创建高度可用的文件服务器，则 CNO 将在 AD DS 中创建相应 VCO。

>[!NOTE]
>在 Windows Server 2012 R2 中，没有创建一个 Active Directory 分离的群集，其中在 AD DS 中创建没有 CNO 或 Vco 的选项。 这被面向于特定类型的群集部署。 有关详细信息，请参阅[Deploy Active Directory-Detached 群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v%3dws.11)>)。

自动创建 CNO，创建故障转移群集的用户必须具有对组织单位 (OU) 或要组成群集的服务器所在的容器**创建计算机对象**权限。 若要启用的用户或组，而无此权限创建群集，具有 AD DS （通常是域管理员） 中的相应权限的用户可以预备 AD DS 中的 CNO。 这还提供了域管理员，通过哪些 OU 群集对象中创建更多控制用于群集的命名约定和控件。

## <a name="step-1-prestage-the-cno-in-ad-ds"></a>步骤 1： 预备 AD DS 中 CNO

在开始之前，请确保您知道以下：

- 要分配群集名称
- 要向其授予权限来创建群集组的用户帐户的名称

作为最佳实践，我们建议您创建的群集对象的 OU。 如果在对 OU 已存在您想要使用中**Account Operators**组, 的成员身份是完成此步骤所需的最低。 如果您需要为群集对象创建一个 OU， **Domain Admins**组或等效成员资格是完成此步骤所需的最低。

>[!NOTE]
>如果您在默认计算机容器而不是在对 OU 中创建了 CNO，您不必完成本主题的步骤 3。 在此方案中，群集管理员可以创建最多 10 个 Vco 无需任何其他配置。

### <a name="prestage-the-cno-in-ad-ds"></a>预备 AD DS 中 CNO

1. 在已从远程服务器管理工具，安装的 AD DS 工具的计算机上或域控制器上，打开**Active Directory 用户和计算机**。 为此服务器上，启动服务器管理器，然后在**工具**菜单中，选择**Active Directory 用户和计算机**。
2. 若要创建群集 OU 计算机对象，右键单击域名或某个现有的 OU，指向**新建**，，然后选择**组织单位**。 在**名称**框中，输入 OU 的名称，然后选择**确定**。
3. 在控制台树中，右键单击您想要创建 CNO，指向**新建**，然后选择**计算机**组织的单位。
4. 在**计算机名称**框中，输入名称，用于故障转移群集，然后选择**确定**。

  >[!NOTE]
  >这是创建群集的用户将在**用于群集管理访问点**页中创建群集向导，或为的值指定的群集名称 *– 名称***新建群集**Windows PowerShell 参数cmdlet。

5. 作为最佳实践，右键单击您刚才创建的计算机帐户，选择**属性**，然后选择**对象**选项卡。**对象**选项卡中，选择**被意外删除的保护对象**复选框，然后选择**确定**。
6. 右键单击刚创建的计算机帐户，然后选择**禁用帐户**。 选择**是**以确认，然后选择**确定**。

  >[!NOTE]
  >以便在群集创建过程中，群集创建过程中可以确认的帐户不是当前使用的现有计算机或域中的群集，必须禁用帐户。

![在示例群集 OU 禁用的 CNO](media/prestage-cluster-adds/disabled-cno-in-the-example-clusters-ou.png)

**图 1. 在示例群集 OU 禁用的 CNO**

## <a name="step-2-grant-the-user-permissions-to-create-the-cluster"></a>步骤 2： 授予用户权限来创建群集

您必须配置权限，以便将用于创建故障转移群集的用户帐户具有对 CNO 完全控制权限。

**Account Operators**组中的成员资格是完成此步骤所需的最低。

下面是如何授予用户权限来创建群集：

1. 在 Active Directory 用户和计算机**视图**菜单中，确保已选中**高级功能**。
2. 找到，然后右键单击 CNO，，然后选择**属性**。
3. 在**安全**选项卡中，选择**添加**。
4. 在**选择用户、 计算机或组**对话框中指定的用户帐户或组，您想要授予权限，，然后选择**确定**。
5. 选择的用户帐户或刚刚添加的组，然后选择**完全控制**，旁边的**允许**复选框。
  
  ![向用户或组将创建群集授予完全控制](media/prestage-cluster-adds/granting-full-control-to-the-user-create-the-cluster.png)
  
  **图 2. 向用户或组将创建群集授予完全控制**
6. 选择**确定**。

完成此步骤后，您授予的访问权限的用户可以创建故障转移群集。 但是，如果 OU 中位于 CNO，则用户无法创建需要客户端访问点，直到您完成 Step 3 的群集的角色。

>[!NOTE]
>如果 CNO 位于默认计算机容器中，群集管理员可以创建最多 10 个 Vco 无需任何其他配置。 若要添加多个 10 Vco，您必须明确**创建计算机对象**权限授予计算机容器 CNO。

## <a name="step-3-grant-the-cno-permissions-to-the-ou-or-prestage-vcos-for-clustered-roles"></a>步骤 3: CNO 权限授予 OU 或预 Vco 安排群集的角色

与客户端访问点创建群集的角色时，群集中相同的 OU CNO 创建 VCO。 为此自动进行，CNO 必须有权创建的 OU 中的计算机对象。

如果预备 AD DS 中的 CNO，您可以执行以下命令以创建 Vco 之一：

- 选项 1：[授予对 OU 的 CNO 权限](#grant-the-cno-permissions-to-the-ou)。 如果您使用此选项，群集在 AD DS 中可以自动创建 Vco。 因此，故障转移群集的管理员可以而无需请求您预备 Vco AD DS 中的创建群集的角色。

>[!NOTE]
>**Domain Admins**组或等效成员资格是完成此选项的步骤所需的最低。

- 选项 2: [Prestage VCO 群集的角色](#prestage-a-vco-for-the-clustered-role)。 如有必要，因为您的组织中的要求预备群集角色的帐户，请使用此选项。 例如，您可能希望控制的命名约定或创建的群集的角色的控件。

>[!NOTE]
>**Account Operators**组中的成员身份是完成此选项的步骤所需的最低。

### <a name="grant-the-cno-permissions-to-the-ou"></a>向 OU 授予 CNO 权限

1. 在 Active Directory 用户和计算机**视图**菜单中，确保已选中**高级功能**。
2. 右键单击您在其中创建 CNO 中的 OU[步骤 1： 预备 AD DS 中的 CNO](#step-1:-prestage-the-CNO-in-ad-ds)，然后选择**属性**。
3. 在**安全**选项卡中，选择**高级**。
4. 在**高级安全设置**对话框中，选择**添加**。
5. 旁边**主体**，选择**选择主体**。
6. 在**选择用户、 计算机、 服务帐户或组**对话框，选择**对象类型**和选择**计算机**复选框，然后选择**确定**。
7. 在**输入要选择的对象名称**输入 CNO 的名称和选择**检查名称**，然后选择**确定**。 在警告消息，指出的响应，您是要添加一个禁用的对象，请选择**确定**。
8. 在**权限项目**对话框中，确保**类型**列表设置为**允许**和**适用范围**列表设置为**此对象和所有后代对象**。
9. 在**权限**中，选择**创建计算机对象**复选框。

  ![授予给 CNO 创建计算机对象权限](media/prestage-cluster-adds/granting-create-computer-objects-permission-to-the-cno.png)

  **图 3. 授予给 CNO 创建计算机对象权限**
10. 选择**确定**，直到您返回到 Active Directory 用户和计算机管理单元中。

现在，故障转移群集上的管理员可以使用客户端访问点创建群集的角色，并使资源联机。

### <a name="prestage-a-vco-for-a-clustered-role"></a>预备群集角色 VCO

1. 在开始之前，请确保您知道群集的名称和群集的角色将具有的名称。
2. 在 Active Directory 用户和计算机**视图**菜单中，确保已选中**高级功能**。
3. 在 Active Directory 用户和计算机中，右键单击群集 CNO 所在的 OU，指向**新建**，，然后选择**计算机**。
4. 在**计算机名称**框中，输入名称，您将用于群集的角色，然后选择**确定**。
5. 作为最佳实践，右键单击您刚才创建的计算机帐户，选择**属性**，然后选择**对象**选项卡。**对象**选项卡中，选择**被意外删除的保护对象**复选框，然后选择**确定**。
6. 右键单击刚创建的计算机帐户，然后选择**属性**。
7. 在**安全**选项卡中，选择**添加**。
8. 在**选择用户、 计算机、 服务帐户或组**对话框，选择**对象类型**和选择**计算机**复选框，然后选择**确定**。
9. 在**输入要选择的对象名称**输入 CNO 的名称和选择**检查名称**，然后选择**确定**。 如果您收到一条警告消息，指出您正准备添加一个禁用的对象，选择**确定**。
10. 请确保选择了 CNO，，然后选择**完全控制**，旁边的**允许**复选框。
11. 选择**确定**。

现在，故障转移群集上的管理员可以创建与客户端访问点的预备的 VCO 名称相匹配的群集的角色，并使资源联机。

## <a name="more-information"></a>详细信息

- [故障转移群集](failover-clustering.md)
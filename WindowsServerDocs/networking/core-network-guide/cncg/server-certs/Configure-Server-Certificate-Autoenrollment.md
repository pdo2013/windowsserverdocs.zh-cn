---
title: 配置服务器证书自动注册
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea7b7efe01525f4ecfb35200463c3f221d92d62d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888558"
---
# <a name="configure-certificate-auto-enrollment"></a>配置证书自动注册

>适用于：Windows 服务器 （半年频道），Windows Server 2016

> [!NOTE]
> 在执行此过程之前，必须在运行 AD CS CA 使用证书模板 Microsoft 管理控制台管理单元中来配置服务器证书模板。
在这种成员资格**Enterprise Admins**和根域**Domain Admins**组是完成此过程所需的最低。

## <a name="configure-server-certificate-auto-enrollment"></a>配置服务器证书自动注册

1. 在计算机上安装 AD DS，打开 Windows PowerShell&reg;，类型**mmc**，然后按 ENTER。 将打开 Microsoft 管理控制台。
2. 在“文件”  菜单上，单击“添加/删除管理单元” 。 **添加或删除管理单元**对话框随即打开。
3. 在中**可用的管理单元**，向下滚动到并双击**组策略管理编辑器**。 **选择组策略对象**对话框随即打开。

     > [!IMPORTANT]
     > 确保选择**组策略管理编辑器**而不**组策略管理**。 如果选择**组策略管理**，使用这些说明配置将失败，服务器证书不会自动注册到你 NPSs。

4. 在中**组策略对象**，单击**浏览**。 **浏览组策略对象**对话框随即打开。
5. 在中**域、 Ou 和链接的组策略对象**单击**默认域策略**，然后单击**确定**。
6. 单击“完成” ，然后单击“确定” 。
7. 双击**默认域策略**。 在控制台中，展开以下路径：**计算机配置**，**策略**， **Windows 设置**，**安全设置**，然后**公钥策略**.
8. 单击**公钥策略**。 在细节窗格中，双击 **“证书服务客户端 - 自动注册”**。 **属性**对话框随即打开。 配置以下各项，然后单击**确定**:

     1. 在 **“配置模型”** 中，选择 **“已启用”**。
     2. 选择**续订过期证书、 更新未决证书并删除吊销的证书**复选框。
     3. 选中**更新使用证书模板的证书**复选框。

9. 单击 **“确定”**。

## <a name="configure-user-certificate-auto-enrollment"></a>配置用户证书自动注册

1. 在计算机上安装 AD DS，打开 Windows PowerShell&reg;，类型**mmc**，然后按 ENTER。 将打开 Microsoft 管理控制台。
2. 在“文件”  菜单上，单击“添加/删除管理单元” 。 **添加或删除管理单元**对话框随即打开。
3. 在中**可用的管理单元**，向下滚动到并双击**组策略管理编辑器**。 **选择组策略对象**对话框随即打开。

     > [!IMPORTANT]
     > 确保选择**组策略管理编辑器**而不**组策略管理**。 如果选择**组策略管理**，使用这些说明配置将失败，服务器证书不会自动注册到你 NPSs。

4. 在中**组策略对象**，单击**浏览**。 **浏览组策略对象**对话框随即打开。
5. 在中**域、 Ou 和链接的组策略对象**单击**默认域策略**，然后单击**确定**。
6. 单击“完成” ，然后单击“确定” 。
7. 双击**默认域策略**。 在控制台中，展开以下路径：**用户配置**，**策略**， **Windows 设置**，**安全设置**。
8. 单击**公钥策略**。 在细节窗格中，双击 **“证书服务客户端 - 自动注册”**。 **属性**对话框随即打开。 配置以下各项，然后单击**确定**:

     1. 在 **“配置模型”** 中，选择 **“已启用”**。
     2. 选择**续订过期证书、 更新未决证书并删除吊销的证书**复选框。
     3. 选中**更新使用证书模板的证书**复选框。

9. 单击 **“确定”**。

## <a name="next-steps"></a>后续步骤

[刷新组策略](refresh-group-policy.md)

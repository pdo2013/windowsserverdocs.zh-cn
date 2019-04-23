---
title: AD 林恢复-引发 RID 池
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862978"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>AD 林恢复-引发可用 RID 池的值 

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用以下过程引发的值的相对 ID (RID) 池还原该 DC 后，将分配 RID 操作主机。 通过提高可用 RID 池的值，可以确保没有 DC 为用于还原域备份后创建的安全主体分配 RID。 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>有关 Active Directory RID 池和 rIDAvailablePool

每个域都有一个对象**CN = RID Manager$，CN = System，DC**=<*domain_name*>。 此对象有一个名为**rIDAvailablePool**。 此属性的值会维护整个域的全局 RID 空间。 值为具有上限和下限的部件的大整数。 上半部分定义的可以为每个域 （0x3FFFFFFF 或只需超过 10 亿） 分配的安全主体。 下半部分是在域中已分配的 Rid 数。 
  
> [!NOTE]
> 在 Windows Server 2016 和 2012年中，可以分配的安全主体数增加到超过 20 亿。 有关详细信息，请参阅[管理 RID 颁发](https://technet.microsoft.com/library/jj574229.aspx)。 
  
- 示例值：4611686014132422708  
- 低部分：2100 （从下一步的 RID 池，并将其分配的）  
- 上半部分：1073741823 （可以在域中创建的 Rid 的总数）  
  
如果增加的大整数值，则增加低部分的值。 例如，如果为示例值的总和 4611686014132522708 4611686014132422708 添加 100000，新的低部分是 102100。 这表示将由 RID 主机分配的下一步 RID 池将开头而不是 2100年 102100。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>若要使用 adsiedit 和计算器可用 RID 池的值的值

1. 打开服务器管理器中，单击**工具**然后单击**ADSI Edit**。
2. 右键单击并选择**连接到**并连接执行默认命名上下文，单击**确定**。
   ![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. 浏览到以下的可分辨的名称路径：**CN = RID Manager$，CN = System，DC =<domain name>**。
   ![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. 右键单击，然后选择属性 CN = RID Manager$。 
4. 选择的属性**rIDAvailablePool**，单击**编辑**，然后将大的整数值复制到剪贴板。
   ![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. 启动计算器，并从**视图**菜单中，选择**科学记数法模式**。 
6. 将 100,000 添加到当前值。
   ![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. 使用 ctrl + c，或**副本**命令**编辑**菜单中，将值复制到剪贴板。 
8. 在 adsi 编辑编辑对话框中，将粘贴此新值。 
   ![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. 单击**确定**在对话框中，并**应用**要更新的属性表中**rIDAvailablePool**属性。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>若要使用 LDP 的可用 RID 池的值的值  
  
1. 在命令提示符下，键入以下命令，然后按 Enter：  
   **ldp**  
2. 单击**连接**，单击**Connect**键入 RID 管理器的名称，然后单击**确定**。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. 单击**连接**，单击**绑定**，选择**使用凭据绑定**并键入你的管理凭据，然后单击**确定**。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. 单击**视图**，单击**树**然后键入以下的可分辨的名称路径：CN = RID Manager$，CN = System，DC =*域名*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. 单击**浏览**，然后单击**修改**。 
6. 添加到当前 100,000 **rIDAvailablePool**值，，然后键入到总和**值**。 
7. 在中**Dn**，类型`cn=RID Manager$,cn=System,dc=` *< 域名\>*。 
8. 在中**编辑条目属性**，类型`rIDAvailablePool`。 
9. 选择**替换为**作为操作，然后单击**Enter**。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. 单击**运行**运行此操作。 单击 **“关闭”**。
11. 若要验证的更改，请单击**视图**，单击**树**，然后键入以下的可分辨的名称路径： CN = RID Manager$，CN = System，DC =*域名*。   检查**rIDAvailablePool**属性。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)

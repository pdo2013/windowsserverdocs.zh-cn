---
title: "广告森林恢复-引发不用池"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>广告森林恢复-引发的值的可用 RID 池 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
 使用以下步骤来提升值相对 ID (RID) 的池还原该直流后，将分配 RID 操作主机。 抬起可用 RID 池值，你可以确保 DC 不分配 RID，用来还原域备份之后创建一个安全主体的。  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>有关 Active Directory 不用池和 rIDAvailablePool
 每个域有对象**CN = RID 管理器美元，CN = 系统特区**=<*域名*>。 此对象有一个名为**rIDAvailablePool**。 此特性值维护整个的域的全球 RID 空间。 值为较大整型与上下部分。 上部定义安全主体可以为每个域（0x3FFFFFFF 或刚好超过 10 亿）分配了的数。 下半部分介绍 Rid 已在域中分配数。  
  
> [!NOTE]
>  Windows Server 2016 2012，在可以分配的安全主体数增加到刚好超过 2 亿。 有关详细信息，请参阅[管理不用颁发](https://technet.microsoft.com/library/jj574229.aspx)。  
  
-   4611686014132422708 示例值：  
  
-   低一部分：2100（的下一个 RID 池分配的开头）  
  
-   部分：1073741823（总数 Rid 可以创建在域中）  
  
 当增加的大型整数值时，你会增加值较低的一部分。 例如，如果 100000 添加的总和为 4611686014132522708 4611686014132422708 的示例值，新低的一部分是 102100。 这表示将通过 RID 主机分配下一步 RID 池将开头而不是 2100 年 102100。  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>增大 adsiedit 和计算器使用的可 RID 池该值  
1.  打开服务器管理器中，单击**工具**单击**Adsi**。    
2.  右键单击选择**连接到**和连接执行默认命名上下文，并单击**确定**。
![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. 浏览到以下识别的名路径：**CN = RID 管理器美元，CN = 系统特区 =<domain name>**。
![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  右键单击并选择属性 CN 和 = RID $ 管理器。  
4.  选择属性**rIDAvailablePool**，单击**编辑**，然后将较大的整数复制到剪贴板。
![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  开始计算器，并从**视图**菜单上，选择**科学型模式下**。  6.  添加 100000 的当前值。  
![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  使用 ctrl c 或**副本**从命令**编辑**菜单上的值复制到剪贴板。  
8.  在 adsiedit 编辑对话框中，将粘贴这一新值。 
![ADSI 编辑](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. 单击**确定**在对话框中，并**应用**更新属性页中**rIDAvailablePool**属性。  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>若要使用 LDP 可用 RID 池的值的值  
  
1.  在命令提示符下，键入以下命令，，然后按 ENTER:  
  
     **ldp**  
  
2.  单击**连接**，单击**连接**、键入 RID 管理器的名称，然后单击**确定**。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  单击**连接**，单击**绑定**、选择**使用凭据绑定**并键入你管理的凭据，然后单击**确定**。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  单击**视图**，单击**树**，然后键入以下识别的名路径：CN = RID 管理器 $，CN = 系统特区 =*域名*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  单击**浏览**，然后单击**修改**。  
6.  添加到当前 100000 **rIDAvailablePool**值，然后键入插入 sum**值**。  
7.  在**Dn**，类型`cn=RID Manager$,cn=System,dc=`*< 域 name\ >*。  
8.  在**编辑条目特性**，类型`rIDAvailablePool`。  
9. 选择**替换**作为操作，然后单击**Enter**。 </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. 单击**运行**要执行的操作。  单击**关闭**。
11. 若要验证更改，请单击**视图**，单击**树**，然后键入以下识别的名路径：CN = RID 管理器美元，CN = 系统特区 =*域名*。    检查**rIDAvailablePool**属性。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
 

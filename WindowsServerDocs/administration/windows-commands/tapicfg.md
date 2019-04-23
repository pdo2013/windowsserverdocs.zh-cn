---
title: tapicfg
description: 了解如何管理 TAPI 应用程序目录分区。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6e89b1e3d7638fbcbe0140658d2d2a9af1ccd852
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886508"
---
# <a name="tapicfg"></a>tapicfg

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建、 删除或显示 TAPI 应用程序目录分区，或设置默认 TAPI 应用程序目录分区。 TAPI 3.1 客户端可以使用目录服务定位器服务使用此应用程序目录分区中的信息来查找并与 TAPI 目录通信。此外可以使用**tapicfg**创建或删除服务连接点，使 TAPI 客户端能够有效地在域中查找 TAPI 应用程序目录分区。 有关详细信息，请参阅备注。 若要查看命令语法，请单击命令。 
-   [tapicfg install](#BKMK_install)
-   [tapicfg remove](#BKMK_remove)
-   [tapicfg publishscp](#BKMK_publishscp)
-   [tapicfg removescp](#BKMK_removescp)
-   [tapicfg show](#BKMK_show)
-   [tapicfg makedefault](#BKMK_makedefault)

## <a name="BKMK_install"></a>tapicfg 安装
创建 TAPI 应用程序目录分区。

### <a name="syntax"></a>语法
```
tapicfg install /directory:<PartitionName> [/server:<DCName>] [/forcedefault]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|安装 /directory:\<PartitionName >|必需。 指定要创建的 TAPI 应用程序目录分区的 DNS 名称。 此名称必须是完全限定的域名。|
|/server:\<DCName>|指定在其创建 TAPI 应用程序目录分区的域控制器的 DNS 名称。 如果未指定的域控制器名称，则使用本地计算机的名称。|
|/forcedefault|指定此目录是默认 TAPI 应用程序目录分区的域。 在域中可以有多个 TAPI 应用程序目录分区。<br /><br />如果此目录是在域上创建的第一个 TAPI 应用程序目录分区，自动设置为默认值，而不管你使用 **/forcedefault**选项。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_remove"></a>tapicfg 删除
删除 TAPI 应用程序目录分区。

### <a name="syntax"></a>语法
```
tapicfg remove /directory:<PartitionName>
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|删除 /directory:\<PartitionName >|必需。 指定要删除的 TAPI 应用程序目录分区的 DNS 名称。 请注意，此名称必须是完全限定的域名。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_publishscp"></a>tapicfg publishscp
创建服务连接点来发布 TAPI 应用程序目录分区。

### <a name="syntax"></a>语法
```
tapicfg publishscp /directory:<PartitionName> [/domain:<DomainName>] [/forcedefault]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|publishscp /directory:\<PartitionName >|必需。 指定将发布服务连接点的 TAPI 应用程序目录分区的 DNS 名称。|
|/domain:\<DomainName >|指定创建服务连接点所在的域的 DNS 名称。 如果未指定的域名，则使用本地域的名称。|
|/forcedefault|指定此目录是默认 TAPI 应用程序目录分区的域。 在域中可以有多个 TAPI 应用程序目录分区。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_removescp"></a>tapicfg removescp
删除 TAPI 应用程序目录分区的服务连接点。

### <a name="syntax"></a>语法
```
tapicfg removescp /directory:<PartitionName> [/domain:<DomainName>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|removescp /directory:\<PartitionName >|必需。 指定将删除服务连接点的 TAPI 应用程序目录分区的 DNS 名称。|
|/domain:\<DomainName>|指定从中删除服务连接点的域的 DNS 名称。 如果未指定的域名，则使用本地域的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_show"></a>tapicfg show
在域中显示的名称和位置的 TAPI 应用程序目录分区。

### <a name="syntax"></a>语法
```
tapicfg show [/defaultonly][ /domain:<DomainName>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/defaultonly|显示的名称和位置仅默认 TAPI 应用程序目录分区的域中。|
|/domain:\<DomainName>|指定要显示其 TAPI 应用程序目录分区的域的 DNS 名称。 如果未指定的域名，则使用本地域的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_makedefault"></a>tapicfg makedefault
设置默认 TAPI 应用程序目录分区的域。

### <a name="syntax"></a>语法
```
tapicfg makedefault /directory:<PartitionName> [/domain:<DomainName>]  
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|makedefault /directory:\<PartitionName >|必需。 指定设置为默认分区的域的 TAPI 应用程序目录分区的 DNS 名称。 请注意，此名称必须是完全限定的域名。 指定为其 TAPI 应用程序目录分区设置为默认的域的 DNS 名称。 如果未指定的域名，则使用本地域的名称。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
您必须是 Enterprise Admins 组的成员在运行 active directory 中**tapicfg 安装**（若要创建 TAPI 应用程序目录分区） 或**tapicfg 删除**（若要删除 TAPI 的应用程序目录分区）。

可以是域成员的任何计算机上运行此命令行工具。

如果安装了相应的字体和语言支持 （如 TAPI 应用程序目录分区、 服务器和域的名称） 与国际或 Unicode 字符的用户提供的文本仅正确显示。

您仍可以使用 Internet 定位符服务 (ILS) 服务器在你的组织，ILS 需支持某些应用程序，因为运行 Windows XP 或 Windows Server 2003 操作系统的 TAPI 客户端可以查询 ILS 服务器或 TAPI 应用程序目录分区。

可以使用**tapicfg**创建或删除服务连接点。 如果出于任何原因 （例如，如果重命名它所在的域） 重命名 TAPI 应用程序目录分区，必须删除现有的服务连接点并创建一个包含 TAPI 应用程序目录的新 DNS 名称的新要发布的分区。 否则，TAPI 客户端将无法找到并访问 TAPI 应用程序目录分区。 您还可以删除维护或安全目的的服务连接点 （例如，如果您不希望公开特定的 TAPI 应用程序目录分区上的 TAPI 数据）。

## <a name="examples"></a>示例
若要创建的服务器上的命名的 tapifiction.testdom.microsoft.com 名为 testdc.testdom.microsoft.com，然后将其设置为新域的默认 TAPI 应用程序目录分区 TAPI 应用程序目录分区，请键入：
```
tapicfg install /directory:tapifiction.testdom.microsoft.com /server:testdc.testdom.microsoft.com /forcedefault
```
若要显示为新的默认 TAPI 应用程序目录分区的名称，请键入：
```
tapicfg show /defaultonly
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

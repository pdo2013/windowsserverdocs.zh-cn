---
title: 开发工具扩展
description: 开发工具扩展 Windows Admin Center SDK (项目 Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445934"
---
# <a name="install-extension-payload-on-a-managed-node"></a>托管节点上安装扩展有效负载

>适用于：Windows Admin Center，Windows Admin Center 预览版

## <a name="setup"></a>安装
> [!NOTE]
> 若要遵循本指南，您将需要生成 1.2.1904.02001 或更高版本。 若要检查你的生成数打开 Windows Admin Center，然后单击右上角的问号。

如果你尚未创建[工具扩展](../develop-tool.md)为 Windows Admin Center。 完成此，请后下创建一个扩展时使用的值：

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 你的公司名称 （包含空格） | ```Contoso``` |
| ```{!Tool Name}``` | 你的工具名称 （包含空格） | ```InstallOnNode``` |

在您的工具扩展文件夹内创建```Node```文件夹 (```{!Tool Name}\Node```)。 放入此文件夹的任何内容将复制到托管节点，使用此 API 时。 添加任何所需的用例的文件。 

此外创建```{!Tool Name}\Node\installNode.ps1```脚本。 此脚本将在被管理节点上运行后的所有文件都复制,```{!Tool Name}\Node```到托管节点的文件夹。 添加你的用例的任何其他逻辑。 示例```{!Tool Name}\Node\installNode.ps1```文件：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 有 API 将查找一个特定名称。 更改此文件的名称将导致错误。


## <a name="integration-with-ui"></a>与 UI 的集成

更新```\src\app\default.component.ts```所示：

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
更新到创建该扩展时使用的值的占位符：
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

此外更新```\src\app\default.component.html```到：
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
最后```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## <a name="creating-and-installing-a-nuget-package"></a>创建和安装 NuGet 包

最后一个步骤是使用我们已添加的文件生成的 NuGet 包，然后在 Windows Admin Center 中安装该程序包。

请按照[发布扩展](../publish-extensions.md)指导如果尚未创建之前扩展包。 
> [!IMPORTANT]
> 在此扩展的.nuspec 文件中，它是重要的```<id>```值与在项目的名称匹配```manifest.json```并```<version>```匹配内容已添加到```\src\app\default.component.ts```。 此外将添加一个条目下的```<files>```: 
> 
> ```<file src="Node\**\*.*" target="Node" />```。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

创建此包后，将路径添加到该数据源。 在 Windows Admin Center 转到设置 > 扩展 > 源并将路径添加到存在的包。 完成您的扩展插件安装，您应该能够单击```install```将调用按钮和 API。  
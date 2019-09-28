---
title: 将模块添加到工具扩展中
description: 开发工具扩展 Windows 管理中心 SDK （Project Honolulu）-向工具扩展添加模块
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9d30980ca404187ff1481242c1c0ef0a3d571416
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357096"
---
# <a name="add-a-module-to-a-tool-extension"></a>将模块添加到工具扩展中

>适用于：Windows Admin Center、Windows Admin Center 预览版

在本文中，我们将向使用 Windows 管理中心 CLI 创建的工具扩展添加一个空模块。

## <a name="prepare-your-environment"></a>准备你的环境

如果尚未这样做，请按照开发[工具](../develop-tool.md)（或[解决方案](../develop-solution.md)）扩展中的说明来准备环境，并创建新的空工具扩展。

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>使用角度 CLI 创建模块（和组件）

如果你不熟悉 Angular，强烈建议你阅读 Angular.Io 网站上的文档，以了解 Angular 和 NgModule。 有关 NgModule 的详细信息，请转至： https://angular.io/guide/ngmodule

* 有关使用 Angular CLI 生成新模块的详细信息，请访问： https://github.com/angular/angular-cli/wiki/generate-module
* 有关使用 Angular CLI 生成新组件的详细信息，请访问： https://github.com/angular/angular-cli/wiki/generate-component


打开命令提示符，将目录更改为你项目中的 \src\app，然后运行以下命令，将 ```{!ModuleName}``` 替换为你的模块名称（删除空格）：

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |

示例用法：
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## <a name="add-routing-information"></a>添加路由信息

如果你不熟悉 Angular，强烈推荐你了解 Angular 路由和导航。 以下各节定义了必要的路由元素，可使 Windows Admin Center 导航到扩展并在扩展之间的视图间进行导航，以响应用户活动。 要了解详细信息，请转至： https://angular.io/guide/router

使用在上述步骤中所用的模块名称。

### <a name="add-content-to-new-routing-file"></a>将内容添加到新的路由文件

* 浏览到上一步中由 ``` ng generate ``` 创建的模块文件夹。

* 按照此命名约定，创建新文件 ```{!module-name}.routing.ts```：

    | ReplTest1 | 说明 | 示例文件名 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.routing.ts``` |

* 将此内容添加到刚创建的文件：

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '', 
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* 将刚创建的文件中的值替换为所需的值：

    | ReplTest1 | 说明 | 示例 |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

### <a name="add-content-to-new-module-file"></a>将内容添加到新模块文件

打开使用以下命名约定找到的文件 ```{!module-name}.module.ts```：

| ReplTest1 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.module.ts``` |

* 将内容添加到文件：

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* 将刚添加的内容中的值替换为所需的值：

    | ReplTest1 | 说明 | 示例 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

* 修改导入语句以导入路由：

    | 原始值 | 新值 |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* 确保 ```import``` 语句按源的字母顺序排序。

### <a name="add-content-to-new-component-typescript-file"></a>向新组件 typescript 文件添加内容

打开使用以下命名约定找到的文件 ```{!module-name}.component.ts```：

| ReplTest1 | 说明 | 示例文件名 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal.component.ts``` |
    
将文件中的内容修改为以下内容：

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### <a name="update-app-routingmodulets"></a>更新应用程序-路由

打开文件 ```app-routing.module.ts```，并修改默认路径，以便加载刚刚创建的新模块。  找到 ```path: ''``` 的条目，然后更新 ```loadChildren``` 以加载模块，而不是默认模块：

| ReplTest1 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 模块名称（删除空格） | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | 模块名称（小写，空格替换为短划线） | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
下面是已更新的默认路径的示例：
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>生成和加载扩展

现在，你已将一个模块添加到了你的扩展。  接下来，你可以在 Windows 管理中心中[构建和端加载](../develop-tool.md#build-and-side-load-your-extension)扩展，以查看结果。

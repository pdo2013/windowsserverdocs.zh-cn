---
title: HCN RPC 上下文句柄
description: ''
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 48e4626f52ef7af19da6d3e0bb28799665498fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884848"
---
# <a name="rpc-context-handles-for-hcn"></a>HCN RPC 上下文句柄

>适用于：Windows 服务器 （半年频道），Windows Server 2016


## <a name="hcnnetwork"></a>HCN_Network

HCN 网络是用于表示主机实体计算网络及其关联的系统资源和策略。 例如，HCN 网络将通常包含一组元数据 （例如 id、 名称、 类型） 的虚拟交换机、 主机虚拟网络适配器 （它将作为默认网关的网络）、 NAT 实例 （如果所需的网络类型）、 一组子网和 MAC 池并且任何网络范围的策略为应用 (如 Acl)。

使用 HCN_NETWORK RPC 上下文句柄表示 HCN 网络实体。

```

/// Handle to an operation
DECLARE_HANDLE(HCN_NETWORK);

/// Return a list of existing Networks
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Networks to
///                        return. By default, all Networks are returned.
/// \retval Networks       Receives a JSON document with the list of Networks.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateNetworks(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Networks,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Create a Network
///
/// \param  Id             Specifies the unique ID for the new Network.
/// \param  Settings       JSON document specifying the settings of the new Network.
/// \retval Network        Receives a handle to the new Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateNetwork(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_NETWORK Network,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Opens a handle to an existing Network.
///
/// \param  Id             Unique ID of the existing Network.
/// \retval Network        Receives a handle to the Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenNetwork(
    _In_ REFGUID Id,
    _Out_ PHCN_NETWORK Network,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of a Network
///
/// \param  Network        Handle to a Network.
/// \param  Settings       JSON document specifying the new settings of the Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyNetwork(
    _In_ HCN_NETWORK Network,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query Network properties
///
/// \param  Network        Handle to a Network.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Network
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Network properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryNetworkProperties(
    _In_ HCN_NETWORK Network,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete a Network
///
/// \param  Id             Unique ID of the existing Network.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteNetwork(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Close a handle to a Network
///
/// \param  Network        Handle to a Network.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseNetwork(
    _In_ HCN_NETWORK Network
    ); 
```

## <a name="hcnendpoint"></a>HCN_Endpoint

HCN 终结点是用于表示上 HCN 网络及其关联的系统资源和策略的 IP 终结点的实体。 例如，HCN 终结点将通常包含一组的元数据 （例如 id、 名称、 父网络 id），其网络标识 （例如，IP 地址、 MAC 地址），并为任何终结点特定策略应用 (例如 Acl、 路由)。
HCN 终结点实体表示使用 HCN_ENDPOINT RPC 上下文句柄。

```

/// Handle to an operation
DECLARE_HANDLE(HCN_ENDPOINT);

/// Return a list of existing Endpoints
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Endpoints to
///                        return. By default all Endpoints are returned.
/// \retval Endpoints      Receives a JSON document with the list of Endpoints.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateEndpoints(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Endpoints,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Create an Endpoint
///
/// \param  Id             Specifies the unique ID for the new Endpoint.
/// \param  Network        Handle to the network on which endpoint is to be created.
/// \param  Settings       JSON document specifying the settings of the new Endpoint.
/// \retval Endpoint       Receives a handle to the new Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateEndpoint(
    _In_ HCN_NETWORK Network,
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_ENDPOINT Endpoint,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Opens a handle to an existing Endpoint.
///
/// \param  Id             Unique ID of the existing Endpoint.
/// \retval Endpoint       Receives a handle to the Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenEndpoint(
    _In_ REFGUID Id,
    _Out_ PHCN_ENDPOINT Endpoint,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of an Endpoint
///
/// \param  Endpoint       Handle to an Endpoint.
/// \param  Settings       JSON document specifying the new settings of the Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyEndpoint(
    _In_ HCN_ENDPOINT Endpoint,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Query Endpoint properties
///
/// \param  Endpoint       Handle to an Endpoint.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Endpoint
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Endpoint properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryEndpointProperties(
    _In_ HCN_ENDPOINT Endpoint,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete an Endpoint
///
/// \param  Id             Unique ID of the existing Endpoint.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteEndpoint(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Close a handle to an Endpoint
///
/// \param  Endpoint       Handle to an Endpoint.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseEndpoint(
    _In_ HCN_ENDPOINT Endpoint
    );
 
```

## <a name="hcnnamespace"></a>HCN_Namespace

HCN Namespace 是用于表示主机计算网络命名空间的实体。 与其他命名空间使您能够拥有独立的网络环境，其中每个命名空间具有其自己的网络接口和路由表，一台主机上分开的命名空间。

HCN Namespace 实体表示使用 HCN_NAMESPACE RPC 上下文句柄。

```
/// Handle to an operation
DECLARE_HANDLE(HCN_NAMESPACE);

/// Return a list of existing Namespaces
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific Namespaces to
///                        return. By default all Namespaces are returned.
/// \retval Namespaces     Receives a JSON document with the list of Namespaces.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateNamespaces(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Namespaces,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Create a Namespace
///
/// \param  Id             Specifies the unique ID for the new Namespace.
/// \param  Settings       JSON document specifying the settings of the new Namespace.
/// \retval Namespace      Receives a handle to the new Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateNamespace(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_NAMESPACE Namespace,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Opens a handle to an existing Namespace.
///
/// \param  Id             Unique ID of the existing Namespace.
/// \retval Namespace      Receives a handle to the Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenNamespace(
    _In_ REFGUID Id,
    _Out_ PHCN_NAMESPACE Namespace,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Modify the settings of a Namespace
///
/// \param  Namespace      Handle to a Namespace.
/// \param  Settings       JSON document specifying the new settings of the Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyNamespace(
    _In_ HCN_NAMESPACE Namespace,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query Namespace properties
///
/// \param  Namespace      Handle to a Namespace.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the Namespace
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with Namespace properties.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryNamespaceProperties(
    _In_ HCN_NAMESPACE Namespace,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );
/// Delete a Namespace
///
/// \param  Id             Unique ID of the existing Namespace.
/// \retval ErrorRecord    Optional, receives a JSON document on failure with extended result
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteNamespace(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Close a handle to a Namespace
///
/// \param  Namespace      Handle to a Namespace.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseNamespace(
    _In_ HCN_NAMESPACE Namespace
    );

```

## <a name="hcnloadbalancer"></a>HCN_LoadBalancer

HCN 负载均衡器是用于表示主机计算网络负载均衡器的实体。 使用负载均衡器，可具有负载平衡的主机计算网络终结点。
使用 HCN_LOADBALANCER RPC 上下文句柄表示 HCN 负载均衡器的实体。

```
/// Handle to an operation
DECLARE_HANDLE(HCN_LOADBALANCER);

//////
/// LoadBalancer Methods

/// Return a list of existing LoadBalancers
///
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing properties of the specific LoadBalancers to
///                        return. By default all LoadBalancers are returned.
/// \retval LoadBalancers    Receives a JSON document with the list of LoadBalancers.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnEnumerateLoadBalancers(
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Create a LoadBalancer
///
/// \param  Id             Specifies the unique ID for the new LoadBalancer.
/// \param  Settings       JSON document specifying the settings of the new LoadBalancer.
/// \retval LoadBalancer     Receives a handle to the new LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCreateLoadBalancer(
    _In_ REFGUID Id,
    _In_ PCWSTR Settings,
    _Out_ PHCN_LOADBALANCER LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Opens a handle to an existing LoadBalancer.
///
/// \param  Id             Unique ID of the existing LoadBalancer.
/// \retval LoadBalancer     Receives a handle to the LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnOpenLoadBalancer(
    _In_ REFGUID Id,
    _Out_ PHCN_LOADBALANCER LoadBalancer,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Modify the settings of a PolcyList
///
/// \param  PolcyList      Handle to a PolcyList.
/// \param  Settings       JSON document specifying the new settings of the PolcyList.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnModifyLoadBalancer(
    _In_ HCN_LOADBALANCER LoadBalancer,
    _In_ PCWSTR Settings,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Query LoadBalancer properties
///
/// \param  LoadBalancer     Handle to a LoadBalancer.
/// \param  Query          Optionally specifies a JSON document for a query
///                        containing specific properties of the LoadBalancer
///                        return. By default all properties are returned.
/// \retval Properties     Receives a JSON document with LoadBalancer properties.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnQueryLoadBalancerProperties(
    _In_ HCN_LOADBALANCER LoadBalancer,
    _In_ PCWSTR Query,
    _Outptr_ PWSTR* Properties,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Delete a LoadBalancer
///
/// \param  Id             Unique ID of the existing LoadBalancer.
/// \retval ErrorRecord    Optional, receives a JSON document with extended errorCode
///                        information. The caller must release the buffer using
///                        CoTaskMemFree.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnDeleteLoadBalancer(
    _In_ REFGUID Id,
    _Outptr_opt_ PWSTR* ErrorRecord
    );

/// Close a handle to a LoadBalancer
///
/// \param  LoadBalancer     Handle to a LoadBalancer.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT
WINAPI
HcnCloseLoadBalancer(
    _In_ HCN_LOADBALANCER LoadBalancer

```

## <a name="hcnnotificationcallback"></a>HCN_Notification_Callback

那里函数提供对整个服务操作，例如通知 （例如网络创建新的接收通知） 的访问。

```
/// Registers a callback function to receive notifications of service-wide events such as network
/// creations/deletions.
///
/// \param  Callback           Function pointer to notification callback.
/// \param  Context            Context pointer.
/// \retval CallbackHandle     Receives a handle to a callback registered on a Service.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT WINAPI
HcnRegisterServiceCallback(
    _In_ HCN_NOTIFICATION_CALLBACK Callback,
    _In_ void* Context,
    _Out_ HCN_CALLBACK* CallbackHandle
    );

/// Unregisters from service-wide notifications
///
/// \retval CallbackHandle     Handle to a callback registered on a Service.
///
/// \returns S_OK if successful; HResult error code on failures.
///
HRESULT WINAPI
HcnUnregisterServiceCallback(
    _In_ HCN_CALLBACK CallbackHandle
    );
```



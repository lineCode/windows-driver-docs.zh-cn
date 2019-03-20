---
title: IRP_MN_QUERY_RESOURCES
description: PnP 管理器使用此 IRP 获取设备的启动配置资源。总线驱动程序必须处理此请求适用于需要的硬件资源及其子设备。 函数和筛选器驱动程序不处理此 IRP。
ms.date: 08/12/2017
ms.assetid: b9a6f06b-07d9-4539-bd41-21cdccdc4b25
keywords:
- IRP_MN_QUERY_RESOURCES 内核模式驱动程序体系结构
ms.localizationpriority: medium
ms.openlocfilehash: e7f88927d772ed26d945456dfd46c6fe2f939af3
ms.sourcegitcommit: a33b7978e22d5bb9f65ca7056f955319049a2e4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "56547719"
---
# <a name="irpmnqueryresources"></a>IRP\_MN\_查询\_资源


PnP 管理器使用此 IRP 获取设备的启动配置资源。

总线驱动程序必须处理此请求适用于需要的硬件资源及其子设备。 函数和筛选器驱动程序不处理此 IRP。

<a name="major-code"></a>主代码
----------

[**IRP\_MJ\_PNP**](irp-mj-pnp.md) When Sent
---------

PnP 管理器将发送此 IRP，枚举设备时。

PnP 管理器将此 IRP 发送在 IRQL 被动\_级别在任意线程上下文中。

## <a name="input-parameters"></a>输入参数


无

## <a name="output-parameters"></a>输出参数


返回在 I/O 状态块中。

## <a name="io-status-block"></a>I/O 状态块


处理此 IRP 的总线驱动程序设置**Irp-&gt;IoStatus.Status**于状态\_成功或相应的错误状态。

如果成功，总线驱动程序设置**Irp-&gt;IoStatus.Information**指向的[ **CM\_资源\_列表**](https://msdn.microsoft.com/library/windows/hardware/ff541994) ，其中包含所需的信息。 发生错误时，总线驱动程序设置**Irp-&gt;IoStatus.Information**为零。

<a name="operation"></a>操作
---------

如果总线驱动程序对此 IRP 响应中返回的资源列表，它会分配[ **CM\_资源\_列表**](https://msdn.microsoft.com/library/windows/hardware/ff541994)从分页的内存。 当不再需要时，即插即用管理器释放缓冲区。

如果设备不需要任何硬件资源，将设备的父总线驱动程序会完成 IRP ([**IoCompleteRequest**](https://msdn.microsoft.com/library/windows/hardware/ff548343)) 而无需修改**Irp-&gt;IoStatus.Status**或**Irp-&gt;IoStatus.Information**。

函数和筛选器驱动程序不会收到此 IRP。

请参阅[插](https://msdn.microsoft.com/library/windows/hardware/ff547125)处理的常规规则[即插即用次要 Irp](plug-and-play-minor-irps.md)。

**发送此 IRP**

保留供系统使用。 驱动程序必须发送此 IRP。

驱动程序可以调用[ **IoGetDeviceProperty** ](https://msdn.microsoft.com/library/windows/hardware/ff549203)原始和已翻译的窗体中获取设备的启动配置。

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>标头</p></td>
<td>Wdm.h 中 （包括 wdm.h 中、 Ntddk.h 或 Ntifs.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另请参阅


[**CM\_资源\_列表**](https://msdn.microsoft.com/library/windows/hardware/ff541994)

[**IoGetDeviceProperty**](https://msdn.microsoft.com/library/windows/hardware/ff549203)

 

 




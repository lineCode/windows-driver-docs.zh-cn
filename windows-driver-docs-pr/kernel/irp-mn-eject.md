---
title: IRP_MN_EJECT
description: 总线驱动程序通常会为支持设备弹出的子设备（子 PDOs）处理此请求。 函数和筛选器驱动程序不会收到此请求。
ms.date: 08/12/2017
ms.assetid: 2807eeca-c614-469a-baeb-3d2d65416c57
keywords:
- IRP_MN_EJECT 内核模式驱动程序体系结构
ms.localizationpriority: medium
ms.openlocfilehash: f3d983bfe7094e6ccf952b7aec592a23cff3fa1a
ms.sourcegitcommit: 7681ac46c42782602bd3449d61f7ed4870ef3ba7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82922469"
---
# <a name="irp_mn_eject"></a>IRP\_MN\_弹出


总线驱动程序通常会为支持设备弹出的子设备（子 PDOs）处理此请求。 函数和筛选器驱动程序不会收到此请求。

## <a name="value"></a>值

0x11

<a name="major-code"></a>主要代码
----------

[**IRP\_MJ\_PNP**](irp-mj-pnp.md)

<a name="when-sent"></a>发送时间
---------

PnP 管理器发送此 IRP 以指示适当的驱动程序或驱动程序从其插槽中弹出设备。

PnP 管理器在任意线程上下文中以\_IRQL 被动级别发送此 IRP。

## <a name="input-parameters"></a>输入参数


None

## <a name="output-parameters"></a>输出参数


None

## <a name="io-status-block"></a>I/o 状态块


总线驱动程序将**Irp-&gt;IoStatus**设置为 "\_成功" 或相应的 "错误" 状态。

如果成功，则总线驱动程序将**Irp&gt;-IoStatus**设置为零。

如果总线驱动程序未处理此 IRP，它会将**irp-&gt;IoStatus**保持原样，并完成 irp。

<a name="operation"></a>操作
---------

对于要弹出的设备，设备必须处于 D3 设备电源状态（关闭），并且必须解锁（如果设备支持锁定）。

为此 IRP 返回成功的任何驱动程序必须等待设备在完成 IRP 之前已弹出。

请参阅[即插即用](https://docs.microsoft.com/windows-hardware/drivers/kernel/implementing-plug-and-play)，了解用于处理[即插即用次要 irp](plug-and-play-minor-irps.md)的一般规则。

**正在发送此 IRP**

预留给系统使用。 驱动程序不得发送此 IRP。

请参阅[**IoRequestDeviceEject**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iorequestdeviceeject)例程的参考页。

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
<td>Wdm.h（包括 Wdm.h、Ntddk.h 或 Ntifs.h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另请参阅


[**IoRequestDeviceEject**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-iorequestdeviceeject)

 

 





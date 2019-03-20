---
title: WDI_TLV_INTERFACE_CAPABILITIES
description: WDI_TLV_INTERFACE_CAPABILITIES 是 TLV 包含 Wi-fi 接口的功能。
ms.assetid: 308331DD-FEEB-4C49-BEBD-117AE58D4792
ms.date: 07/18/2017
keywords:
- 从 Windows Vista 开始 WDI_TLV_INTERFACE_CAPABILITIES 网络驱动程序
ms.localizationpriority: medium
ms.openlocfilehash: 5482be969dd2b31106647416c4a847a5ae6ba480
ms.sourcegitcommit: a33b7978e22d5bb9f65ca7056f955319049a2e4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "56523289"
---
# <a name="wditlvinterfacecapabilities"></a>WDI\_TLV\_接口\_功能


WDI\_TLV\_接口\_功能是包含的功能的 Wi-fi 接口 TLV。

## <a name="tlv-type"></a>TLV 类型


0xF

## <a name="length"></a>长度


所有包含的元素的大小的总和 （以字节为单位）。

## <a name="values"></a>值


键入说明 UINT32 的最大传输单元 (MTU) 大小。
UINT32 适配器多播的列表大小。
UINT16 回填的大小，以字节为单位。 此值不能超过 256 个字节。
[**WDI\_MAC\_地址**](https://msdn.microsoft.com/library/windows/hardware/dn926071)适配器的永久 MAC 地址。 如果设备支持多个永久 MAC 地址，则应返回将由设备的第一个 MAC 地址。
UINT32 支持的最大以 kbps 为单位的此适配器的发送速率。
UINT32 支持的最大接收此适配器的速率以 kbps 为单位。
UINT8 指定是否由硬件启用单选。 有效值为 0 （禁用） 和 1 （启用）。
UINT8 指定它们单选是否启用软件。 有效值为 0 （禁用） 和 1 （启用）。
UINT8 指定接口是否支持 PLR。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定接口是否支持 FLR。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定是否支持发送和接收操作帧。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 RX 空间流的受支持的数量。
UINT8 TX 空间流的受支持的数量。
UINT8 的适配器可以同时处理，而不考虑操作模式的通道数。
UINT8 指定天线多样性受支持。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定是否支持 eCSA。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定适配器是否支持 MAC 地址随机化。 有效值为 0 （不支持） 和 1 （支持）。
[**WDI\_MAC\_地址**](https://msdn.microsoft.com/library/windows/hardware/dn926071)一个位掩码，它指定为位指示是否可为每个地址随机 (0) 还是应保留永久地址 (1) 相同的值。 默认值为全部为零。
[**WDI\_蓝牙\_共存\_支持**](https://msdn.microsoft.com/library/windows/hardware/dn897795) (UINT32) Wi-fi-蓝牙共存的支持的级别。
UINT8 指定 WDI OID 支持。 有效值包括：
-   0 :不支持。 Microsoft 组件并不了解不转发到适配器的 Oid。
-   1 :支持。 Microsoft 组件不能理解转发到适配器的 Oid。

这些 Oid 将不包含 WDI 标头。 若要标识该请求来自的适配器的端口，请使用 NdisPortNumber 中 NDIS\_OID\_请求并将它匹配中的一个[WDI\_任务\_创建\_端口](https://msdn.microsoft.com/library/windows/hardware/dn925949).

UINT8 指定是否支持快速转换。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定是否支持 Mu MIMO。 有效值为 0 （不支持） 和 1 （支持）。
UINT8 指定如果此接口不能支持 Miracast 的接收器。 有效值为 0 （支持） 和 1 （不支持）。
UINT8 指定如果 802.11v BSS 转换支持。 有效值为 0 （不支持） 和 1 （支持）。
UINT8

在 Windows 10，版本 1607，WDI 版本 1.0.21 中添加。

指定设备是否支持 IP 停靠功能。 有效值为 0 （不支持） 和 1 （支持）。
 

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>最低受支持的客户端</p></td>
<td><p>Windows 10</p></td>
</tr>
<tr class="even">
<td><p>最低受支持的服务器</p></td>
<td><p>Windows Server 2016</p></td>
</tr>
<tr class="odd">
<td><p>标头</p></td>
<td>Wditypes.hpp</td>
</tr>
</tbody>
</table>

 

 




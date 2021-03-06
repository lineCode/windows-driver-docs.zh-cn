---
title: 显示适配器的子设备
description: 显示适配器的子设备
ms.assetid: 9fd20e1a-db98-4571-8fc4-6d33fd0e2f16
keywords:
- 视频显示网络 WDK 显示，显示适配器子设备
- VidPN WDK 显示，显示适配器子设备
- 子设备 WDK 视频呈现网络
- 显示适配器子设备 WDK 视频呈现网络
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 2956e6a8976206d0b128a4125b80b9657e618f3f
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72839056"
---
# <a name="child-devices-of-the-display-adapter"></a>显示适配器的子设备


显示适配器的子设备是显示适配器上的一个设备，由显示微型端口驱动程序枚举为子项。 显示适配器的所有子设备都在板上;连接到显示适配器的监视器和其他外部设备不被视为子设备。

显示微型端口驱动程序的[**DxgkDdiQueryChildRelations**](https://docs.microsoft.com/windows-hardware/drivers/ddi/dispmprt/nc-dispmprt-dxgkddi_query_child_relations)函数负责枚举显示适配器的子设备。 在枚举过程中，显示微型端口驱动程序将每个子设备指定一个类型和热插拔检测（HPD）感知值。 该类型是[**DXGK\_子\_设备\_类型**](https://docs.microsoft.com/windows-hardware/drivers/ddi/dispmprt/ne-dispmprt-_dxgk_child_device_type)枚举器之一：

-   **TypeVideoOutput**

-   **TypeOther**

HPD 感知值是[**DXGK\_子\_设备\_HPD\_感知**](https://docs.microsoft.com/windows-hardware/drivers/ddi/d3dkmdt/ne-d3dkmdt-_dxgk_child_device_hpd_awareness)枚举器之一：

-   **HpdAwarenessAlwaysConnected**

-   **HpdAwarenessInterruptible**

-   **HpdAwarenessPolled**

下表提供了具有各种类型和 HPD 感知值的设备的一些示例。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HpdAwareness</th>
<th align="left">VideoOutput</th>
<th align="left">其他</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>AlwaysConnected</strong></p></td>
<td align="left"><p>台式计算机上集成 LCD 面板的输出</p></td>
<td align="left"><p>电视调谐器</p>
<p>交叉栏开关</p>
<p>MPEG2 编解码器</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>间断</strong></p></td>
<td align="left"><p>DVI</p>
<p>HDMI</p>
<p>便携计算机上集成 LCD 面板的输出</p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>轮询</strong></p></td>
<td align="left"><p>S-视频</p>
<p>HD15</p></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

操作系统使用多种策略中的一种，具体取决于 HPD 知晓值，确定外部设备是否已连接到子设备。 下表简要说明了操作系统如何确定具有各种 HPD 知晓值的设备的连接状态。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HpdAwareness</th>
<th align="left">操作系统如何确定连接状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>AlwaysConnected</p></td>
<td align="left"><p>操作系统知道子设备始终存在。 没有任何外部设备连接到子设备或断开与子设备的连接。</p></td>
</tr>
<tr class="even">
<td align="left"><p>间断</p></td>
<td align="left"><p>当外部显示设备连接到或与子设备断开连接时，系统会发出通知。 （当盖子打开并在盖子关闭时断开连接时，便携式计算机上的 "显示" 面板被视为已连接。）</p></td>
</tr>
<tr class="odd">
<td align="left"><p>轮询</p></td>
<td align="left"><p>操作系统会询问外部显示设备是否已连接到子设备。</p></td>
</tr>
</tbody>
</table>

 

 

 






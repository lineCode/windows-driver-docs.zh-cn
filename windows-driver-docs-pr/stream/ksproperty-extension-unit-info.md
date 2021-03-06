---
title: KSPROPERTY\_扩展\_单元\_信息
description: KSPROPERTY\_扩展\_UNIT\_INFO 属性检索扩展单元描述符的 guidExtensionCode、bNumControls、bNrInPins 和 baSourceID 成员。
ms.assetid: a7a2f655-8df7-4260-883f-53d6f5a7c6f3
keywords:
- KSPROPERTY_EXTENSION_UNIT_INFO 流媒体设备
topic_type:
- apiref
api_name:
- KSPROPERTY_EXTENSION_UNIT_INFO
api_location:
- Ksmedia.h
api_type:
- HeaderDef
ms.date: 11/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: 0d417ced4a20bec54a3451a8fe0318d322bf6633
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72837954"
---
# <a name="ksproperty_extension_unit_info"></a>KSPROPERTY\_扩展\_单元\_信息


KSPROPERTY\_扩展\_UNIT\_INFO 属性检索扩展单元描述符的 guidExtensionCode、bNumControls、bNrInPins 和 baSourceID 成员。

## <span id="ddk_ksproperty_extension_unit_info_ks"></span><span id="DDK_KSPROPERTY_EXTENSION_UNIT_INFO_KS"></span>


### <a name="usage-summary-table"></a>使用情况摘要表

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>“获取”</th>
<th>设置</th>
<th>目标</th>
<th>属性描述符类型</th>
<th>属性值类型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>“是”</p></td>
<td><p>无</p></td>
<td><p>筛选器节点</p></td>
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ks/ns-ks-ksp_node" data-raw-source="[&lt;strong&gt;KSP_NODE&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ks/ns-ks-ksp_node)"><strong>KSP_NODE</strong></a></p></td>
<td><p>PVOID</p></td>
</tr>
</tbody>
</table>

 

<a name="remarks"></a>备注
-------

此属性适用于 Windows Vista 和更高版本，以及适用于 Microsoft DirectX 9.2 或更高版本的 SDK。

在设备启动过程中，系统提供的 USB 视频类驱动程序（*Usbvideo*）将从设备的扩展单元描述符缓存信息。 然后， *Usbvideo*使用此缓存信息来响应\_单元\_信息的 KSPROPERTY\_扩展。

因此，此属性返回的字段与扩展单元描述符中设备提供的字段相同。 有关此类描述符的示例，请参阅[示例扩展单元描述符](https://docs.microsoft.com/windows-hardware/drivers/stream/sample-extension-unit-descriptor)。

具体而言，KSPROPERTY\_扩展\_UNIT\_信息返回扩展单元 GUID，后跟来自说明符的数据字段，如下表所示。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>bNumControls</p></td>
<td><p>此扩展单元中控件的数目。</p></td>
</tr>
<tr class="even">
<td><p>bNrInPins</p></td>
<td><p>扩展单元中的输入插针数目。</p></td>
</tr>
<tr class="odd">
<td><p>baSourceID （n）</p></td>
<td><p>此扩展单位的引脚<em>n</em>连接到的单元或终端的标识符。 这是一个硬件标识符，而不是一个 DirectShow 标识符。</p></td>
</tr>
</tbody>
</table>

 

下面的代码示例演示如何提交 KSPROPERTY\_扩展\_UNIT\_信息，该信息来自[示例扩展单元插件 DLL](https://docs.microsoft.com/windows-hardware/drivers/stream/sample-extension-unit-plug-in-dll)中所示的完整示例：

```cpp
ExtensionProp.Property.Set = PROPSETID_VIDCAP_EXTENSION_UNIT;
    ExtensionProp.Property.Id = KSPROPERTY_EXTENSION_UNIT_INFO;
    ExtensionProp.Property.Flags = KSPROPERTY_TYPE_GET | 
                                   KSPROPERTY_TYPE_TOPOLOGY;
    ExtensionProp.NodeId = m_dwNodeId;

    hr = m_pKsControl->KsProperty(
        (PKSPROPERTY) &ExtensionProp,
        sizeof(ExtensionProp),
        NULL,
        0,
        &ulBytesReturned);
```

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>版本</p></td>
<td><p>适用于 Windows Vista 和更高版本的操作系统，以及适用于 Microsoft DirectX 9.2 或更高版本的 SDK。</p></td>
</tr>
<tr class="even">
<td><p>标头</p></td>
<td>Ksmedia （包括 Ksmedia）</td>
</tr>
</tbody>
</table>

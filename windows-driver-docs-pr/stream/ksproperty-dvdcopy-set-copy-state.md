---
title: KSPROPERTY\_DVDCOPY\_集\_复制\_状态
description: KSPROPERTY\_DVDCOPY\_集\_COPY\_状态属性设置 DVD 解码器流的复制状态。 此属性可用于实现。
ms.assetid: f4e46d79-c70b-413a-9702-a73d3776ee2c
keywords:
- KSPROPERTY_DVDCOPY_SET_COPY_STATE 流媒体设备
topic_type:
- apiref
api_name:
- KSPROPERTY_DVDCOPY_SET_COPY_STATE
api_location:
- ksmedia.h
api_type:
- HeaderDef
ms.date: 11/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: 5b098d9753b25c4ce5d3de9bfecab6aeb91d6b83
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72843323"
---
# <a name="ksproperty_dvdcopy_set_copy_state"></a>KSPROPERTY\_DVDCOPY\_集\_复制\_状态


KSPROPERTY\_DVDCOPY\_集\_COPY\_状态属性设置 DVD 解码器流的复制状态。 此属性可用于实现。

## <span id="ddk_ksproperty_dvdcopy_set_copy_state_ks"></span><span id="DDK_KSPROPERTY_DVDCOPY_SET_COPY_STATE_KS"></span>


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
<td><p>“是”</p></td>
<td><p>大头针</p></td>
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ks/ns-ks-ksidentifier" data-raw-source="[&lt;strong&gt;KSPROPERTY&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ks/ns-ks-ksidentifier)"><strong>KSPROPERTY</strong></a></p></td>
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_set_copy_state" data-raw-source="[&lt;strong&gt;KS_DVDCOPY_SET_COPY_STATE&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_set_copy_state)"><strong>KS_DVDCOPY_SET_COPY_STATE</strong></a></p></td>
</tr>
</tbody>
</table>

 

属性值（操作数据）是一个 KS\_DVDCOPY\_集\_复制\_状态结构，该结构描述 DVD 解码器流的版权保护状态。

<a name="remarks"></a>备注
-------

此属性指示此 pin 是否需要 CSS 身份验证。 如果未实现该属性，默认值将被假定为**ks\_DVDCOPYSTATE\_AUTHENTICATION\_** [**ks\_DVDCOPYSTATE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ne-ksmedia-ks_dvdcopystate)枚举中的必需值。

此属性的主要用途是用于支持具有相同 decrypter 的多个 pin 的解码器。 例如，如果一个筛选器同时提供了子画面和视频解码，则只需为这两个 pin 中的一个提供交换密钥。 如果某个筛选器将返回**KS\_DVDCOPYSTATE\_身份验证\_不\_** 其中一个 pin 需要，则它必须始终 **\_** 返回在其上颁发属性的第一个插针。

当此属性作为**Get**调用发出时，筛选器可以使用**ks\_DVDCOPYSTATE\_authentication\_必需**的或 KS\_DVDCOPYSTATE\_身份验证，\_不需要\_身份验证。

如果此属性作为**集**调用发出，则这是一个信息性调用，由硬件解码器用来指示正在输入版权保护协商的哪个阶段。 解码器可以通过以下方式之一来保持\_状态集的状态，直到收到正确的位，指出需要新的 CSS 密钥：

<span id="KS_DVDCOPYSTATE_INITIALIZE"></span><span id="ks_dvdcopystate_initialize"></span> **\_INITIALIZE\_KS**  
指示光盘密钥协商序列的开头。

<span id="KS_DVDCOPYSTATE_INITIALIZE_TITLE"></span><span id="ks_dvdcopystate_initialize_title"></span>**KS\_DVDCOPYSTATE\_初始化\_标题**  
指示标题键协商序列的开头。

<span id="KS_DVDCOPYSTATE_DONE"></span><span id="ks_dvdcopystate_done"></span>**KS\_DVDCOPYSTATE\_完成**  
指示密钥协商序列的完成。

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
<td>Ksmedia （包括 Ksmedia）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另请参阅


[**KS\_DVDCOPY\_集\_复制\_状态**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ns-ksmedia-_ks_dvdcopy_set_copy_state)

[**KS\_DVDCOPYSTATE**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ksmedia/ne-ksmedia-ks_dvdcopystate)

[DVD 版权保护](https://docs.microsoft.com/windows-hardware/drivers/stream/dvd-copyright-protection)

[同一硬件上的多个数据流](https://docs.microsoft.com/windows-hardware/drivers/stream/multiple-data-streams-on-the-same-hardware)

[将密钥交换与数据流同步](https://docs.microsoft.com/windows-hardware/drivers/stream/synchronizing-key-exchange-with-data-flow)

 

 







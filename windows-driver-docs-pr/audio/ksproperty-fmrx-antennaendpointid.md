---
title: KSPROPERTY\_FMRX\_ANTENNAENDPOINTID
description: KSTOPOLOGY\_ENDPOINTID 属性包含有关要用作调频广播天线的终结点的信息。
ms.assetid: 96B831E8-2372-413E-A9DE-63248F4F5863
keywords:
- KSPROPERTY_FMRX_ANTENNAENDPOINTID Audio Devices
topic_type:
- apiref
api_name:
- KSPROPERTY_FMRX_ANTENNAENDPOINTID
api_location:
- ksmedia.h
api_type:
- HeaderDef
ms.date: 11/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: 164dbd362665cb3ea9fadc964374bd7e438c6bb1
ms.sourcegitcommit: a33b7978e22d5bb9f65ca7056f955319049a2e4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "56548198"
---
# <a name="kspropertyfmrxantennaendpointid"></a>KSPROPERTY\_FMRX\_ANTENNAENDPOINTID


[ **KSTOPOLOGY\_ENDPOINTID** ](https://msdn.microsoft.com/library/windows/hardware/mt169886)属性包含有关要用作调频广播天线的终结点的信息。

## <a name="span-idusagesummarytablespanspan-idusagesummarytablespanspan-idusagesummarytablespanusage-summary-table"></a><span id="Usage_Summary_Table"></span><span id="usage_summary_table"></span><span id="USAGE_SUMMARY_TABLE"></span>使用率摘要表


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
<th align="left">Get</th>
<th align="left">设置</th>
<th align="left">目标</th>
<th align="left">属性描述符类型</th>
<th align="left">属性值类型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>是</p></td>
<td align="left"><p>否</p></td>
<td align="left"><p>Filter</p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/hardware/ff564262" data-raw-source="[&lt;strong&gt;KSPROPERTY&lt;/strong&gt;](https://msdn.microsoft.com/library/windows/hardware/ff564262)"><strong>KSPROPERTY</strong></a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/hardware/mt169886" data-raw-source="[&lt;strong&gt;KSTOPOLOGY_ENDPOINTID&lt;/strong&gt;](https://msdn.microsoft.com/library/windows/hardware/mt169886)"><strong>KSTOPOLOGY_ENDPOINTID</strong></a></p></td>
</tr>
</tbody>
</table>

 

属性值为类型[ **KSTOPOLOGY\_ENDPOINTID** ](https://msdn.microsoft.com/library/windows/hardware/mt169886)和指定的名称和与调频广播的拓扑终结点的 pin ID 接收天线。

## <a name="span-idreturnvaluespanspan-idreturnvaluespanspan-idreturnvaluespanreturn-value"></a><span id="Return_Value"></span><span id="return_value"></span><span id="RETURN_VALUE"></span>返回值


KSPROPERTY\_FMRX\_ANTENNAENDPOINTID 属性请求返回的名称和要用作调频广播天线的终结点的 pin id。

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>最低受支持的客户端</p></td>
<td align="left"><p>Windows 10</p></td>
</tr>
<tr class="even">
<td align="left"><p>最低受支持的服务器</p></td>
<td align="left"><p>Windows Server 2016</p></td>
</tr>
<tr class="odd">
<td align="left"><p>客户端</p></td>
<td align="left"><p>Windows 10 移动版</p></td>
</tr>
<tr class="even">
<td align="left"><p>标头</p></td>
<td align="left">Ksmedia.h</td>
</tr>
</tbody>
</table>

 

 





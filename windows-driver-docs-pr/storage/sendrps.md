---
title: SendRPS 函数
description: SendRPS WMI 方法将读取端口状态块（RPS）请求发送到指定的端口或域控制器。
ms.assetid: e8179a42-4095-4c59-81c5-7db7a2985939
keywords:
- SendRPS 函数存储设备
topic_type:
- apiref
api_name:
- SendRPS
api_location:
- Hbaapi.lib
- Hbaapi.dll
api_type:
- LibDef
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 3a62e8d584632efff2d27e972fa4fc0a65f85234
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838029"
---
# <a name="sendrps-function"></a>SendRPS 函数


**SendRPS** WMI 方法将读取端口状态块（RPS）请求发送到指定的端口或域控制器。

<a name="syntax"></a>语法
------

```ManagedCPlusPlus
void SendRPS(
   [out, HBA_STATUS_QUALIFIERS] HBA_STATUS       HBAStatus,
   [in, HBAType("HBA_WWN")] uint8                PortWWN[8],
   [in, HBAType("HBA_WWN")] uint8                AgentWWN[8],
   [in, HBAType("HBA_WWN")] uint8                ObjectWWN[8],
   [in] uint32                                   AgentDomain,
   [in] uint32                                   ObjectPortNumber,
   [out] uint32                                  TotalRspBufferSize,
   [out] uint32                                  ActualRspBufferSize,
   [out, WmiSizeIs("ActualRspBufferSize")] uint8 RspBuffer[]
);
```

<a name="parameters"></a>参数
----------

*HBAStatus*   
返回时，包含操作的状态。 有关允许值及其说明的列表，请参阅[HBA\_状态](hba-status.md)。 微型端口驱动程序在[**SendRPS\_OUT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_out)结构的**HBAStatus**成员中返回此信息。

*PortWWN*   
用于发送 RPS 命令的本地端口的全球名称。 此信息将传送到结构中[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)的**PortWWN**成员中的微型端口驱动程序。

*AgentWWN*   
端口的全球名称，将在该端口上查询*ObjectWWN*指示的端口状态。 此信息将传送到结构中[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)的**AgentWWN**成员中的微型端口驱动程序。

*ObjectWWN*   
要为其返回端口状态的端口的全球名称。 此信息将传送到结构中[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)的**ObjectWWN**成员中的微型端口驱动程序。

*AgentDomain*   
要查询*ObjectWWN*指示的端口状态的域控制器的域号。 此信息将传送到结构中[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)的**AgentDomain**成员中的微型端口驱动程序。

*ObjectPortNumber*   
要为其返回端口状态的端口的全球名称。 此信息将传送到结构中[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)的**ObjectPortNumber**成员中的微型端口驱动程序。

*TotalRspBufferSize*   
RPS 命令结果的大小（以字节为单位）。 微型端口驱动程序在[**SendRPS\_OUT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_out)结构的**TotalRspBufferSize**成员中返回此信息。

*ActualRspBufferSize*   
实际检索到的数据的大小（以字节为单位）。 微型端口驱动程序在[**SendRPS\_OUT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_out)结构的**ActualRspBufferSize**成员中返回此信息。

*RspBuffer*   
RPS 命令的结果。 微型端口驱动程序在[**SendRPS\_OUT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_out)结构的**RspBuffer**成员中返回此信息。

<a name="return-value"></a>返回值
------------

不适用于 WMI 方法。

<a name="remarks"></a>备注
-------

此 WMI 方法属于[MSFC\_HBAADAPTERMETHODS WMI 类](msfc-hbaadaptermethods-wmi-class.md)。

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>目标平台</p></td>
<td align="left">桌面设备</td>
</tr>
<tr class="even">
<td align="left"><p>标头</p></td>
<td align="left">Hbapiwmi （包括 Hbapiwmi、Hbaapi 或 Hbaapi）。</td>
</tr>
<tr class="odd">
<td align="left"><p>Library</p></td>
<td align="left">Hbaapi</td>
</tr>
</tbody>
</table>

## <a name="span-idsee_alsospansee-also"></a><span id="see_also"></span>另请参阅


[HBA\_状态](hba-status.md)

[**SendRPS\_** ](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_in)

[**SendRPS\_OUT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/hbapiwmi/ns-hbapiwmi-_sendrps_out)

 

 







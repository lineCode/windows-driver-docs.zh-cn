---
title: WDI_TLV_UNICAST_CIPHER_ALGO_LIST
description: WDI_TLV_UNICAST_CIPHER_ALGO_LIST 是一个 TLV，其中包含单播密码算法的列表。
ms.assetid: 67FAEE8A-1CD6-4430-92C1-84E9F43BEF63
ms.date: 07/18/2017
keywords:
- WDI_TLV_UNICAST_CIPHER_ALGO_LIST 从 Windows Vista 开始的网络驱动程序
ms.localizationpriority: medium
ms.openlocfilehash: 9278599b641c82bb7707e7d2749ff14f9d9b77f7
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72841719"
---
# <a name="wdi_tlv_unicast_cipher_algo_list"></a>WDI\_TLV\_单播\_密码\_算法\_列表


WDI\_TLV\_单播\_密码\_算法\_列表是包含单播密码算法列表的 TLV。

## <a name="tlv-type"></a>TLV 类型


0x3E

## <a name="length"></a>长度


WDI 数组的大小（以字节为单位） [ **\_密码\_算法**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wditypes/ne-wditypes-_wdi_cipher_algorithm)结构。 数组必须包含1个或多个元素。

## <a name="values"></a>值


| 在任务栏的搜索框中键入                                                            | 描述                            |
|-----------------------------------------------------------------|----------------------------------------|
| [**WDI\_密码\_算法**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wditypes/ne-wditypes-_wdi_cipher_algorithm)\[\] | 单播密码算法的数组。 |

 

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
<td><p>Windows 10</p></td>
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

 

 





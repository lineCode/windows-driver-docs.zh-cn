---
title: Bug 检查 0xF9 DRIVER_RETURNED_STATUS_REPARSE_FOR_VOLUME_OPEN
description: DRIVER_RETURNED_STATUS_REPARSE_FOR_VOLUME_OPEN bug 检查，它指示驱动程序返回的 STATUS_REPARSE 到 IRP_MJ_CREATE 请求，但没有尾随名称。
ms.assetid: 60eeb24a-accf-4db8-ba5b-1738a9aa4b46
keywords:
- Bug 检查 0xF9 DRIVER_RETURNED_STATUS_REPARSE_FOR_VOLUME_OPEN
- DRIVER_RETURNED_STATUS_REPARSE_FOR_VOLUME_OPEN
ms.date: 05/23/2017
topic_type:
- apiref
api_name:
- DRIVER_RETURNED_STATUS_REPARSE_FOR_VOLUME_OPEN
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: a3013fc6e6e65b69b86dce54c974690f38ef8bb6
ms.sourcegitcommit: 4bc550183bc403aee37e7aef2c38fecda1815bff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038096"
---
# <a name="bug-check-0xf9-driver_returned_status_reparse_for_volume_open"></a>Bug 检查0xF9：驱动\_返回\_状态\_重新分析\_\_卷\_打开


\_返回的驱动程序\_状态\_重新分析\_\_卷\_打开 bug 检查的值为0x000000F9。 这表明驱动程序返回状态\_重新分析为 IRP\_MJ\_CREATE request，无尾随名称。

> [!IMPORTANT]
> 本主题面向程序员。 如果你是在使用计算机时收到蓝屏错误代码的客户，请参阅[排查蓝屏错误](https://www.windows.com/stopcode)。


## <a name="driver_returned_status_reparse_for_volume_open-parameters"></a>驱动\_\_状态返回\_重分析\_\_卷\_打开参数


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>已打开的设备对象</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>向其发出 IRP_MJ_CREATE 请求的设备对象</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>包含文件的新名称的 Unicode 字符串的地址（要被重新分析）</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>驱动程序为 IRP_MJ_CREATE 请求返回的信息</p></td>
</tr>
</tbody>
</table>

 

<a name="remarks"></a>备注
-------

[ **！分析**](https://docs.microsoft.com/windows-hardware/drivers/debugger/-analyze)调试扩展显示有关 bug 检查的信息，可帮助确定根本原因。
仅应为 IRP\_MJ\_CREATE 带有尾随名称的请求返回\_重新分析状态，这表示该驱动程序支持名称空间。

 

 





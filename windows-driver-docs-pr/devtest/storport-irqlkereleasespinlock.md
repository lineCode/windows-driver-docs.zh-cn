---
title: IrqlKeReleaseSpinLock 规则 (storport)
description: 此规则将验证在 IRQL 调度时调用 KeReleaseSpinLock\_仅级别。 它还必须设置为以前的 IRQL 级别的 IRQL。 通常会通过调用 KeAcquireSpinLock 前面有此调用。
ms.assetid: A1AEE8C9-F5F1-4BBE-8291-1E61D73AFC6A
ms.date: 05/21/2018
keywords:
- IrqlKeReleaseSpinLock 规则 (storport)
topic_type:
- apiref
api_name:
- IrqlKeReleaseSpinLock
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: ba75362e68f5e154407b9d1b13487de7aca42a19
ms.sourcegitcommit: a33b7978e22d5bb9f65ca7056f955319049a2e4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "56525592"
---
# <a name="irqlkereleasespinlock-rule-storport"></a>IrqlKeReleaseSpinLock 规则 (storport)


此规则验证**KeReleaseSpinLock**在调用**IRQL = 调度\_级别**仅。 它还必须设置为以前的 IRQL 级别的 IRQL。 此调用通常会通过调用前面**KeAcquireSpinLock**。

|              |          |
|--------------|----------|
| 驱动程序模型 | Storport |

<a name="how-to-test"></a>如何测试
-----------

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">在编译时</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>运行<a href="https://msdn.microsoft.com/library/windows/hardware/ff552808" data-raw-source="[Static Driver Verifier](https://msdn.microsoft.com/library/windows/hardware/ff552808)">Static Driver Verifier</a>并指定<strong>IrqlKeReleaseSpinLock</strong>规则。</p>
使用以下步骤来分析你的代码：
<ol>
<li><a href="https://msdn.microsoft.com/library/windows/hardware/hh454281#preparing-your-source-code" data-raw-source="[Prepare your code (use role type declarations).](https://msdn.microsoft.com/library/windows/hardware/hh454281#preparing-your-source-code)">准备你的代码 （使用角色类型声明）。</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/hardware/hh454281#running-static-driver-verifier" data-raw-source="[Run Static Driver Verifier.](https://msdn.microsoft.com/library/windows/hardware/hh454281#running-static-driver-verifier)">运行的 Static Driver Verifier。</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/hardware/hh454281#viewing-and-analyzing-the-results" data-raw-source="[View and analyze the results.](https://msdn.microsoft.com/library/windows/hardware/hh454281#viewing-and-analyzing-the-results)">查看和分析结果。</a></li>
</ol>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/hardware/hh454281" data-raw-source="[Using Static Driver Verifier to Find Defects in Drivers](https://msdn.microsoft.com/library/windows/hardware/hh454281)">以找到缺陷驱动程序中使用 Static Driver Verifier</a>。</p></td>
</tr>
</tbody>
</table>

<a name="applies-to"></a>适用于
----------

[**KeReleaseSpinLock**](https://msdn.microsoft.com/library/windows/hardware/ff553145)
 

 





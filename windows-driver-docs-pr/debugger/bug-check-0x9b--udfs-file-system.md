---
title: Bug 检查 0x9B UDFS_FILE_SYSTEM
description: UDFS_FILE_SYSTEM bug 检查具有 0x0000009B 值。 此 bug 检查指示 UDF 文件系统中出现问题。
ms.assetid: cf20429d-6007-47e7-9faa-db7e1489e96b
keywords:
- Bug 检查 0x9B UDFS_FILE_SYSTEM
- UDFS_FILE_SYSTEM
ms.date: 05/23/2017
topic_type:
- apiref
api_name:
- UDFS_FILE_SYSTEM
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 08c39decebbfd46f5db69b54a2ebea53ca5555a8
ms.sourcegitcommit: d03b44343cd32b3653d0471afcdd3d35cb800c0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2019
ms.locfileid: "67519102"
---
# <a name="bug-check-0x9b-udfsfilesystem"></a>Bug 检查 0x9B：UDF\_文件\_系统


UDF\_文件\_检查系统错误的值为 0x0000009B。 此 bug 检查指示 UDF 文件系统中出现问题。

> [!IMPORTANT]
> 本主题面向程序员。 如果你已使用计算机时收到一个蓝色的屏幕，错误代码的客户，请参阅[疑难解答蓝屏错误](https://www.windows.com/stopcode)。


## <a name="udfsfilesystem-parameters"></a>UDF\_文件\_系统参数


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
<td align="left"><p>源文件和行号信息。 高 16 位 （"0x"后的前四个十六进制数字） 标识由其标识符编号的源代码文件。 低 16 位标识发生错误检查的文件中的源行。</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>如果<strong>UdfExceptionFilter</strong>是在堆栈上，此参数指定的地址的异常记录。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>如果<strong>UdfExceptionFilter</strong>是在堆栈上，此参数指定的上下文记录的地址。</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>保留。</p></td>
</tr>
</tbody>
</table>

 

<a name="cause"></a>原因
-----

UDF\_文件\_检查系统错误可能会导致磁盘损坏。 文件系统或磁盘上的坏扇区 （扇区） 中的损坏可导致此错误。 损坏的 SCSI 和 IDE 驱动程序可能还会影响系统的能够读取和写入到磁盘，导致该错误。

如果非分页缓冲的池内存已满，也可能出现此 bug 检查。 如果非分页缓冲的池内存已满时，此错误可以停止系统。 但是，在索引过程中，如果可用的非分页缓冲的池内存量为非常低，需要非分页缓冲的池的内存的另一个内核模式驱动程序还可以触发此错误。

<a name="resolution"></a>分辨率
----------

**若要调试此问题：** 使用[ **.cxr （显示上下文记录）** ](-cxr--display-context-record-.md)命令参数 3 中，并使用[ **kb （显示堆栈回溯）** ](k--kb--kc--kd--kp--kp--kv--display-stack-backtrace-.md)。

**若要解决磁盘损坏问题：** 检查事件查看器从 SCSI 和 FASTFAT （系统日志） 或 Autochk （应用程序日志） 可帮助标识设备或导致错误的驱动程序的错误消息。 禁用任何病毒扫描程序、 备份应用程序或持续监视系统的磁盘碎片整理程序工具。 您还应运行硬件诊断系统制造商提供。 有关这些过程的详细信息，请参阅您的计算机的用户手册。 运行**Chkdsk /f /r**检测和解决任何文件系统结构损坏。 系统分区上的磁盘扫描开始之前，必须重新启动系统。

**若要解决非分页缓冲的池内存耗尽问题：** 向计算机添加新的物理内存。 此内存增加了对内核可用的非分页缓冲的池内存的数量。

 

 





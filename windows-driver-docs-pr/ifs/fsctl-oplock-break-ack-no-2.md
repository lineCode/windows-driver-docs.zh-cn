---
title: FSCTL_OPLOCK_BREAK_ACK_NO_2 控制代码
description: FSCTL\_OPLOCK\_中断\_ACK\_NO\_2 控制代码对文件上的独占（级别1、批处理或筛选）机会锁（OPLOCK）的通知进行响应。
ms.assetid: f76c98ba-e0bf-4b86-bda4-92f233ea5839
keywords:
- FSCTL_OPLOCK_BREAK_ACK_NO_2 控制代码可安装的文件系统驱动程序
topic_type:
- apiref
api_name:
- FSCTL_OPLOCK_BREAK_ACK_NO_2
api_location:
- ntifs.h
api_type:
- HeaderDef
ms.date: 11/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: 5d5708eaadd9d44b99ec3af78f485b30dfc5e002
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72841275"
---
# <a name="fsctl_oplock_break_ack_no_2-control-code"></a>FSCTL\_OPLOCK\_中断\_ACK\_无\_控制代码


**FSCTL\_OPLOCK\_中断\_ACK\_no\_2**控制代码对文件上的独占（级别1、批处理或筛选）机会锁（OPLOCK）的通知进行响应。

客户端应用程序发送此控制代码以指示它将确认 oplock 中断，并且如果 oplock 是已分解为级别2的第1级 oplock，则不需要2级 oplock。

若要处理此控制代码，微筛选器将使用以下参数调用[**FltOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltoplockfsctrl) 。 文件系统或旧筛选器驱动程序调用[**FsRtlOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-_fsrtl_advanced_fcb_header-fsrtloplockfsctrl)。

有关机会锁定的详细信息，以及关于 FSCTL 的\_的详细信息\_中断\_确认\_没有\_2 控制代码，请参阅 Microsoft Windows SDK 文档。

**Parameters**

<a href="" id="oplock"></a>*机会*  
文件的不透明 oplock 对象指针。

<a href="" id="callbackdata"></a>*CallbackData*  
仅[**FltOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltoplockfsctrl) 。 IRP\_MJ\_文件\_SYSTEM\_控制 FSCTL 请求的回调数据（[**FLT\_回调\_数据**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/ns-fltkernel-_flt_callback_data)）结构。 操作的*FsControlCode*参数必须是 FSCTL\_OPLOCK\_BREAK\_ACK\_no\_2。

<a href="" id="irp"></a>*Irp*  
仅[**FsRtlOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-_fsrtl_advanced_fcb_header-fsrtloplockfsctrl) 。 Irp\_MJ\_文件\_系统\_控制 FSCTL 请求。 操作的*FsControlCode*参数必须是 FSCTL\_OPLOCK\_BREAK\_ACK\_no\_2。

<a href="" id="opencount"></a>*OpenCount*  
不与此操作一起使用;设置为零。

<a name="status-block"></a>状态块
------------

对于此操作， [**FltOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltoplockfsctrl)始终返回 FLT\_PREOP\_完成。

对于此操作， [**FsRtlOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-_fsrtl_advanced_fcb_header-fsrtloplockfsctrl)返回以下 NTSTATUS 值之一：

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">术语</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>STATUS_SUCCESS</strong></p></td>
<td align="left"><p>将确认 oplock 中断。 不保留剩余的 oplock。</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>STATUS_INVALID_OPLOCK_PROTOCOL</strong></p></td>
<td align="left"><p>此句柄未持有 oplock，或 oplock 中断当前未进行。 这是一个错误代码。</p></td>
</tr>
</tbody>
</table>

 

<a name="requirements"></a>要求
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>标头</p></td>
<td align="left">Ntifs （包括 Ntifs 或 Fltkernel）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另请参阅


[**FLT\_回调\_数据**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/ns-fltkernel-_flt_callback_data)

[**FLT\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/ns-fltkernel-_flt_parameters)

[**用于 IRP\_MJ\_文件\_系统\_控制的 FLT\_参数**](flt-parameters-for-irp-mj-file-system-control.md)

[**FltOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/fltkernel/nf-fltkernel-fltoplockfsctrl)

[**FSCTL\_OPBATCH\_ACK\_关闭\_挂起**](fsctl-opbatch-ack-close-pending.md)

[**FSCTL\_OPLOCK\_中断\_确认**](fsctl-oplock-break-acknowledge.md)

[**FSCTL\_OPLOCK\_中断\_通知**](fsctl-oplock-break-notify.md)

[**FSCTL\_请求\_批\_OPLOCK**](fsctl-request-batch-oplock.md)

[**FSCTL\_REQUEST\_FILTER\_OPLOCK**](fsctl-request-filter-oplock.md)

[**FSCTL\_请求\_OPLOCK\_级别\_1**](fsctl-request-oplock-level-1.md)

[**FSCTL\_请求\_OPLOCK\_级别\_2**](fsctl-request-oplock-level-2.md)

[**FsRtlOplockFsctrl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntifs/nf-ntifs-_fsrtl_advanced_fcb_header-fsrtloplockfsctrl)

[**IRP\_MJ\_文件\_系统\_控件**](irp-mj-file-system-control.md)

 

 







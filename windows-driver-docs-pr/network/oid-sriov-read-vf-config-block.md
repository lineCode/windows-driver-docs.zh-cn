---
title: OID_SRIOV_READ_VF_CONFIG_BLOCK
description: 过量驱动程序发出 OID_SRIOV_READ_VF_CONFIG_BLOCK 的对象标识符（OID）方法请求，以从指定的 PCI Express （PCIe）虚函数（VF）配置块中读取数据。
ms.assetid: A7AC7A18-5DA2-4EE8-B635-04616ABFE08C
ms.date: 08/08/2017
keywords: -从 Windows Vista 开始 OID_SRIOV_READ_VF_CONFIG_BLOCK 的网络驱动程序
ms.localizationpriority: medium
ms.openlocfilehash: 3ad76974bc7f1140279e63aaef1646449c840e16
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72843983"
---
# <a name="oid_sriov_read_vf_config_block"></a>OID\_SRIOV\_读取\_VF\_配置\_块


过量驱动程序发出 OID\_的对象标识符（OID）方法请求，\_读取\_VF\_CONFIG\_块读取指定 PCI Express （PCIe）虚函数（VF）配置块中的数据。

过量驱动程序将此 OID 方法请求颁发给网络适配器的 PCIe 物理功能（PF）的微型端口驱动程序。 对于支持单个根 i/o 虚拟化（SR-IOV）接口的 PF 小型端口驱动程序，此 OID 方法请求是必需的。

[ **\_OID 的 NDIS\_请求**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/ns-ndis-_ndis_oid_request)结构的**InformationBuffer**成员包含指向调用方分配的缓冲区的指针。 此缓冲区的格式设置为包含以下内容：

-   [**NDIS\_SRIOV\_读取\_VF\_CONFIG\_块**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)，其中包含偏移量（以字节为单位），其中包含偏移量（以字节为单位），其中包含从该结构的开始到包含从 VF 配置块读取的数据的缓冲区中的位置。\_

-   要从指定的 VF 配置块中读取的数据的额外缓冲区空间。

<a name="remarks"></a>备注
-------

VF 配置块用于 backchannel 和 VF 微型端口驱动程序之间的通信。 IHV 可以为微型端口驱动程序定义一个或多个 VF 配置块。 每个 VF 配置块都有一个 IHV 定义的格式、长度和块 ID。

**请注意**，每个 VF 配置块中  的数据仅供 PF 和 VF 微型端口驱动程序使用。

 

在\_SRIOV 的 OID 方法请求\_读取\_VF\_CONFIG\_块之前，过量驱动程序必须将[**NDIS\_SRIOV**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)的成员设置为\_\_\_\_\_

-   将**VFId**成员设置为要从中读取信息的 VF 的标识符。

-   将**块 id**成员设置为要从中读取信息的 VF 配置块的标识符。

-   将**Length**成员设置为要从配置块中读取的字节数。

-   将**BufferOffset**成员设置为缓冲区内的偏移量（由**InformationBuffer**成员引用），该偏移量将包含从指定的 VF 配置块中读取的数据。 此偏移量是以字节数为单位指定的，从[**NDIS\_SRIOV 的开始\_读取\_VF\_配置\_块\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)结构。

当它处理 OID\_SRIOV 的 OID 方法请求时\_读取\_VF\_配置\_块中，PF 微型端口驱动程序必须遵循以下准则：

-   PF 微型端口驱动程序必须验证由 NDIS\_SRIOV 的**VFId**成员指定的 VF [ **\_读取\_vf\_配置\_块\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)结构是否具有以前分配的资源。 在 oid 的 OID 方法请求中，PF 微型端口驱动程序为一个 VF 分配资源[\_NIC\_交换机\_分配\_VF](oid-nic-switch-allocate-vf.md)。 如果没有为指定的 VF 分配资源，则驱动程序必须使 OID 请求失败。

-   PF 微型端口驱动程序必须验证 NDIS\_SRIOV 的**块 id**成员[ **\_读取\_VF\_CONFIG\_BLOCK\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)结构指定有效的 VF 配置块。 否则，驱动程序必须使 OID 请求失败。

有关单根 i/o 虚拟化（SR-IOV）接口中的 backchannel 通信的详细信息，请参阅[SR-IOV PF/VF Backchannel 通信](https://docs.microsoft.com/windows-hardware/drivers/network/sr-iov-pf-vf-backchannel-communication)。

### <a name="return-status-codes"></a>返回状态代码

PF 多端口驱动程序返回 OID\_SRIOV\_读取\_VF\_CONFIG\_块的以下状态代码之一。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>状态代码</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>NDIS_STATUS_SUCCESS</p></td>
<td><p>OID 请求已成功完成。</p></td>
</tr>
<tr class="even">
<td><p>NDIS_STATUS_NOT_SUPPORTED</p></td>
<td><p>微型端口驱动程序不支持单根 i/o 虚拟化（SR-IOV）接口，或者未启用使用接口。</p></td>
</tr>
<tr class="odd">
<td><p>NDIS_STATUS_INVALID_PARAMETER</p></td>
<td><p><a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters" data-raw-source="[&lt;strong&gt;NDIS_SRIOV_READ_VF_CONFIG_BLOCK_PARAMETERS&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)"><strong>NDIS_SRIOV_READ_VF_CONFIG_BLOCK_PARAMETERS</strong></a>结构的一个或多个成员的值无效。</p></td>
</tr>
<tr class="even">
<td><p>NDIS_STATUS_INVALID_LENGTH</p></td>
<td><p>信息缓冲区太短。 微型端口驱动程序必须设置<strong>数据。METHOD_INFORMATION。</strong>将<a href="https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/ns-ndis-_ndis_oid_request" data-raw-source="[&lt;strong&gt;NDIS_OID_REQUEST&lt;/strong&gt;](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/ns-ndis-_ndis_oid_request)"><strong>NDIS_OID_REQUEST</strong></a>结构中的成员 BytesNeeded 为所需的最小缓冲区大小。</p></td>
</tr>
<tr class="odd">
<td><p>NDIS_STATUS_FAILURE</p></td>
<td><p>由于其他原因，请求失败。</p></td>
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
<td><p>版本</p></td>
<td><p>在 NDIS 6.30 和更高版本中受支持。</p></td>
</tr>
<tr class="even">
<td><p>标头</p></td>
<td>Ntddndis （包括 Ndis .h）</td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>另请参阅


****
[**NDIS\_OID\_请求**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ndis/ns-ndis-_ndis_oid_request)

[**NDIS\_SRIOV\_读取\_VF\_配置\_块\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/ntddndis/ns-ntddndis-_ndis_sriov_read_vf_config_block_parameters)

[OID\_NIC\_交换机\_分配\_VF](oid-nic-switch-allocate-vf.md)

[OID\_SRIOV\_读取\_VF\_配置\_空间](oid-sriov-read-vf-config-space.md)

 

 





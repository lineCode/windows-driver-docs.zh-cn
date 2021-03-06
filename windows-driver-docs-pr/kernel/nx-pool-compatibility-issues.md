---
title: NX 池兼容性问题
description: 当你在 Windows 8 的驱动程序二进制文件中使用 NX 非分页池时，如果在 Windows 的早期版本上运行这些二进制文件，则会发现兼容性问题。
ms.assetid: 652AE9A2-D733-4EC2-9D49-B95DDABE42B1
ms.localizationpriority: medium
ms.date: 10/17/2018
ms.openlocfilehash: 9a04248ecd7977ff47f7e836b4d148de7d8c145b
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72827793"
---
# <a name="nx-pool-compatibility-issues"></a>NX 池兼容性问题


当你在 Windows 8 的驱动程序二进制文件中使用 NX 非分页池时，如果在 Windows 的早期版本上运行这些二进制文件，则会发现兼容性问题。

Windows 8 是支持 NX 非分页池的第一个 Windows 版本。 但是，在 x86、x64 和 IA64 处理器体系结构上运行的 Windows 7 和更早版本的 Windows 中都提供了大量现有的内核模式驱动程序二进制文件。 若要分配非分页内存，这些驱动程序将改用 NX 非分页池。 为实现向后兼容性，在 Windows 7 上运行的内核模式驱动程序二进制文件以及某些较早版本的 Windows 上运行，并在未修改的情况下在 Windows 8 上运行。 但是，这些驱动程序不会利用 Windows 8 中的 NX 非分页池的可用性。

## <a name="running-existing-driver-binaries-on-windows8"></a>在 Windows 8 上运行现有的驱动程序二进制文件


不会阻止在 Windows 8 上运行的驱动程序二进制文件（也可能是早期版本的 Windows）和使用**非分页池**池类型的驱动程序二进制文件。 为了实现向后兼容性， **NonPagedPoolExecute**常数定义为与[**池中\_类型**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/ne-wdm-_pool_type)枚举的**非分页池**常量具有相同的值。 因此，在运行此驱动程序的任何 Windows 版本中，驱动程序从非分页池分配的内存始终是可执行的。

Windows 8 是 Windows 的第一个版本，用于支持 ARM 体系结构。 因此，没有为 Windows 早期版本生成的 ARM 的驱动程序二进制文件，需要向后兼容性。 相反，在 ARM 上为 Windows 编写的所有驱动程序都应在其非分页池分配中指定**NonPagedPoolNx**而不是**NonPagedPoolExecute** ，除非它们显式要求可执行内存。

如果将驱动程序从 x86、x64 或 IA64 移植到 ARM，则会在驱动程序生成过程中自动应用[\_NX\_OPTIN\_自动](multiple-binary-opt-in-pool-nx-optin-auto.md)选择加入机制的池。 默认情况下，此选择机制使用预处理器将**非分页池**常量名称的所有实例替换为**NonPagedPoolNx**。 如有必要，可以使用[\_NX 的池\_选择](selective-opt-out-pool-nx-optout.md)选择退出机制，在每个文件的基础上 overrride 此选择机制。

## <a name="other-compatibility-issues"></a>其他兼容性问题


从 Windows 8 开始支持**NonPagedPoolNx**池类型。 请勿在早期版本的 Windows 的驱动程序中使用此池类型。 当驱动程序请求具有**NonPagedPoolNx**池类型的分配时，这些早期版本的 Windows 中的池分配器不能正常运行。

在 Windows 8 之前的 Windows 版本中，可以自由地将**NonPagedPoolExecute**池类型用作**非分页池**池类型的替代项。 [**池\_类型**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/ne-wdm-_pool_type)枚举将**非分页池**和**NonPagedPoolExecute**定义为具有相同的值。

## <a name="nx-pool-type-porting-guidelines"></a>NX 池类型移植指导原则


将驱动程序代码从 Windows 的早期版本移植到 Windows 8 或更高版本时，有几种方法可以添加对**NonPagedPoolNx**和**NonPagedPoolExecute**池类型的支持。 从以下列表中，选择最适合你的要求的方法：

-   如果你的驱动程序不打算在早于 Windows 8 的 Windows 版本上运行，请将**非分页池**的大部分或全部实例替换为**NonPagedPoolNx**。 开发人员只需将**非分页池**的实例替换为**NonPagedPoolExecute。**

-   如果驱动程序源代码针对 Windows 8 和早期版本的 Windows，并为每个版本提供了不同的驱动程序二进制文件，请使用\_NX\_的池\_自动选择加入机制。 此方法不需要替换驱动程序源中的**非分页池**的实例。 有关详细信息，请参阅[NX 池选择加入机制](nx-pool-opt-in-mechanisms.md)。

-   如果驱动程序源代码针对 Windows 8 和早期版本的 Windows，并且你在所有受支持的版本上提供了一个要运行的驱动程序二进制文件，请使用\_NX 的池\_OPTIN 选择加入机制。 此方法不需要替换驱动程序源中的**非分页池**的实例。 有关详细信息，请参阅[NX 池选择加入机制](nx-pool-opt-in-mechanisms.md)。

通过这三种方法中的一种，可以快速移植大多数驱动程序，而不需要做很多工作。

避免只需用**NonPagedPoolExecute**替换驱动程序代码中的所有**非分页池**实例。 **NonPagedPoolExecute**池类型仅用于必须是可执行文件的池分配（例如，运行由实时或 JIT 编译器生成的代码）。

 

 





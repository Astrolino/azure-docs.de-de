---
title: Includedatei
description: Includedatei
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 09/24/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: dc03f49a684bfeb43ffd8bac9f551a67f034a04f
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042876"
---
## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA-Treiber

NVIDIA CUDA-Treiber für virtuelle Computer der Serien NC, NCv2, NCv3 und ND (optional für NV-Serie) werden nur unter den in der folgenden Tabelle aufgeführten Linux-Distributionen unterstützt. Die CUDA-Treiberinformationen sind zum Zeitpunkt der Veröffentlichung aktuell. Die aktuellsten CUDA-Treiber finden Sie auf der [NVIDIA](https://developer.nvidia.com/cuda-zone)-Website. Stellen Sie sicher, dass Sie eine Installation der bzw. ein Upgrade auf die neuesten CUDA-Treiber für Ihre Distribution ausgeführt haben. 

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Linux-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Ubuntu 16.04 LTS oder CentOS 7.4 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.

| Distribution | Treiber |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/> 7.3 oder 7.4 (CentOS-basiert), 7.4 HPC (CentOS-basiert) | NVIDIA CUDA 10.0, Treiberbranch R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle Computer der NV- und NVv2-Serie, die als virtuelle Arbeitsstationen oder für virtuelle Anwendungen verwendet werden. Installieren Sie nur diese GRID-Treiber auf Azure NV-VMs, und zwar nur unter den in der folgenden Tabelle aufgeführten Distributionen. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten.

| Distribution | Treiber |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 oder 7.4<br/><br/>7.3 oder 7.4 (CentOS-basiert) | NVIDIA GRID 6.2, Treiberbranch R390|



> [!WARNING] 
> Die Installation von Drittanbietersoftware auf Red Hat-Produkten kann Auswirkungen auf die Red Hat-Supportbedingungen haben. Weitere Informationen hierzu finden Sie im [Red Hat-Knowledgebase-Artikel](https://access.redhat.com/articles/1067).
>

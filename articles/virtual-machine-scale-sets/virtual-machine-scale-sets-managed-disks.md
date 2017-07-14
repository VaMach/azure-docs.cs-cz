---
title: "Použití spravovaných disků se škálovacími sadami virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Naučte se, kdy a jak používat spravované disky se škálovacími sadami virtuálních počítačů."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: f9cc157f25e70a8154614b659fb7e59b7fd06ed1
ms.contentlocale: cs-cz
ms.lasthandoff: 06/01/2017


---
# Škálovací sady virtuálních počítačů Azure a spravované disky
<a id="azure-vm-scale-sets-and-managed-disks" class="xliff"></a>

[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure teď podporují virtuální počítače se spravovanými disky. Použití spravovaných disků se škálovacími sadami má několik výhod:

* Pro virtuální počítače škálovacích sad už není potřeba předem vytvářet a spravovat účty úložiště pro ukládání disků operačního systému.

* Spravované datové disky můžete připojit ke škálovací sadě.

* Při použití spravovaného disku může mít škálovací sada kapacitu až 1 000 virtuálních počítačů, pokud je založená na imagi platformy, nebo 100 virtuálních počítačů, pokud je založená na vlastní imagi.

## Začínáme
<a id="get-started" class="xliff"></a>

Jednoduchým způsobem, jak začít pracovat se škálovacími sadami spravovaného disku, je provést nasazení z webu Azure Portal. Další informace najdete v [tomto článku](./virtual-machine-scale-sets-portal-create.md). Dalším jednoduchým způsobem, jak začít, je nasadit škálovací sadu pomocí [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2). Následující příklad ukazuje, jak vytvořit škálovací sadu založenou na Ubuntu s 10 virtuálními počítači, z nichž každý má 50GB a 100GB datový disk:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Další možností je vyhledat v [úložišti šablon Azure Quickstart na GitHubu](https://github.com/Azure/azure-quickstart-templates) složky, které obsahují `vmss`, a podívat se na předem vytvořené příklady šablon, které nasazují škálovací sady. Ke zjištění, které ze šablon už používají spravované disky, můžete použít [tento seznam](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## Verze rozhraní API
<a id="api-versions" class="xliff"></a>

Škálovací sady se spravovanými disky vyžadují rozhraní API Microsoft Compute ve verzi `2016-04-30-preview` nebo novější. Škálovací sady s nespravovanými disky budou dál fungovat stejně jako doteď, a to i v nových verzích rozhraní API, které mají podporu pro spravované disky. Škálovací sady s nespravovanými disky ale nebudou mít ani v těchto nových verzích rozhraní API výhody spravovaných disků.

## Další kroky
<a id="next-steps" class="xliff"></a>

Další obecné informace o spravovaných discích najdete v [tomto článku](../storage/storage-managed-disks-overview.md).

Postup převedení šablony Resource Manageru pro zřízení škálovací sady se spravovanými disky najdete v [tomto článku](./virtual-machine-scale-sets-convert-template-to-md.md). Stejné úpravy šablon Resource Manageru platí i pro rozhraní Azure REST API.

Další informace o použití spravovaných datových disků se škálovacími sadami najdete v [tomto článku](./virtual-machine-scale-sets-attached-disks.md).

Pokud chcete začít pracovat s rozsáhlými škálovacími sadami, přečtěte si [tento článek](./virtual-machine-scale-sets-placement-groups.md).




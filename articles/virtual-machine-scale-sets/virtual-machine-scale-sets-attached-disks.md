---
title: "Škálovací sady virtuálních počítačů Azure – připojené datové disky | Dokumentace Microsoftu"
description: "Naučte se používat připojené datové disky se škálovacími sadami virtuálních počítačů."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/6/2017
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 2939de432fcb5dbf4a7316343c3c6ae102b579e8
ms.openlocfilehash: 5b34969f9c854775587d402acbedee12f236ab7a


---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Škálovací sady virtuálních počítačů Azure a připojené datové disky
[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure teď podporují virtuální počítače s připojenými datovými disky. Datové disky můžete definovat v profilu úložiště pro škálovací sady vytvořené pomocí Azure Managed Disks. Dříve byly u virtuálních počítačů ve škálovacích sadách jedinou dostupnou možností přímo připojeného úložiště jednotka operačního systému a dočasné jednotky.

> [!NOTE]
>  Když vytvoříte škálovací sadu s definovanými připojenými datovými disky, před jejich použitím stále musíte disky připojit a naformátovat na virtuálním počítači (stejně jako u samostatných virtuálních počítačů Azure). Pohodlným způsobem, jak to provést, je použít rozšíření vlastních skriptů, které volá standardní skript a ten pak naformátuje a rozdělí do oddílů všechny datové disky na virtuálním počítači.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Vytvoření škálovací sady s připojenými datovými disky
Jednoduchým způsobem, jak vytvořit škálovací sadu s připojenými disky, je použít příkaz [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_. Následující příklad vytvoří skupinu prostředků Azure a škálovací sadu virtuálních počítačů s 10 virtuálními počítači s Ubuntu, z nichž každý bude mít 2 připojené datové disky o velikosti 50 a 100 GB.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Poznámka: příkaz _vmss create_ použije určité výchozí hodnoty konfigurace, pokud je nezadáte. Pokud chcete zobrazit dostupné možnosti, které můžete přepsat, vyzkoušejte:
```bash
az vmss create --help
```
Dalším způsobem, jak vytvořit škálovací sadu s připojenými datovými disky, je definovat škálovací sadu v šabloně Azure Resource Manageru, vložit část _dataDisks_ do profilu _storageProfile_ a nasadit šablonu. Výše uvedený příklad disků o velikosti 50 a 100 GB by se v šabloně definoval takto:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
Kompletní příklad šablony škálovací sady s nadefinovaným připojeným diskem připravený k nasazení najdete tady: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Přidání datového disku do existující škálovací sady
Datový disk můžete do škálovací sady virtuálních počítačů přidat pomocí příkazu Azure CLI _az vmss disk attach_. Ujistěte se, že zadáváte logickou jednotku (LUN), která se ještě nepoužívá. Následující příklad rozhraní příkazového řádku přidá 50GB disk k logické jednotce 3:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```
> [!NOTE]
> Různé velikosti virtuálních počítačů mají různá omezení počtu podporovaných připojených disků. Před přidáním nového disku zkontrolujte [charakteristiky velikostí virtuálních počítačů](../virtual-machines/virtual-machines-windows-sizes.md).

Disk můžete přidat také přidáním nového záznamu do vlastnosti _dataDisks_ v profilu _storageProfile_ definice škálovací sady a aplikováním změn. Pokud to chcete otestovat, vyhledejte existující definici škálovací sady v [Průzkumníku prostředků Azure](https://resources.azure.com/). Vyberte _Upravit_ a přidejte nový disk do seznamu datových disků. Například když použijeme výše uvedený příklad:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```
Potom vyberte _PUT_ a aplikujte změny na škálovací sadu. Tento příklad bude fungovat, pokud použijete velikost virtuálních počítačů, která podporuje více než dva připojené datové disky.

> [!NOTE]
> Když provedete změnu definice škálovací sady, například přidáním nebo odebráním datového disku, změna se aplikuje na všechny nově vytvořené virtuální počítače. Na existující virtuální počítače se ale aplikuje pouze pokud je vlastnost _upgradePolicy_ nastavena na Automatic. Pokud je nastavena na Manual, je třeba nový model na existující virtuální počítače aplikovat ručně. To můžete udělat na portálu, pomocí příkazu PowerShellu _Update-AzureRmVmssInstance_ nebo pomocí příkazu rozhraní příkazového řádku _az vmss update-instances_.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Odebrání datového disku ze škálovací sady
Datový disk můžete ze škálovací sady virtuálních počítačů odebrat pomocí příkazu Azure CLI _az vmss disk detach_. Například následující příkaz odebere disk definovaný na logické jednotce 2:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Podobně můžete odebrat disk ze škálovací sady také odebráním záznamu z vlastnosti _dataDisks_ v profilu _storageProfile_ a aplikováním změny. 

## <a name="additional-notes"></a>Další poznámky
Podpora služby Azure Managed Disks a připojených datových disků škálovacích sad byla přidána do rozhraní Microsoft.Compute API verze [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json). Můžete použít všechny sady SDK a nástroje příkazového řádku vytvořené s použitím této nebo novější verze rozhraní API.

Během počáteční implementace podpory připojených disků pro škálovací sady nelze datové disky připojit k jednotlivým virtuálním počítačům ve škálovací sadě, ani je od nich odpojit.

Podpora připojených datových disků ve škálovacích sadách je na webu Azure Portal zpočátku omezená. V závislosti na požadavcích můžete ke správě připojených disků použít šablony Azure, rozhraní příkazového řádku, PowerShell, sady SDK nebo rozhraní REST API.





<!--HONumber=Feb17_HO2-->



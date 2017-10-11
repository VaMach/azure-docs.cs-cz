---
title: "Vytvoření fondu služby Azure Batch události | Microsoft Docs"
description: "Referenční dokumentace pro fondu Batch vytvoření události."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 67edaa55d7ccd00d4aebb309f11bcf95486e87fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-create-event"></a>Vytvoření fondu události

 Tato událost je vygenerované po vytvoření fondu. Obsah protokolu zveřejní obecné informace o fondu. Všimněte si, že pokud cílovou velikost fondu je větší než 0 výpočetní uzly, bude následovat počáteční událost změny velikosti fondu ihned po této události.

 Následující příklad ukazuje vytvoření události pro fond vytvořili pomocí vlastnosti CloudServiceConfiguration text fondu.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Id fondu.|
|displayName|Řetězec|Zobrazovaný název fondu.|
|vmSize|Řetězec|Velikost virtuálního počítače ve fondu. Všechny virtuální počítače ve fondu mají stejnou velikost. <br/><br/> Informace o dostupných velikostí virtuálních počítačů pro Cloud Services najdete v části fondy (fondy vytvořené pomocí cloudServiceConfiguration), [velikosti pro cloudové služby](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch podporuje všechny velikosti virtuálních počítačů služby Cloud s výjimkou `ExtraSmall`.<br/><br/> Informace o dostupných virtuálních počítačů najdete v části velikosti pro fondy používat Image z Marketplace virtuálních počítačů (fondy vytvořené pomocí virtualMachineConfiguration) [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) nebo [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Služba Batch podporuje všechny velikosti VM Azure kromě `STANDARD_A0` a těch, které mají úložiště Premium (série `STANDARD_GS`, `STANDARD_DS` a `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Komplexní typ|Konfigurace služby cloud pro fond.|
|[virtualMachineConfiguration](#bk_vmconf)|Komplexní typ|Konfigurace virtuálního počítače pro fond.|
|[networkConfiguration](#bk_netconf)|Komplexní typ|Konfigurace sítě pro fond.|
|resizeTimeout|Čas|Časový limit pro přidělení výpočetní uzly fondu zadané pro poslední operace změny velikosti ve fondu.  (Počáteční dimenzování při vytváření fondu počítá jako změny velikosti.)|
|targetDedicated|Int32|Počet výpočetních uzlů, které jsou požadovány pro fond.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu automaticky přizpůsobí v čase.|
|enableInterNodeCommunication|BOOL|Určuje, zda je nastavení fondu pro přímou komunikaci mezi uzly.|
|isAutoPool|BOOL|Speficies jestli fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.|
|maxTasksPerNode|Int32|Maximální počet úloh, které můžou běžet současně na jednom výpočetním uzlu ve fondu.|
|vmFillType|Řetězec|Definuje způsob, jakým služba Batch distribuuje úkoly mezi výpočetní uzly ve fondu. Platné hodnoty jsou rozloženy nebo aktualizací Service Pack.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|atribut osFamily|Řetězec|Rodina Azure hostovaného operačního systému na virtuální počítače ve fondu.<br /><br /> Možné hodnoty:<br /><br /> **2** – operační systém řady 2, ekvivalentní na Windows Server 2008 R2 SP1.<br /><br /> **3** – operačního systému rodiny 3, ekvivalentní na Windows Server 2012.<br /><br /> **4** – 4 operačního systému rodiny, ekvivalentní na Windows Server 2012 R2.<br /><br /> Další informace najdete v tématu [verzí hostovaného operačního systému Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Řetězec|Verze Azure hostovaného operačního systému na virtuální počítače ve fondu.<br /><br /> Výchozí hodnota je  **\***  který určuje nejnovější verze operačního systému pro zadané řady.<br /><br /> Pro ostatní povolené hodnoty, najdete v části [verzí hostovaného operačního systému Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|[Element imageReference](#bk_imgref)|Komplexní typ|Určuje informace o platformu nebo Marketplace obrázek, který má použít.|
|nodeAgentSKUId|Řetězec|SKU agenta uzlu Batch zřídit na výpočetním uzlu.|
|[windowsConfiguration](#bk_winconf)|Komplexní typ|Určuje nastavení operačního systému Windows na virtuálním počítači. Tato vlastnost nesmí být zadán, pokud element imageReference odkazuje na bitovou kopii operačního systému Linux.|

###  <a name="bk_imgref"></a>Element imageReference

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|Vydavatele|Řetězec|Vydavatel bitovou kopii.|
|Nabídka|Řetězec|Nabídka obrázku.|
|SKU|Řetězec|SKU bitovou kopii.|
|Verze|Řetězec|Verze bitové kopie.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|enableAutomaticUpdates|Logická hodnota|Určuje, zda virtuální počítač povolena pro automatické aktualizace. Pokud není tato vlastnost určena, výchozí hodnota je true.|

###  <a name="bk_netconf"></a>networkConfiguration

|Název elementu|Typ|Poznámky|
|------------------|--------------|----------|
|subnetId|Řetězec|Určuje identifikátor prostředku podsítě, ve kterém jsou vytvořeny fondu výpočetních uzlů.|

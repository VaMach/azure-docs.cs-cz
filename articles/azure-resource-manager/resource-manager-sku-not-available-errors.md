---
title: "Azure SKU není k dispozici chyby | Microsoft Docs"
description: "Popisuje postupy řešení verze SKU není k dispozici došlo k chybě během nasazení."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: a6cccfa5097847429d3e402e3d522addc14b8c31
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="resolve-errors-for-sku-not-available"></a>Vyřešte chyby pro SKU není k dispozici

Tento článek popisuje, jak vyřešit **SkuNotAvailable** chyba.

## <a name="symptom"></a>Příznaky

Při nasazení prostředků (obvykle virtuální počítač), se zobrazí následující kód chyby a chybová zpráva:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Příčina

Tato chyba se zobrazí, když prostředek SKU, které jste vybrali (například velikost virtuálního počítače) není k dispozici pro umístění, které jste vybrali.

## <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, musíte určit, která SKU jsou k dispozici v oblasti. Operaci REST, na portálu nebo prostředí PowerShell můžete použít k vyhledání dostupné edice.

### <a name="solution-1"></a>Řešení 1

Použití [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) příkazu v prostředí PowerShell. Filtrovat výsledky podle umístění. Musíte mít nejnovější verzi prostředí PowerShell pro tento příkaz.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations -icontains "southcentralus"}
```

Výsledky budou zahrnovat seznam SKU pro umístění a nějaká omezení pro tento SKU.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

### <a name="solution-2"></a>Řešení 2

V rozhraní příkazového řádku Azure používat `az vm list-skus` příkaz. Pak můžete použít `grep` nebo podobného nástroje pro filtrování výstupu.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

### <a name="solution-3"></a>Řešení 3

Použití [portál](https://portal.azure.com). Přihlaste se k portálu a přidat prostředek přes rozhraní. Při nastavování hodnoty, uvidíte dostupné edice pro tento prostředek. Není nutné k dokončení nasazení.

![dostupné edice](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Řešení 4

Pomocí rozhraní REST API pro virtuální počítače. Odešlete následující požadavek:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Vrátí k dispozici SKU a oblastí, v následujícím formátu:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Pokud jste se nepodařilo najít vhodnou SKU v této oblasti nebo alternativní oblast, která splňuje vaše obchodní potřebuje, odeslání [SKU požadavek](https://aka.ms/skurestriction) pro podporu Azure.

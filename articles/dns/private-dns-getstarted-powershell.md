---
title: "Začínáme s privátními zónami DNS Azure pomocí PowerShellu | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit privátní zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první privátní zónu a záznam DNS pomocí PowerShellu."
services: dns
documentationcenter: na
author: garbrad
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/04/2017
ms.author: garbrad
ms.openlocfilehash: c5d4b33682f9b5d33e74d45973d5fbd76cf16465
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Začínáme s privátními zónami DNS Azure pomocí PowerShellu

Tento článek vás provede kroky k vytvoření první privátní zóny a záznamu DNS pomocí Azure PowerShellu.

K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Když chcete publikovat privátní zónu DNS do virtuální sítě, zadáte seznam virtuálních sítí, které mají povoleno překládat záznamy v rámci této zóny.  Označujeme je jako překladové sítě.  Můžete také zadat sadu virtuálních sítí, pro které bude DNS Azure uchovávat záznamy názvů hostitelů při každém vytvoření virtuálního počítače, změně jeho IP adresy nebo jeho zničení.  Označujeme je jako registrační sítě.

Protože je tato funkce aktuálně ve verzi Managed Preview, bude k dispozici modul PowerShellu ve verzi Preview.

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Před vytvořením zóny DNS se vytvoří skupina prostředků, která bude obsahovat zónu DNS. Následuje ukázka příkazu.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří pomocí rutiny `New-AzureRmDnsZone`. Následující příklad vytvoří zónu DNS *contoso.local* ve skupině prostředků *MyResourceGroup* a zpřístupní tuto zónu DNS pro virtuální síť *MyAzureVnet*. Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -ResolutionVirtualNetworkId @($vnet.Id)
```

Pokud potřebujete, aby platforma Azure v této zóně automaticky vytvářela záznamy názvů hostitelů, místo parametru *ResolutionVirtualNetworkId* použijte parametr *RegistrationVirtualNetworkId*.  U registračních virtuálních sítí je automaticky povolen překlad.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -RegistrationVirtualNetworkId @($vnet.Id)
```


## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Sady záznamů vytvoříte pomocí rutiny `New-AzureRmDnsRecordSet`. Následující příklad vytvoří záznam s relativním názvem db v zóně DNS contoso.local ve skupině prostředků MyResourceGroup. Plně kvalifikovaný název sady záznamů je db.contoso.local. Typ záznamu je A, IP adresa je 10.0.0.4 a hodnota TTL je 3 600 sekund.

```powershell
New-AzureRmDnsRecordSet -Name db -RecordType A -ZoneName contoso.local -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "10.0.0.4")
```

Informace o dalších typech záznamů, sadách záznamů s více než jedním záznamem a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí Azure PowerShellu](dns-operations-recordsets.md). 


## <a name="view-records"></a>Zobrazení záznamů

K výpisu záznamů DNS ve vaší zóně použijte:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyResourceGroup
```

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující krok:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Další kroky

Další informace o privátních zónách DNS najdete v tématu [Použití DNS Azure pro privátní domény](private-dns-overview.md).

Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS v DNS Azure pomocí PowerShellu](dns-operations-recordsets.md).


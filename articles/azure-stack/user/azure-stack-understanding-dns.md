---
title: "Seznámení s DNS v Azure zásobníku | Microsoft Docs"
description: "Princip funkce služby DNS a funkce v zásobníku Azure"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 86ed2805e93bd147841e22a773b52d1451f8c353
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introducing-idns-for-azure-stack"></a>Představení IDN pro Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

IDN je funkce v zásobníku Azure, která umožňuje překládat externí názvy DNS (například http://www.bing.com).
Také umožňuje registraci názvů interní virtuální síť. Díky tomu můžete vyřešit virtuální počítače ve stejné virtuální síti název místo IP adresy, bez nutnosti poskytnout vlastní položky serveru DNS.

Je něco, co byla vždy existuje v Azure, ale je k dispozici v systému Windows Server 2016 a zásobník Azure příliš.

## <a name="what-does-idns-do"></a>Jakým způsobem IDN?
S IDN v zásobníku Azure získáte následující možnosti, aniž byste museli zadat vlastní položky serveru DNS:

* Sdílené služby překladu názvů DNS pro úlohy klientů.
* Autoritativní služby DNS pro rozlišení názvů a registraci DNS v rámci virtuální sítě klienta.
* Služba DNS rekurzivní pro překlad názvů v síti Internet z klientské virtuální počítače. Klienti se již muset zadat vlastní záznamy DNS pro překlad názvů v Internetu (například www.bing.com).

Můžete stále přineste si vlastní DNS a pokud chcete použít vlastní servery DNS. Ale nyní, pokud jste právě chcete umět překládat názvy DNS pro Internet a být schopni se připojit k jiné virtuální počítače ve stejné virtuální síti, nemusíte nic zadejte a bude prostě fungovat.

## <a name="what-does-idns-not-do"></a>Co IDN neprovádí?
Jaké IDN neumožňuje musíte udělat, je vytvořit záznam DNS pro název, který lze vyřešit z mimo virtuální síť.

V Azure máte možnost zadat popisek názvu DNS, který může být spojen s veřejnou IP adresu. Můžete zvolit popisek (předpona), ale Azure vybere příponu, která je založena na oblast, ve kterém vytvoříte veřejnou IP adresu.

![Popisek názvu DNS z snímek obrazovky](media/azure-stack-understanding-dns-in-tp2/image3.png)

Na předchozím obrázku, Azure vytvoří "A" záznamu DNS pro popisek názvu DNS, který je zadán v rámci zóny **westus.cloudapp.azure.com**. Předpona a přípona společně tvoří plně kvalifikovaný domény název (FQDN), můžete vyřešit kdekoli na veřejného Internetu.

Azure zásobníku IDN podporuje pouze pro interní název registraci, proto nelze provést následující:

* Vytvořte záznam DNS v rámci stávající hostované zónu DNS (například local.azurestack.external).
* Vytvoření zóny DNS (například Contoso.com).
* Vytvořte záznam v části vlastní zónu DNS.
* Podpora nákupu názvů domén.


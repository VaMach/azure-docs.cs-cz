---
title: "Seznámení s DNS v Azure zásobníku | Microsoft Docs"
description: "Princip funkce služby DNS a funkce v zásobníku Azure"
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 60f5ac85-be19-49ac-a7c1-f290d682b5de
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 8c023eda179ace41a082bf4a4fadc281c14db7ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-idns-for-azure-stack"></a>Představení IDN pro Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

IDN je funkce v zásobníku Azure, která umožňuje překládat externí názvy DNS (například http://www.bing.com).
Také umožňuje registraci názvů interní virtuální síť. Díky tomu můžete vyřešit virtuální počítače ve stejné virtuální síti název místo IP adresy, bez nutnosti poskytnout vlastní položky serveru DNS.

Je něco, co byla vždy existuje v Azure, ale je k dispozici v systému Windows Server 2016 a zásobník Azure příliš.

## <a name="what-does-idns-do"></a>Jakým způsobem IDN?
S IDN v zásobníku Azure získáte následující možnosti, aniž byste museli zadat vlastní položky serveru DNS.

* Sdílené služby překladu názvů DNS pro úlohy klientů.
* Autoritativní služby DNS pro rozlišení názvů a registraci DNS v rámci virtuální sítě klienta.
* Služba DNS rekurzivní pro překlad názvů v síti Internet z klientské virtuální počítače. Klienti se již muset zadat vlastní záznamy DNS pro překlad názvů v Internetu (například www.bing.com).

Můžete stále přineste si vlastní DNS a pokud chcete použít vlastní servery DNS. Ale nyní, pokud jste právě chcete umět překládat názvy DNS pro Internet a být schopni se připojit k jiné virtuální počítače ve stejné virtuální síti, nemusíte nic zadejte a bude prostě fungovat.

## <a name="what-does-idns-not-do"></a>Co IDN neprovádí?
Jaké IDN neumožňuje musíte udělat, je vytvořit záznam DNS pro název, který lze vyřešit z mimo virtuální síť.

V Azure máte možnost zadat popisek názvu DNS, který může být spojen s veřejnou IP adresu. Můžete zvolit popisek (předpona), ale Azure vybere příponu, která je založena na oblast, ve kterém vytvoříte veřejnou IP adresu.

![Popisek názvu DNS z snímek obrazovky](media/azure-stack-understanding-dns-in-tp2/image3.png)

Na předchozím obrázku, Azure vytvoří "A" záznamu DNS pro popisek názvu DNS, který je zadán v rámci zóny **westus.cloudapp.azure.com**. Předpona a přípona společně tvoří plně kvalifikovaný domény název (FQDN), můžete vyřešit kdekoli na veřejného Internetu.

Azure zásobníku IDN podporuje pouze pro interní název registraci, proto nelze provést následující.

* Vytvořte záznam DNS v rámci stávající hostované zónu DNS (například local.azurestack.external).
* Vytvoření zóny DNS (například Contoso.com).
* Vytvořte záznam v části vlastní zónu DNS.
* Podpora nákupu názvů domén.


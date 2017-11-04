---
title: "Zobrazit veřejnou IP adresu spotřebu v zásobníku Azure | Microsoft Docs"
description: "Správci mohou prohlížet spotřeby veřejné IP adresy v oblasti."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 7651565eebf6272f307a4ce4790ca19b41bfa826
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Zobrazení veřejnou IP adresu energie v zásobníku Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Jako správce cloudu můžete zobrazit počet veřejné IP adresy, které byly přiděleny na klienty, počet veřejné IP adresy, které jsou stále k dispozici pro přidělení a procento veřejné IP adresy, které byly přiděleny v tomto umístění.

**Veřejnou IP adresu do fondu využití** dlaždice zobrazuje celkový počet veřejné IP adresy, které bylo spotřebováno přes všechny veřejné fondy IP adres v prostředcích architektury, zda byly použity pro instance virtuálních počítačů IaaS, klienta prostředky infrastruktury služby nebo veřejných IP adres prostředky, které byly vytvořeny explicitně klienty.

Účelem této dlaždice je a poskytuje správcům zásobník Azure představu o celkový počet veřejné IP adresy, které bylo spotřebováno v tomto umístění. To pomáhá správcům určit, zda běží nízkou u tohoto prostředku.

Na **zprostředkovatelé prostředků**, **sítě** okně **veřejné IP adresy** položky nabídky v části **prostředky klienta** uvádí jenom tyto veřejné IP adresy, které byly *explicitně vytvořené klienty*. Jako například počet **používá** veřejné IP adresy na **veřejnou IP adresu do fondu využití** dlaždice se liší od (větší než) vždy na číslo **veřejné IP adresy** dlaždici v části **klienta prostředky**.

## <a name="view-the-public-ip-address-usage-information"></a>Zobrazení informací o veřejné IP adresy používání
Chcete-li zobrazit celkový počet veřejné IP adresy, které bylo spotřebováno v oblasti:

1. Na portálu Azure zásobníku správce, klikněte na tlačítko **další služby**v části **prostředky pro správu**, klikněte na tlačítko **zprostředkovatelé prostředků**.
2. Ze seznamu **zprostředkovatelé prostředků**, vyberte **sítě**.
3. **Sítě** zobrazuje **veřejnou IP adresu do fondu využití** dlaždice v nástroji **přehled** části.

![Okno poskytovatele prostředků sítě](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Mějte na paměti, že **používá** číslo představuje počet veřejných IP adres z všechny veřejné fondy IP adres v daném umístění přiřazené. **Volné** číslo představuje počet veřejných IP adres ze všech veřejných IP adres, fondy, které nebyly přiřazeny a jsou stále k dispozici. **% Používá** číslo představuje počet použít nebo přiřazené adresy jako procento z celkového počtu veřejné IP adresy v všechny veřejné fondy IP adres v daném umístění.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Zobrazit veřejné IP adresy, které byly vytvořeny odběry klienta
Chcete-li zobrazit seznam veřejné IP adresy, které byly explicitně vytvořeny odběry klienta v určité oblasti, klikněte na tlačítko **veřejné IP adresy** pod **prostředky klienta**.

![Veřejné IP adresy klienta](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Můžete si všimnout, že některé veřejné IP adresy, které byly přiděleny dynamicky zobrazí v seznamu, ale nemají adresu ještě s nimi spojených. Je to proto, že prostředek adresy zatím nebyla vytvořena v poskytovatele síťových prostředků, ale není v síťovém adaptéru.

Síťový adaptér nepřiřazuje adresu pro tento prostředek, dokud ve skutečnosti je vázána na rozhraní, síťová karta (NIC), nástroj pro vyrovnávání zatížení nebo brány virtuální sítě. Když veřejná IP adresa je vázána na rozhraní, síťový adaptér přidělí IP adresu do něj a zobrazí se v **adresu** pole.

## <a name="view-the-public-ip-address-information-summary-table"></a>Zobrazení veřejnou IP adresu informace tabulku souhrnu
Existuje několik různých případů ve kterých jsou přiřazeny veřejné IP adresy, které určují, zda adresa se zobrazí v seznamu jednoho nebo druhého.

| **Veřejná IP adresa přiřazení případu** | **Zobrazí se v souhrnu využití** | **Zobrazí se v seznamu klienta veřejné IP adresy** |
| --- | --- | --- |
| Dynamické veřejnou IP adresu dosud není přiřazen k seskupování nebo službu Vyrovnávání zatížení (dočasný) |Ne |Ano |
| Dynamické veřejnou IP adresu přiřazené seskupování nebo službu Vyrovnávání zatížení. |Ano |Ano |
| Statickou veřejnou IP adresu se přiřadila klienta síťový adaptér nebo službu Vyrovnávání zatížení. |Ano |Ano |
| Statickou veřejnou IP adresu přiřadit pro koncový bod služby infrastruktury prostředků infrastruktury. |Ano |Ne |
| Veřejná IP adresa implicitně vytvořené pro instance virtuálních počítačů IaaS a používat pro odchozí NAT ve virtuální síti. Tyto soubory jsou vytvořeny na pozadí vždy, když klient vytvoří instanci virtuálního počítače tak, aby virtuální počítače můžete odesílat informace k Internetu. |Ano |Ne |

## <a name="next-steps"></a>Další kroky
[Správa účtů úložiště v Azure zásobníku](azure-stack-manage-storage-accounts.md)
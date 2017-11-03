---
title: "Azure doporučení služby Advisor vysokou dostupnost | Microsoft Docs"
description: "Pomocí Azure Advisor lze zvýšit vysokou dostupnost vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7b63de2453180e562596c211d40cebe85b95bd54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-high-availability-recommendations"></a>Doporučení pro vysokou dostupnost služby Advisor

Azure Advisor pomáhá zajistit, a zlepšování kontinuity důležitými obchodními aplikacemi. Můžete získat vysokou dostupnost doporučení Advisor z **vysokou dostupnost** Advisor řídicího panelu.

![Tlačítko vysoké dostupnosti na řídicím panelu Advisor](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Zajištění odolnosti proti chybám virtuálního počítače

Advisor identifikuje virtuálních počítačů, které nejsou součástí skupiny dostupnosti a doporučuje jejich přesunutí do skupiny dostupnosti. Pokud chcete zajistit redundanci pro vaši aplikaci, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač je k dispozici a splňuje virtuální počítač Azure SLA. Můžete si vybrat, chcete-li vytvořit sadu dostupnosti pro virtuální počítač nebo virtuální počítač přidat do stávající sadu dostupnosti.

> [!NOTE]
> Pokud se rozhodnete vytvořit skupinu dostupnosti, je nutné přidat alespoň jeden další virtuální počítač do ní. Doporučujeme, které jsou seskupeny dvě nebo více virtuálních počítačů ve skupině dostupnosti nastavena zajistit, aby aspoň jeden počítač je k dispozici během výpadku.

![Advisor doporučení: redundanci virtuálního počítače, použijte nastavení dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Ujistěte se, že odolnost proti chybám sadu dostupnosti 

Advisor určí skupiny dostupnosti, které obsahují jeden virtuální počítač a doporučuje přidání do jednoho nebo více virtuálních počítačů. Pokud chcete zajistit redundanci pro vaši aplikaci, doporučujeme seskupit dva nebo více virtuálních počítačů do skupiny dostupnosti. Tato konfigurace zajistí, že během buď plánované i neplánované údržby alespoň jeden virtuální počítač je k dispozici a splňuje virtuální počítač Azure SLA. Můžete buď vytvořit virtuální počítač nebo použít existující virtuální počítač a tím ho přidáte do skupiny dostupnosti.  

![Advisor doporučení: Přidejte jednu nebo více virtuálních počítačů do této skupiny dostupnosti](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Ujistěte se, aplikace brány odolnost proti chybám
Zajištění kontinuity obchodních procesů důležitých aplikací, které jsou zapnuté nástrojem application Gateway, Advisor identifikuje instance brány aplikace, kteří nejsou nakonfigurovaní pro odolnost proti chybám, a navrhování nápravné akce, které můžete provést. Advisor určí střední a velké aplikace s jedinou instancí brány a doporučí přidat nejméně jedna další instance. Také určí jeden nebo více instance malé application Gateway a doporučuje migrace na střední a velké jednotky SKU. Advisor doporučuje tyto akce se ujistíte, že instance brány vaší aplikace nakonfigurovaná vyhovět požadavkům SLA pro tyto prostředky.

![Advisor doporučení: nasazení dvou nebo více instancí brány střední a velké velikostí aplikace](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Zvýšení výkonu a spolehlivosti disky virtuálního počítače

Advisor určí virtuálních počítačů s standardní disky a doporučuje upgrade na prémiové disky.
 
Azure Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače spustit I náročnými úlohy. Disky virtuálního počítače, které používají účty úložiště premium ukládat data do disků SSD (Solid-State Drive). Nejlepšího výkonu pro vaši aplikaci doporučujeme migraci všechny disky virtuálního počítače, které vyžadují vysokou IOPS pro storage úrovně premium. 

Pokud vaše disky nevyžadují vysokou IOPS, můžete omezit náklady na údržbu je v standardní úložiště. Standardní úložiště ukládá data disku virtuálního počítače na jednotkách pevných disků (HDD) namísto SSD. Můžete migrovat disky virtuálního počítače do prémiové disky. Pro prémiové disky jsou podporovány na virtuálním počítači většina SKU. Ale v některých případech, pokud chcete použít prémiové disky, může musíte upgradovat virtuální počítač SKU také.

![Advisor doporučení: upgrade na prémiové disky vylepšit spolehlivost disky virtuálního počítače](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Virtuální počítač data chránit před nechtěným odstraněním
Advisor identifikuje virtuální počítače, kde není povolené zálohování, a doporučí povolení zálohování. Nastavení zálohování virtuálních počítačů zajistíte dostupnost důležitých podnikových dat a poskytuje ochranu před náhodným odstraněním nebo poškození.

![Advisor doporučení: Konfigurace zálohování virtuálních počítačů k ochraně důležitých dat](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Přístup k zajištění vysoké dostupnosti doporučení v Advisor

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V levém podokně klikněte na **další služby**.

3. V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 Se zobrazí řídicí panel služby Advisor.

4. Na řídicím panelu služby Advisor, klikněte na **vysokou dostupnost** kartě a potom vyberte předplatné, pro který chcete dostávat doporučení.

> [!NOTE]
> Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:
* [Úvod do Azure Advisor](advisor-overview.md)
* [Začínáme se službou Advisor](advisor-get-started.md)
* [Náklady na doporučení služby Advisor](advisor-performance-recommendations.md)
* [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)


---
title: "Zobrazit měsíční náklady na trend odhadované testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o graf trendů Azure DevTest Labs měsíční odhadované náklady."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Zobrazit měsíční náklady na trend odhadované testovacího prostředí v Azure DevTest Labs
Náklady na správu funkce DevTest Labs vám pomůže sledovat náklady na vašem testovacím prostředí. Tento článek ukazuje, jak používat **měsíční odhadované náklady Trend** grafu zobrazíte aktuální kalendářní měsíc odhadované náklady na začátku a předpokládané náklady na konci měsíce pro aktuální měsíc kalendáře. V tomto článku zjistěte, jak chcete-li zobrazit měsíční graf trendů odhadované náklady na portálu Azure.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Zobrazení grafu měsíční odhadované náklady trendů
Chcete-li zobrazit graf měsíční odhadované náklady trendů, postupujte takto: 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
3. Ze seznamu labs vyberte požadované testovací prostředí.   
4. V okně v prostředí, vyberte **náklady nastavení**.
5. V tomto prostředí **náklady nastavení** vyberte **trend náklady na testovacím**.
6. Následující snímek obrazovky ukazuje příklad grafu náklady. 
   
    ![Cenově grafu](./media/devtest-lab-configure-cost-management/graph.png)

**Odhadované náklady na** hodnota je aktuální kalendářní měsíc odhadované náklady na začátku. **Plánované náklady** je odhadované náklady pro celou aktuální kalendářní měsíc vypočítává pomocí náklady na testovacím předchozí pět dní.

Objemy náklady jsou zaokrouhlený nahoru na nejbližší celé číslo. Například: 

* 5.01 zaokrouhlí až 6 
* 5.50 zaokrouhlí až 6
* 5.99 zaokrouhlí až 6

Jak je uvedeno výše grafu, náklady, se zobrazí v grafu jsou *odhadované* stojí pomocí [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) nabízejí sazby.
Kromě toho jsou následující *není* zahrnutých do výpočtu náklady na:

* Předplatné Dreamspark a CSP nejsou aktuálně podporovány jako používá Azure DevTest Labs [rozhraní API Azure fakturace](../billing/billing-usage-rate-card-overview.md) vypočítat náklady, který nepodporuje zprostředkovatele kryptografických služeb nebo Dreamspark odběry testovací prostředí.
* Nabídka sazby. V současné době Nedokážeme používat sazby nabídka (zobrazené v rámci svého předplatného), že máte vyjedná se společnosti Microsoft nebo Microsoft partnery. Používáme průběžné platby sazby.
* Vaše daně
* Vaše slevy
* Vaše fakturační Měna. V současné době náklady na testovacího prostředí se zobrazí jenom v USD měny.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Dva kroky zachovat vaše náklady na sledování v DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Proč náklady prahové hodnoty?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Další kroky
Zde jsou další vyzkoušejte:

* [Definování zásad testovacího](devtest-lab-set-lab-policy.md) – zjistěte, jak nastavit různé zásady použité k řízení použití testovacího prostředí a jeho virtuální počítače. 
* [Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace. Tento článek ukazuje, jak vytvořit vlastní obrázek ze souboru virtuálního pevného disku.
* [Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace. Tento článek ukazuje, jak určit, které, pokud existuje, může být Azure Marketplace Image používá při vytváření virtuálních počítačů v testovacím prostředí.
* [Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -znázorňuje, jak vytvořit virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a postup práce s artefakty ve vašem virtuálním počítači.


---
title: "Řízení nákladů pomocí Azure náklady na správu | Microsoft Docs"
description: "Náklady na správu pomocí náklady přidělení a kompletní přehled nákladů a sestav vracení peněz."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/21/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: bfbcded98814500a03b2b79b0248c84f8f043dc0
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Řízení nákladů pomocí Azure náklady na správu

Řízení nákladů a vytváření sestav kompletní přehled nákladů v Azure náklady na správu Cloudyn přidělí poplatkům za značky. Proces přidělení nákladů přiřadí náklady pro vaše prostředky spotřebované cloudu. Náklady jsou plně přiřazený při všechny prostředky, které jsou klasifikovány pomocí značek. Po náklady jsou přiděleny, můžete poskytovat kompletní přehled nákladů nebo vrácení peněz uživatelům s řídicí panely a sestavy. Však množství prostředků můžou vyžadující nebo untaggable když začnete používat náklady na správu.

Například můžete chtít získat vráceny pro engineering náklady. Musíte být schopni zobrazit technického týmu, které potřebujete určitou velikostí, na základě nákladů prostředků. Je možné zobrazit sestavy pro všechny prostředky spotřebované, které jsou označené *engineering*.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Pomocí vlastních značek přidělit náklady.
> * Vytvoření kompletní přehled nákladů a sestav vracení peněz.

## <a name="use-custom-tags-to-allocate-costs"></a>Pomocí vlastních značek přidělení náklady

Když spustíte přidělení nákladů, je první věcí, které můžete provést je definování oboru pomocí modelu náklady. Model náklady nemění náklady, distribuuje je. Když vytvoříte model náklady, segmentovat se vaše data entity náklady, účet nebo předplatné a více značek. Společné Příklad značky mohou zahrnovat kód pro účtování, nákladové středisko nebo název skupiny. Značky také můžete provádět kompletní přehled nákladů nebo vrácení peněz do dalších částí vaší organizace.

Pro vytvoření modelu přidělení vlastní náklady, vyberte **náklady** &gt; **náklady na správu** &gt; **přidělení nákladů 360°** v nabídce sestavy.

![Výběr náklady 360 přidělení](./media/tutorial-manage-costs/cost-allocation-360.png)

Na **náklady přidělení 360** vyberte **přidat** a pak zadejte název a popis pro váš model náklady. Vyberte všechny účty nebo jednotlivé účty. Pokud chcete používat samostatné účty, můžete vybrat více účtů z více poskytovatelů cloudových služeb. Klikněte na tlačítko **kategorizaci** zvolit zjištěných značky, které kategorizace dat náklady. Zvolte značky (kategorie), které chcete zahrnout do modelu. V následujícím příkladu **jednotky** značky je vybrána.

![Příklad nákladů modelu kategorizaci](./media/tutorial-manage-costs/cost-model01.png)



Příklad ukazuje, že je tento 14,444 $ Nezařazeno do kategorie (bez značek).

Potom vyberte **nezařazené prostředky** a vyberte služby, které mají volné náklady. Poté definujte pravidla přidělit náklady.

Můžete například provést náklady na úložiště Azure a distribuovat náklady na stejnou měrou na virtuálních počítačích Azure (VM). To pokud chcete udělat, vyberte **úložiště Azure** služby, vyberte **úměrná Categorized**a potom vyberte **virtuální počítač Azure nebo**. Pak vyberte **vytvořit**.

![Příklad nákladů modelu přidělení pravidlo pro rovnoměrně](./media/tutorial-manage-costs/cost-model02.png)



V různých třeba můžete přidělit všechny náklady na síť Azure konkrétní organizační jednotky ve vaší organizaci. Chcete-li to provést, vyberte **Azure a síťových** služby a potom vyberte **explicitní distribuční**. Poté nastavit pro rozdělování do 100 a vyberte organizační jednotku –**G&amp;A** na následujícím obrázku:

![Příklad nákladů modelu přidělení pravidlo pro konkrétní organizační jednotky](./media/tutorial-manage-costs/cost-model03.png)



Pro všechny zbývající nezařazené prostředky vytvořte další přidělení pravidla.

Pokud máte všechny nepřidělené instance vyhrazené Amazon Web Services (AWS), můžete je přiřadit k s příznakem kategorie s **vyhrazenou instancí**.

Chcete-li zobrazit informace o volby, které jste provedli přidělit náklady, vyberte **Souhrn**. Chcete-li pokračovat v práci s další pravidla později a ukládat informace, vyberte **uložit jako koncept**. Nebo pokud chcete ukládat informace a mít Cloudyn zahájení zpracování modelu přidělení náklady, vyberte **uložit a aktivujte**.

Zobrazuje seznam modelů náklady na nové náklady na modelu pomocí **zpracování stavu**. Může trvat nějakou dobu, než aktualizaci databáze Cloudyn modelu náklady. Po dokončení zpracování k aktualizaci stavu **dokončeno**. Potom můžete zobrazit data z modelu náklady na v sestavě analýza nákladů v **rozšířené filtry** &gt; **náklady modelu**.

### <a name="category-manager"></a>Správce kategorie

Kategorie Manager je nástroj Vyčištění dat, který umožňuje sloučit více kategorií (značky) k vytvoření nové hodnoty. Je jednoduchý nástroj založený na pravidlech kde vyberte kategorii a vytvářet pravidla pro sloučení existující hodnoty. Například můžete mít existující kategorie pro **R&amp;D** a **dev** kde obě představují vývojářské skupině.

V portálu Cloudyn, klikněte na symbol ozubené kolečko v horním pravém rohu a vyberte **správce kategorie**. Chcete-li vytvořit novou kategorii, vyberte plus symbol (**+**). Zadejte název pro kategorii a pak v části **klíče**, zadejte kategorie klíčů, které chcete zahrnout do nové kategorie.

Když definujete pravidlo, můžete přidat více hodnot s podmínkou nebo. Můžete také provést některé základní operace s řetězci. V obou případech kliknutím na symbol tří teček (**...** ) napravo od **pravidlo**.

Chcete-li definovat nové pravidlo v **pravidla** oblasti, vytvořit nové pravidlo. Zadejte například **dev** pod **pravidla** a pak zadejte **R&amp;D** pod **akce**. Když jste hotovi, uložte novou kategorii.

Následující obrázek ukazuje příklad pravidel vytvořených pro novou kategorii s názvem **pracovní zatížení**:

![Příklad kategorie](./media/tutorial-manage-costs/category01.png)

### <a name="tag-sources-and-reports"></a>Značka zdroje a sestavy

Data značek, která se zobrazí v sestavách Cloudyn pochází na třech místech:

- Cloudové prostředky poskytovatele rozhraní API
- Fakturace rozhraní API poskytovatele cloudu
- Ruční vytvoření značky, z následujících zdrojů:
    - Značky entity Cloudyn - uživatelem definované datové meta u Cloudyn entity
    - Kategorie Správce – dat čisticí nástroj, který vytvoří nové značky na základě pravidel, které se použijí pro existující značky

Chcete-li zobrazit značky zprostředkovatele cloudu v sestavách Cloudyn náklady na musí vytvořit model přidělení vlastní náklady pomocí 360 přidělení náklady. Chcete-li to provést, přejděte na **náklady** > **náklady na správu** > **náklady přidělení 360**, vyberte požadované značky a pak definovat pravidla pro zpracování vyžadující náklady. Pak vytvořte nový model náklady. Potom můžete zobrazit sestavy v analýza nákladů na přidělení k zobrazení, filtrů a řazení na značek prostředků Azure.

Značky prostředku Azure jsou zobrazeny pouze ve **náklady přidělení Analysis** sestavy.

Cloud poskytovatele fakturace značky se zobrazí v všechny sestavy náklady.

Značky entity Cloudyn a značky, které ručně vytvoříte zobrazí všechny sestavy náklady.


## <a name="create-showback-and-chargeback-reports"></a>Vytvoření sestavy kompletní přehled nákladů a vrácení peněz

Metody, které organizace používat k provádění kompletní přehled nákladů a vrácení peněz se výrazně liší. Však můžete všechny řídicí panely a sestavy na portálu Cloudyn jako základ pro buď účel. Zajistíte uživatelský přístup všem uživatelům ve vaší organizaci tak, aby se může zobrazit řídicí panely a sestavy na vyžádání. Všechny náklady analýza sestavy podporují kompletní přehled nákladů, protože ukazují uživatelům prostředky, které budou využívat. A umožňují uživatelům přejít k podrobnostem náklady a využití, data, která je specifická pro jejich skupiny v rámci vaší organizace.

Chcete-li zobrazit výsledky přidělení nákladů, otevřete sestavu analýzy náklady a vyberte náklady na model, který jste vytvořili. Pak přidejte seskupení jednu nebo více značek vybrané v modelu náklady.

![Sestava analýzy náklady](./media/tutorial-manage-costs/cost-analysis.png)

Můžete snadno vytvořit a uložit sestav, které se zaměřují na konkrétní služby spotřebovávají konkrétních skupin. Například může mít oddělení, který hojně používá virtuální počítače Azure. Můžete vytvořit sestavu, která je filtrován na virtuálních počítačích Azure zobrazíte využívání a náklady.

Pokud potřebujete poskytovat data snímku do dalších týmů, můžete exportovat žádnou sestavu ve formátu PDF nebo CSV.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Pomocí vlastních značek přidělit náklady.
> * Vytvoření kompletní přehled nákladů a sestav vracení peněz.



Další informace o začátcích se Cloudyn a její funkce pomocí přechodu na dokumentaci Cloudyn.

> [!div class="nextstepaction"]
> [Cloudyn dokumentace](https://support.cloudyn.com/hc/)

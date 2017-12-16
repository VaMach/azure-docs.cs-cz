---
title: "Zkontrolujte využití a náklady v Azure náklady na správu | Microsoft Docs"
description: "Zkontrolujte využití a nákladů pro sledování trendů, zjišťování umožňuje zvýšit efektivitu a vytvořit oznámení."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 363a7e8a5b5be2175cb2f6d3539878673a2b469a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="review-usage-and-costs"></a>Zkontrolujte využití a náklady

Azure náklady na správu Cloudyn ukazuje využití a náklady, aby mohli sledovat trendy, zjistit umožňuje zvýšit efektivitu a vytvořit výstrahy. Všechna data o využití a náklady se zobrazí v Cloudyn řídicí panely a sestavy. V příkladech v tomto kurzu vás provede, když je kontrola využití a náklady na použití řídicích panelů a sestav. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Sledování využití a trendy nákladů
> * Zjištění umožňuje zvýšit efektivitu využití
> * Vytvořte upozornění na neobvyklé výdajů a nákladů nad plán



## <a name="open-the-cloudyn-portal"></a>Otevřete portál Cloudyn

Prostudování všech využití a náklady na portálu Cloudyn. Otevřete portál Cloudyn z portálu Azure nebo přejděte na https://azure.cloudyn.com a přihlaste se.

## <a name="track-usage-and-cost-trends"></a>Sledování využití a trendy nákladů

Můžete sledovat skutečné peníze stráveného pro využití a náklady se sestavami v čase pro identifikaci trendů. Pokud chcete spustit, prohlížení trendy, pomocí sestavy skutečné náklady v čase. V nabídce sestavy v horní části portálu, klikněte na tlačítko **náklady** > **analýza nákladů** > **skutečné náklady v čase**. Když poprvé otevřete sestavu, použijí se k němu žádné skupiny nebo filtry.

Tady je ukázková sestava:

![Ukázková sestava](./media/tutorial-review-usage/actual-cost01.png)

Tato sestava zobrazuje všechny útraty za posledních 30 dní. Chcete-li zobrazit pouze výdaje služby Azure, použijte skupiny služby a pak filtrovat všech služeb Azure. Následující obrázek znázorňuje filtrované služby.

![filtrované služby](./media/tutorial-review-usage/actual-cost02.png)

V předchozím příkladu byl stráven nižší cenu na 2017-08-31 než před zahájením. Tento trend náklady dál pro různé služby o devět dní. Potom další výdaje pokračuje jako předtím. Příliš mnoho sloupců však mohou skrývat zřejmé trendu. Zobrazení sestavy můžete změnit na řádek nebo oblasti grafu zobrazíte data zobrazená v ostatních zobrazeních. Následující obrázek ukazuje trend zřetelněji.

![trend v sestavě](./media/tutorial-review-usage/actual-cost03.png)

V příkladu byste jasně vidět, že Azure Storage a náklady vynechaných spouštění na 2017-08-31 při výdaje na jinými službami Azure nižší úrovně. Ano co způsobilo tento snížení v výdaje? Někteří zaměstnanci v tomto příkladu byly na dovolenou mimo pracovní a nepoužívá službu úložiště.

Podívejte se na kurz video o sledování trendů využití a náklady, najdete v tématu [analýza cloudu fakturace dat oproti době s Azure náklady na správu Cloudyn](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Zjištění umožňuje zvýšit efektivitu využití

Optimalizátor sestavy zlepšit efektivitu, optimalizovat využití a identifikovat způsoby, jak ušetřit peníze věnovaný cloudové prostředky. Jsou užitečné zejména s doporučeními nákladově efektivní velikosti určené snížit nečinnosti nebo nákladné virtuálních počítačů.

Strategie jejich virtualizace je častých problémů, které ovlivňuje organizace, když se původně přesouvají prostředky cloudu. Často používají přístup podobně jako ten, který se používá k vytváření virtuálních počítačů pro místní virtualizovaného prostředí. A, se předpokládá, že přesunutím jejich místní virtuální počítače do cloudu, jako jsou snížit náklady-je. Tento přístup však není pravděpodobné, jak snížit náklady.

Problém je, že byl již zaplacení své stávající infrastruktury. Uživatelé lze vytvořit a udržovat velké virtuální počítače se systémem, pokud jejich líbilo – nečinnosti nebo Ne a s malým množstvím důsledkem. Přechodu do cloudu je velký nebo nečinné virtuálních počítačů, bude pravděpodobně *zvýšit* náklady. Cenově přidělení pro zdroje je důležité, když zadáte do smluv s poskytovatele cloudových služeb. Platíte musí pro potvrzení ať už používáte prostředek plně nebo ne.

Sestava náklady na platné velikost doporučení identifikuje potenciální úspora roční tak, že porovnáte kapacity typ instance virtuálních počítačů k jejich historických procesoru a data o využití paměti.  

V nabídce sestavy v horní části portálu, klikněte na tlačítko **Optimalizátor** > **ceny optimalizace** > **náklady platné velikost doporučení**. Filtrujte zprostředkovatele k Azure a podívejte se na pouze virtuální počítače Azure. Tady je příklad obrázek.

![Virtuální počítače Azure](./media/tutorial-review-usage/sizing01.png)

V tomto příkladu může uložit $3,114 pomocí následujících doporučení k měnit typy instance virtuálních počítačů. Kliknutím na symbol plus (+) v části **podrobnosti** pro první doporučení. Zde jsou uvedeny podrobnosti o první doporučení.

![Podrobnosti o doporučení](./media/tutorial-review-usage/sizing02.png)

Zobrazit ID instance virtuálního počítače kliknutím na plus symbol vedle **seznamu kandidátů**.

![Seznam kandidáty](./media/tutorial-review-usage/sizing03.png)

Podívejte se na kurz video o zjišťování umožňuje zvýšit efektivitu využití, najdete v tématu [optimalizace velikost virtuálního počítače v Azure náklady na správu Cloudyn](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Vytvořte výstrahy pro neobvyklou výdaje

Můžete výstrahu zúčastněným stranám automaticky pro útraty anomálií a nákladů nad plán rizika. Můžete snadno a rychle vytvořit výstrah pomocí sestavy, podporu výstrahy založené na nároky a náklady prahové hodnoty.

Vytvoření oznámení pro jakékoli útraty pomocí žádnou sestavu náklady. V tomto příkladu pomocí upozornění v případě virtuálního počítače Azure výdaje přiblíží celkový rozpočet sestavy skutečné náklady v čase. V nabídce sestavy v horní části portálu, klikněte na tlačítko **náklady** > **analýza nákladů** > **skutečné náklady v čase**. Nastavit **skupiny** k **služby** a nastavte **filtru ve službě** k **virtuální počítač Azure nebo**. V horní pravé části sestavy, klikněte na tlačítko **akce** a pak vyberte **Naplánování sestavy**.

Použití **plánování** kartu a pošlete sami sobě e-mailu pomocí frekvenci, který chcete sestavy. Všechny značky, seskupování a filtrování jste použili jsou zahrnuty v sestavě poslaného e-mailem. Klikněte na tlačítko **prahová hodnota** kartě a vyberte zvolte **vs skutečné náklady. Prahová hodnota**. Pokud jste měli celkové nároky než 500 000 $ a jste chtěli oznámení, když náklady v blízkosti o polovinu, vytvořte **Red výstraha** na 250 000 $ a **žlutá výstraha** v 240,000 $. Zvolte počet po sobě jdoucích výstrah. Pokud se zobrazí celkový počet výstrah, které jste zadali, budou odeslány žádné další výstrahy. Uložení plánované sestavy.

![Ukázková sestava](./media/tutorial-review-usage/schedule-alert01.png)

Můžete také náklady na procento vs. Prahová hodnota metrika nároky na vytvoření výstrahy. Pomocí této metrika můžete nároky procenta místo hodnoty měny.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Sledování využití a trendy nákladů
> * Zjištění umožňuje zvýšit efektivitu využití
> * Vytvořte upozornění na neobvyklé výdajů a nákladů nad plán


Přechodu na v dalším kurzu se dozvíte o řízení přístupu k datům.

> [!div class="nextstepaction"]
> [Řízení přístupu k datům](tutorial-user-access.md)

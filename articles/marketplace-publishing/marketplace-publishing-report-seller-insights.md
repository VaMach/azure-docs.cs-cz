---
title: "Principy sestavy na základě využití Azure Marketplace a statistiky prodejce reporting | Microsoft Docs"
description: "Jako prodejce na Azure Marketplace pochopit sestavy na základě využití, také známé jako sestavu Statistika prodejce"
services: Azure Marketplace
documentationcenter: na
author: v-jeana
manager: lakoch
editor: 
ms.assetid: f1ffde66-98f0-4c3e-ad94-fee1f97cae03
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: v-jeana; hascipio; v-dabosl
ms.openlocfilehash: e098e27e32f7b7ae2009580a430f262aa7225206
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-your-seller-insights-report"></a>Pochopení sestavy Statistika prodejce
**Jaké jsou prodejce insights?**

Všechny virtuální počítač a na základě využití fakturace vydavatelů služby vývojáře dostávat měsíčními sestavami od Microsoftu, pokud jejich nabídky generují využití.

**Co se zobrazí?**

* **Uvítací e-mail:** jako nový vydavatel, obdržíte Uvítacího e-mailu s oznámením, že bude zahájena přijetí měsíční sestavy Statistika prodejce.
* **Sestavy měsíčních prodejů:** Pokud máte využití, zobrazí se druhé e-mail, který obsahuje sestavy a pokyny, jak získat přístup k heslo:

  * Pokud máte virtuální počítač nebo na základě využití fakturace služby pro vývojáře SKU, která generuje využití, měsíční sestavy se zobrazí podrobnosti o objednávky, využití, trhy a podrobnosti o odběrateli vaší nabídky.
  * Sestavy jsou zamčené heslem, které pouze vám a Microsoft vědět, aby bylo možné chránit data vašich zákazníků.
  * Microsoft nebude poslat zprávu, pokud žádná z vaší nabídky vygenerování využití v měsíci.

## <a name="understand-your-seller-insights-report"></a>Pochopení sestavy Statistika prodejce
**Objednávky podle typů SKU a licence: kartu objednávek Marketplace.**

![readingreportbyorders][2]

* Průřezy pomoct filtrovat podle jednotlivých prvků sestavy.
* Graf zobrazuje měsíční objednávky podle typu Azure licence. Každý panel zobrazuje celkový počet objednávky pro daný měsíc odděleně pro každý typ licence Azure.
* Graf zobrazuje měsíční objednávky jednotkou SKU. Každý řádek zobrazuje celkové měsíční objednávky pro všech skladových položek rozdělená jednotkou SKU.
* Graf zobrazuje měsíční trend objednávky typ licence Azure a typ licence Azure Marketplace.
* Výsečové grafy zobrazují objednávky typ licence Azure a typ licence Marketplace.
* Tabulka ukazuje celkové měsíční objednávky typ licence Marketplace, měsíční součty a celkové pro všechny měsíce.

**Objednávky podle typů SKU a licence: kartě používání Marketplace.**

![readingreportbyusage][3]

* Průřezy pomoct filtrovat podle jednotlivých prvků sestavy.
* Měli byste vybrat normalizovaný využití virtuálních počítačů nebo nezpracovaná využití.
* Graf zobrazuje měsíční využití podle typu Azure licence. Každý řádek zobrazuje celkové využití pro daný měsíc odděleně pro každý typ licence Azure.
* Graf zobrazuje měsíční využití jednotkou SKU. Každý řádek zobrazuje celkové měsíční využití pro všech skladových položek rozdělená jednotkou SKU.
* Graf zobrazuje měsíční trend využití typ licence Azure a typ licence Marketplace.
* Výsečové grafy zobrazují využití typ licence Azure a typ licence Marketplace.
* Tabulka ukazuje celkové měsíční využití typ licence Marketplace, měsíční součty a celkové pro všechny měsíce.

**Data o využití a Data pořadí karet**

Tyto karty poskytují podrobná data sloužící ke generování sestavy.

![orderdata][4]

![usagedata][5]

**Využití podle typů SKU a licence: karta zákazníků**

![customerstab][6]

* Poznámka: v klauzuli důvěrnost.
* Tato karta obsahuje seznam zákazníků SKU, informace o profilu, datum transakce a výslovný souhlas pro propagační kontakt.
* Sestava obsahuje počet pořadí podle SKU a celkový počet položek.

**Právní omezení**

![legal][1]

Pečlivě si přečtěte právní omezení. Pokud máte jakékoli dotazy nebo připomínky, klikněte na odkaz v dolní části omezení k přesměrování na stránku podpory Marketplace.

## <a name="request-a-password-reminder"></a>Žádost o připomenutí heslo
Přejděte do https://publish.windowsazure.com/ a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.
![passwordreminder][7]

Vyberte **vydavatelů** kartě.
![selectpublisherstab][8]

Najít ID vydavatele v adrese URL:

* Toto ID můžete použijte jako heslo pro otevření souboru aplikace Excel Statistika prodejce.
  Toto je vaše heslo až do dalšího oznámení.
* Doporučujeme používat Microsoft Office 2013 s Windows jako váš sešit čtečky výběru.  Někteří uživatelé hlásili problémy pomocí aplikace Microsoft Office for Mac.

![publisherid][9]

## <a name="next-steps"></a>Další kroky
Pokud máte dotazy týkající se sestav a statistiky, obraťte se na náš tým podpory:

1. Přejděte na stránkách podpory v https://publish.windowsazure.com/.
2. V **typ problému** vyberte **sestavy a statistiky**.
3. V **kategorie** vyberte **otázky související se sestavami**.
4. Klikněte na tlačítko **žádost o spuštění**.
   ![sellerinsightsquestions][10]

[1]: ./media/marketplace-publishing-report-seller-insights/legal.png
[2]: ./media/marketplace-publishing-report-seller-insights/readingreportbyorders.png
[3]: ./media/marketplace-publishing-report-seller-insights/readingreportbyusage.png
[4]: ./media/marketplace-publishing-report-seller-insights/orderdata.png
[5]: ./media/marketplace-publishing-report-seller-insights/usagedata.png
[6]: ./media/marketplace-publishing-report-seller-insights/customerstab.png
[7]: ./media/marketplace-publishing-report-seller-insights/passwordreminder.png
[8]: ./media/marketplace-publishing-report-seller-insights/selectpublisherstab.png
[9]: ./media/marketplace-publishing-report-seller-insights/publisherid.png
[10]: ./media/marketplace-publishing-report-seller-insights/sellerinsightsquestions.png

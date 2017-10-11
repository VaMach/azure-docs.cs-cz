---
title: "Testování dat služby nabídku pro Marketplace | Microsoft Docs"
description: "Pochopit, jak otestovat vaši nabídku Data služby pro Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Při přípravě testování vaši nabídku Data Service
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po dokončení první dva kroky [vytvoření účtu Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) a [vytváří nabízejí služby Data publikování portálu](marketplace-publishing-data-service-creation.md) jste připraveni zpřístupnění vaši nabídku ve službě Azure Marketplace. Toto téma vás provede první, zprostředkující krok s názvem "Přípravy"

Pracovní znamená nasazení vaši nabídku v soukromé "izolovaného", kde můžete otestovat a ověřit jeho funkčnost před odesláním do produkčního prostředí. Nabídka se zobrazí v pracovním stejně, jako by zákazníkovi, který je nasazena.

## <a name="step-1-pushing-your-offer-to-staging"></a>Krok 1. Když zavedete vaši nabídku do přípravy
Když zavedete vaši nabídku pracovní umožňuje otestovat nabídku předtím, než je k dispozici budoucí odběratelům.  Uvidíte, jak vaši nabídku zobrazených a funkce pro ty odběr k datům.  

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Přihlášení do [publikování portálu](https://publish.windowsazure.com)
2. Vyberte **datové služby** v levém navigačním okně
3. Vyberte nabídku, které chcete nabízená pracovní. Zobrazí se na obrazovce výše.
4. Klikněte na tlačítko **Push pracovní** tlačítko.  
5. Pokud jsou problémy s nabídka, která potřeba provést před vkládání do přípravy, zobrazí se seznam zobrazí.  Opravte tyto položky kliknutím na každou položku v seznamu. Po kliknutí na všechny opravy provedené, **nabízená pracovní** tlačítko znovu.

Pokud zde nejsou žádné problémy s vaši nabídku uvidíte automaticky otevíraném okně níže.  

Pokud jste není plánování nebo nebyla schválena prezentovat vaši nabídku na portálu Azure (aktuálně má omezenou kapacitu), zavřete právě překryvné okno.

Testování dat služby v portálu Azure (kromě portálu DataMarket), budete potřebovat ID předplatného Azure k testování s.  Toto ID předplatného bude identifikovat účet, který se bude moct otestovat vaši nabídku.  

Vyjmout a vložit svoje ID předplatného a kliknutím na značku zaškrtnutí pokračujte.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> ID těchto předplatných Azure jsou pouze požadované pro testovacích a přípravných v [portálu pro správu Azure](https://manage.windowsazure.com). Nejsou potřeba otestovat v Azure Marketplace.
> 
> 

Na další obrazovce, která se zobrazí ukazuje, že se zobrazením na ikonu "Probíhá" Probíhá publikování zvýrazněná žlutý níže. Když zavedete pracovní trvat 10 až 15 minut.  Pokud trvá déle, nejprve aktualizujte webový prohlížeč (stiskněte F5 v aplikaci Internet Explorer).  Ve výjimečných případech, kde je vaši nabídku stále nabízet pracovní po hodině, klikněte na tlačítko kontaktujte nás propojit dejte nám vědět, že se vyskytl problém.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Po dokončení nabízeného oznámení do přípravy na ikonu "Probíhá" bude zastavení přesouvání a stav bude aktualizovat na "Připravený".  Nyní jste připraveni k testování vaši nabídku.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Krok 2. Otestovat vaši nabídku dvoufázové instalace v DataMarket
Klikněte na odkaz následující text **"v tématu služby nabízejí v..."** Chcete-li zobrazit na obrazovce, že odběratele se zobrazí, když vaši nabídku přejde do produkčního prostředí a zobrazí se v DataMarket.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testování nebo ověřte jednotlivých 12 položkách označena výše zajistit všechny loga, ceny nebo transakce, text, obrázky, dokumentace, a odkazy správně jsou správné a funkční.  Toto je vhodná doba na Ujistěte se, že všechny testovací hodnoty, které jste zadali při vytváření vaši nabídku byly nahrazeny skutečnými hodnotami.

1. Logo nabídka
2. Název nabídky
3. Název vydavatele nebo odkaz na web vaší společnosti
4. Kategorie vyhledávání pro vaši nabídku
5. Odkaz na podporu vaši nabídku pomůže odběratele
6. Kontextová popis pro vaši nabídku
7. Nabídka plán znázorňující fakturačních údajů
8. Odkaz na implementaci kódu
9. Ukázka bitové kopie, které ilustrují použití dat nabídky
10. Metadata vstupu a výstupu u každé služby v rámci nabídky
11. Nabídka pro podmínky použití
12. Náhled nabídka dat

Nakonec zkontrolujte, že služba bude fungovat prostřednictvím Datamarket kliknutím na odkaz "Prozkoumat tento datovou sadu".  Otevře se nové okno v nástroji říkáme "Service Explorer", můžete zobrazit náhled výsledků dotazu na vaši službu.  V tomto okně můžete zadat potřebné parametry a zobrazit výsledky zobrazí z dotazu na vaši službu.   Navíc zobrazí je adresa URL pro svůj dotaz.  

> [!NOTE]
> Nezapomeňte si projít textový popis služby zobrazí v horní části.  A pokud vaši nabídku se skládá z více než jedna služba volání, klikněte na karty v dolní části přejděte na další služby ke kontrole a testování.
> 
> 

## <a name="next-step"></a>Další krok
Pokud potřebujete další pomoc a problémy s jejich řešení obraťte se prosím [Azure technickou podporu vydavatele](http://go.microsoft.com/fwlink/?LinkId=272975).

Pokud jste splněna a připravena k publikování vaši nabídku, přečtěte si prosím [schválení žádosti o změnu do Push do produkčního prostředí](marketplace-publishing-push-to-production.md) dokumentaci.

## <a name="see-also"></a>Viz také
* [Začínáme: Jak publikování nabídky pro Azure Marketplace](marketplace-publishing-getting-started.md)


---
title: "Příručka o vytváření datové služby pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit, certifikovat a datové služby pro nasazení zakoupit na webu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Služba data publikování průvodce pro Azure Marketplace
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po dokončení kroku 1, [vytváření účtů a jejich registrace](marketplace-publishing-accounts-creation-registration.md), jsme na základě vás [obecné netechnické](marketplace-publishing-pre-requisites.md) a [technické požadavky](marketplace-publishing-data-service-creation-prerequisites.md) z datové služby nabídku Azure Marketplace. Nyní jsme vás provede kroky pro vytvoření nabídka služby Data na [publikování portál] [ link-pubportal] pro Azure Marketplace.

## <a name="1----login-to-the-publishing-portal"></a>1.    Přihlášení k publikování portálu.
Přejděte na [https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Pro první čas přihlášení na portál publikování použijte stejný účet, pomocí kterého byl zaregistrován vaší společnosti prodejce profil ve středisku pro vývojáře.**  (Později můžete přidat všechny zaměstnance ve vaší společnosti jako spolusprávce publikování portálu).

Klikněte na **publikovat Data Services** dlaždici, pokud je to první přihlášení na portál pro publikování.

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2.    Zvolte **datové služby** v navigační nabídce na levé straně.
  ![Kreslení](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Vytvoření nové služby dat
Zadejte název pro vaše nové dat služby nabízejí a klikněte na "+" na pravé straně.

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4.    Zkontrolujte dílčí nabídky pod nově vytvořený službou Data v navigační nabídce.
Klikněte na **návod** a zkontrolujte všechny potřebné kroky potřebné k publikování správně službu dat v Azure Marketplace.

> [!TIP]
> Vždy můžete kliknutím na odkazy na stránce "Postup" nebo použití karet na nabídku služby Data dílčí nabídky na levé straně.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Vytvořte nový plán.
### <a name="offers-plans-transactions"></a>Nabízí, plánů, transakce.
Každý nabídka může mít více plánů, ale musí mít alespoň jedna (1) plánu. Když koncoví uživatelé přihlášení k odběru na vaši nabídku přihlášení odběru pro jednu nabídku na plánu. Každý plán definuje, jak koncoví uživatelé budou moci používat vaši službu.

Aktuálně Azure Marketplace podporovat pouze měsíční předplatné transakce založené model pro datové služby, tj. koncoví uživatelé budou platit měsíční poplatek podle cenu konkrétní plán, který se přihlásit k odběru a budou moci využívat každý měsíc počet transakcí definované v plánu.

Každou transakci, obvykle definovány jako počet záznamů, které vrátí Data služby založené na dotazu, odešle do služby. Výchozí hodnota je 100. Počet transakcí vrátil pro každý dotaz musí být počet záznamů dělený 100 a zaokrouhlené nahoru na nejbližší celé číslo.

Je odpovědností vrstvy služby Azure Marketplace monitorování (monitorování) počet transakcí spotřebovávají každý dotaz.

> [!IMPORTANT]
> Koncoví uživatelé, které bylo dosaženo limitu transakce v měsíci se bude blokovat pokračovat v používání služby až konce jejich měsíčním cyklu předplatného.
> 
> Plán nebo jeden z plánů může (ale není nutné) obsahovat neomezený počet transakcí.
> 
> 

### <a name="create-a-plan"></a>Vytvoření plánu.
1. Klikněte na **"+"** vedle "Přidat nový plán".
2. Vyberte jednu z možností: **neomezený** nebo **omezené** využití pro tento plán.  Pokud omezené potom zadejte počet transakcí s plánem vám umožní využívat za měsíc.
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Publikování portál také navrhnout "Plán identifikátor", který bude používaný pro komunikaci se koncoví uživatelé název plánu v uživatelském rozhraní a také používá služba Marketplace k identifikaci plánu. Pokud chcete, můžete změnit "Plánování identifikátor".
   
   > [!NOTE]
   > "Plánování identifikátor" musí být jedinečný v rámci oboru každou nabídku. Po první publikování do výroby a nebude možné změnit tento identifikátor se uzamkne jako mnoho dalších identifikátorů použít v identifikátoru plánování publikování portálu.
   > 
   > 
3. Kliknutím na tlačítko Přijmout svého výběru.
4. Potom budete požádáni několik další otázky týkající se nově vytvořená plánu.
   
    ![Kreslení](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Otázky | Násobek. |
| --- | --- |
| **Tento plán je volné a dostupné celosvětové?** |Můžete vytvořit plán úplně bez z – zdarma. Pokud se jedná pouze plán k této nabídce –, znamená to, že publikujete "Volné nabízejí" na webu Marketplace. Pokud se jedná pouze pro jeden (z několika) plán, it vám dává možnost nabízí koncovým uživatelům, aby další informace o služby s relativně malý počet transakcí za měsíc.  Pokud je odpověď "Ano", bude se dotaz žádné další otázky. |

> [!NOTE]
> Koncoví uživatelé můžete vždy upgradovat na placené plány.
> 
> 

| Otázky | Násobek. |
| --- | --- |
| **Je k dispozici bezplatná zkušební verze?** |Můžete vybrat mezi "Ne zkušební verzi" vůbec nebo poskytnout možnost k použití vašeho plánu pro "Jeden měsíc". Vydavatelé se chtěli použít tuto možnost dát koncovým uživatelům možnost pochopit výhody sady nabídku zdarma pro jeden měsíc. |

> [!IMPORTANT]
> Koncoví uživatelé budou pouze moct zakoupit bezplatnou zkušební verzi, pokud budou mít vytvořené platebním nástrojem například platební karty, smlouvy enterprise.
> 
> Po jednom měsíci z bezplatné zkušební verze Azure Marketplace se spustí zákazníkům poplatků za cenu k datu odběru, pokud zákazník iniciované zrušení předplatného. Žádná speciální oznámení bude potřeba poskytnout koncovým uživatelům.
> 
> 

| Otázky | Násobek. |
| --- | --- |
| **Tento plán vyžaduje propagační kód, chcete-li zakoupit?** |Vydavatelé mít možnost omezit přístup k jejich plány služby tím, že poskytuje speciální kódu, nazývají "A Promocode" pro konkrétní zákazníky. Pouze koncoví uživatelé, kteří budou mít tento Promocode budou moci přihlásit do plánu. Pokud zvolíte "Ne", pak souhlasíte s tím, každý z oblasti, kde je k dispozici nabídku (najdete v části [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) podrobnosti) budou moci přihlásit na tento plán. Žádné další otázky se dotaz. |
| **Také skrýt tento plán z každý, kdo nemá platný propagační kód?** |Pokud je odpověď na otázku předchozí "Ano" vydavatele má možnost úplně odebrat tento plán zobrazování v uživatelském rozhraní Marketplace. Znamená to, zákazníci neuvidí tento plán nabídku na stránce Podrobnosti. Koncoví uživatelé, kteří obdrží promocode o zakoupení, budou moci přihlásit se pomocí této promocode. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    Vytvoření vašeho webu Marketplace marketingové obsahu
Postup zadejte informace v **Marketing, Cenová, podpory a kategorie** karty navštivte [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) tedy společné pro všechny artefakty publikována ve službě Azure Marketplace.  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7.    Vaši nabídku připojení k službě (na základě SQL Azure nebo na základě webové služby).
Klikněte na **datové služby** dílčí nabídky.

V horní polovině stránky budete požádáni o zadání nabídka **Namespace**.  

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.png)

Následující otázky bude definovat jak vydavatele bude nově vytvořený zveřejněte nabídku na webu Azure Marketplace. (Podrobnosti najdete v části [Data služby technické požadovaných průvodce](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Publikování databáze na základě služby**

Klikněte na **databáze**. Zobrazí se následující stránka:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.3.png)

Vytvořit mapování CSDL datové sady založené na databázi SQL Azure:

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.4.png)

A potom pro každou tabulku

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.6.png)

Pokud webová služba

  ![Kreslení](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Čtení [mapování existující webové služby OData prostřednictvím CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) podrobné pokyny a příklady pro vytváření CSDL webové služby.
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když jste vytvořili vaši nabídku Data Service, zkontrolujte, že provedete podle pokynů v [Marketplace Marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) předtím, než můžete přejít ke [testování služby dat v pracovním](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Viz také
* [Začínáme: Jak publikování nabídky pro Azure Marketplace](marketplace-publishing-getting-started.md)
* Pokud vás zajímá porozumět celkový proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) ke kontrole definice struktury a pokynů.
* Pokud vás zajímá učení a seznámit se s konkrétním uzlům a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete ukázkový kód a pochopit syntaxe kódu a kontext.

[link-pubportal]:https://publish.windowsazure.com

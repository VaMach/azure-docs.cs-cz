---
title: "Plánů služby App Service v Azure App Service Web Apps | Microsoft Docs"
description: "Zjistěte, jak plánů služby App Service pro pracovní Azure App Service a jak budou využívat vaše prostředí pro správu."
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: fb5b782f09bdd8c8a862eddfbd65b0f86ef8d08c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="app-service-plans-in-azure-app-service-web-apps"></a>Plány služby App Service v Azure App Service Web Apps

Plány aplikační služby představují kolekci fyzické prostředky, které jsou použity k hostování vaší aplikace.

Plány služby App Service definují:

- Oblast (západní USA, Východ USA, atd.)
- Počet škálování (jeden, dva, tři instance atd.)
- Velikost instance (malé, střední, velké)
- Skladová položka (bezplatné sdílené, Basic, Standard, Premium, PremiumV2, izolovaný režim)

Webové aplikace, mobilní aplikace, aplikace API, funkce aplikace (nebo funkce), v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spuštění v rámci plánu služby App Service.  Plán služby App Service můžete sdílet aplikací ve stejném předplatném a oblast. 

Všechny aplikace, které jsou přiřazené **plán služby App Service** sdílet prostředky definované ho. Tato sdílení úsporu nákladů při hostování více aplikacemi v jednom plánu služby App Service.

Vaše **plán služby App Service** možné škálovat od **volné** a **sdílené** úrovně do **základní**, **standardní**,  **Premium**, a **izolovaná** vrstev. Každý vyšší úroveň umožňuje přístup k více prostředkům a funkce.

Pokud váš plán služby App Service je nastavený na **základní** úroveň, nebo vyšší, pak můžete řídit **velikost** a škálování počtu virtuálních počítačů.

Například, pokud váš plán je nakonfigurovaný na použití dvě instance "malá" v **standardní** vrstvy, všechny aplikace v tomto plánu spustit na obou instancí. Aplikace také mají přístup k **standardní** vrstvy funkce. Instancích plánu, na kterých běží aplikace jsou plně spravovaná a vysoce dostupné.

> [!IMPORTANT]
> Cenové úrovně (SKU) se plán služby App Service určuje náklady a není počet aplikací, které jsou v ní umístěné.

Tento článek popisuje klíčové charakteristiky plán služby App Service, například cenové úrovně a škálování a jak pracují při správě aplikací.

## <a name="new-pricing-tier-premiumv2"></a>Nové cenová úroveň: PremiumV2

Nové **PremiumV2** cenová úroveň nabízí [virtuální počítače Dv2-series](../virtual-machines/windows/sizes-general.md#dv2-series) s rychlejších procesorů, úložiště SSD a dvojité poměr paměti jádra ve srovnání s **standardní** vrstvy. **PremiumV2** také podporuje škálování na větší rozsah prostřednictvím počet vyšší instancí současně stále zajišťuje rozšířené možnosti v standardní plán nalezen. Všechny funkce, které jsou k dispozici v existující **Premium** jsou součástí vrstvy **PremiumV2**.

Podobně jako u jiných vyhrazené vrstev, tři velikosti virtuálních počítačů jsou k dispozici pro tuto vrstvu:

- Malá (1 jádro procesoru, paměti 3.5 GiB) 
- Střední (2 jádra procesoru, paměti 7 GiB) 
- Velké (4 jádra procesoru, paměti 14 GiB)  

Pro **PremiumV2** informace o cenách najdete v části [App Service – ceny](/pricing/details/app-service/).

Začněte s novým **PremiumV2** cenovou úroveň, najdete v části [konfigurace PremiumV2 úroveň služby App Service](app-service-configure-premium-tier.md).

## <a name="apps-and-app-service-plans"></a>Aplikace a plány služby App Service

Aplikace ve službě App Service může být přidružen pouze jeden plán služby App Service v daném okamžiku.

Aplikace a plány jsou součástí **skupiny prostředků**. Skupiny prostředků slouží jako hranice životního cyklu pro každý prostředek, který je v něm. Skupiny prostředků můžete spravovat všechny součásti aplikace společně.

Vzhledem k tomu, že jedna skupina prostředků může mít víc plány služby App Service, kterou můžete přidělit různé aplikace na různé fyzické prostředky.

Například můžete oddělit prostředků mezi vývoj, testování a provozním prostředí. S samostatných prostředí pro provoz a vývoj nebo testování umožňuje izolovat prostředky. Tímto způsobem není testování proti nové verze aplikace zatížení pokouší o stejné prostředky jako produkční aplikací, které slouží skutečnou zákazníků.

Pokud máte více plánů v jedna skupina prostředků, můžete také definovat aplikaci, která přesahuje zeměpisné oblasti.

Například vysokou dostupností aplikaci spuštěnou ve dvou oblastech obsahuje alespoň dva plánům, jeden pro každou oblast a jedna aplikace, které jsou spojené s každou plánu. V takovém případě jsou všechny kopie aplikace pak obsažené v jedna skupina prostředků. Skupinu prostředků s více schématy a víc aplikací mít usnadňuje správu, řízení a zobrazení stavu aplikace.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Vytvořit plán služby App Service nebo použít existující

Při vytváření nové webové aplikace ve službě App Service, můžete sdílet prostředky hostování tím, že aplikace do existující plán aplikační služby. Chcete-li zjistit, že jestli nové aplikace bude mít potřebné prostředky, je potřeba pochopit kapacitu existující plán služby App Service a očekávané zatížení pro novou aplikaci. Přepsání přidělování prostředků může potenciálně způsobit výpadky v nových nebo stávajících aplikací.

Doporučujeme, abyste izoluje aplikace do nové služby App Service při plánování:

- Aplikace je náročná.
- Aplikace má různých faktorech škálování z jiných aplikací hostovaná v existující plán.
- Aplikace musí prostředků v jiné zeměpisné oblasti.

Tímto způsobem můžete přidělit novou sadu prostředků pro vaši aplikaci a získáte větší kontrolu nad vaší aplikace.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

> [!TIP]
> Pokud máte služby App Service Environment, najdete v části [vytvořit plán služby App Service ve službě App Service Environment](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Můžete vytvořit plán služby App Service prázdný nebo jako součást vytváření aplikace.

V [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.

![Vytvoření aplikace v portálu Azure.][createWebApp]

Pak můžete vybrat nebo vytvořit plán služby App Service pro novou aplikaci.

 ![Vytvořte plán služby App Service.][createASP]

Chcete-li vytvořit plán služby App Service, klikněte na tlačítko **[+] vytvořit nové**, typ **plán služby App Service** název a potom vyberte odpovídající **umístění**. Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň pro službu. Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**. Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Přesuňte aplikace na jiný plán služby App Service

Aplikace můžete přesunout na jiný plán služby App Service v [portál Azure](https://portal.azure.com). Aplikace můžete přesunout mezi plány také plány jsou v _stejnou skupinu prostředků a geografické oblasti_.

Přesunutí aplikace do jiného plánu:

- Přejděte do aplikace, který chcete přesunout.
- V **nabídky**, vyhledejte **plán služby App Service** části.
- Vyberte **plán služby App Service změnu** ke spuštění procesu.

**Plán služby App Service změnu** otevře **plán služby App Service** selektor. V tomto okamžiku můžete vybrat existující plán přesunout do této aplikace. Zobrazí se pouze plány ve stejné skupině prostředků a oblast.

![Selektor plán služby App Service.][change]

Každý plán má svou vlastní cenová úroveň. Například přesun lokalitu z úrovně Free plán úrovně Standard, umožňuje všechny aplikace, které jsou přiřazené k použití funkcí a prostředků na plán úrovně Standard.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonování aplikace na jiný plán služby App Service

Pokud chcete přesunout aplikace v jiné oblasti, jeden alternativou je aplikace klonování. Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.

Můžete najít **klonování aplikace** v **nástroje pro vývoj** části nabídky.

> [!IMPORTANT]
> Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Škálovat plán služby App Service

Existují tři způsoby, jak škálovat plán:

- **Změna plánu je cenová úroveň**. Plán v základní vrstvě lze převést na Standard a všechny aplikace, které jsou přiřazené k použití funkcí na plán úrovně Standard.
- **Změňte velikost instance plánu**. Jako příklad můžete změnit plán v základní vrstvě, který používá malé instance na používání velké instancí. Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší velikost instance.
- **Změnit počet instancí plánu**. Například standardní plán, který je na tři instance škálovat na více systémů můžete škálovat na 10 instancí. Plán Premium můžete škálovat na více systémů na 20 instance (přičemž podléhá dostupnosti). Všechny aplikace, které jsou přidruženy plán teď můžete použít další paměť a prostředky procesoru, které nabízí větší počet instancí.

Můžete změnit cenovou úroveň a instance velikost klepnutím **vertikálně navýšit kapacitu** položky v části nastavení pro aplikace nebo plán služby App Service. Změny použít na plán služby App Service a ovlivňují všechny aplikace, které je hostitelem.

 ![Nastavte hodnoty škálování aplikace.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Vyčištění plán služby App Service

> [!IMPORTANT]
> **Plánů služby App Service** mají žádné aplikace přidružené k je stále platit poplatky vzhledem k tomu, že budou nadále záložní kapacita výpočty.

Nechcete neočekávané poplatky, když se odstraní poslední aplikace hostovaná v plán služby App Service, je taky odstranit výsledné prázdný plán služby App Service ve výchozím nastavení.

## <a name="summary"></a>Souhrn

Plány aplikační služby představují sadu funkcí a kapacity, které můžete sdílet mezi aplikacemi. Plány služby App Service poskytují flexibilitu pro přidělovat konkrétní aplikace sadu prostředků a dále optimalizovat využití vaší prostředků Azure. Tímto způsobem, pokud chcete ušetřit peníze u svého testovacího prostředí, můžete plán sdílet mezi více aplikacemi. Pomocí příjmu v několika oblastech a plány a také můžete maximalizovat propustnost pro produkční prostředí.

## <a name="whats-changed"></a>Co se změnilo

- Průvodce změnou z webů na službu App Service, najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png

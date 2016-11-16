---
title: "Vytvoření první funkce Azure | Dokumentace Microsoftu"
description: "Vytvořte svoji první funkci Azure, aplikaci bez serveru, za méně než dvě minuty."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/08/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e446766c0f3d19a8cce585d708e8e7a809593408


---
# <a name="create-your-first-azure-function"></a>Vytvoření první funkce Azure
## <a name="overview"></a>Přehled
Azure Functions je výpočetní prostředí na vyžádání založené na událostech. Rozšiřuje stávající platformu aplikací Azure o funkce pro implementaci kódu aktivovaného událostmi, ke kterým dochází v jiných službách Azure, produktech SaaS a místních systémech. Díky Azure Functions se aplikace škálují na základě poptávky a vy platíte jen za využité prostředky. Azure Functions umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma vám ukáže, jak pomocí rychlého startu Azure Functions na portálu vytvořit jednoduchou funkci „hello world“ v Node.js, která se vyvolá triggerem protokolu HTTP. Můžete také zhlédnout krátké video a podívat se, jak se tyto kroky na portálu provádějí.

## <a name="watch-the-video"></a>Přehrát video
Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Vytvoření funkce z rychlého startu
Provádění funkcí v Azure je hostováno v aplikaci funkce. Podle těchto kroků můžete vytvořit novou aplikaci funkce a novou funkci. Aplikace Function App se vytvoří s výchozí konfigurací. Příklad explicitního vytvoření aplikace Function App najdete ve [stručném úvodním kurzu k Azure Functions](functions-create-first-azure-function-azure-portal.md).

Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.
2. Zadejte jedinečný **název** nové aplikace funkce nebo přijměte vygenerovaný název, vyberte upřednostňovanou **Oblast** a klikněte na možnost **Vytvořit a začít**. 
3. Na kartě **Rychlý start** klikněte na **WebHook + API** a **JavaScript** a potom klikněte na **Vytvořit funkci**. Vytvoří se nová předdefinovaná funkce v Node.js. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Volitelné) V rychlém startu teď můžete absolvovat krátkou prohlídku funkcí Azure Functions na portálu.    Až prohlídku dokončíte nebo přeskočíte, můžete novou funkci otestovat pomocí triggeru protokolu HTTP.

## <a name="test-the-function"></a>Testování funkce
Rychlé starty Azure Functions obsahují funkční kód, a proto můžete novou funkci hned otestovat.

1. Na kartě **Vývoj** si prohlédněte okno **Kód** a všimněte si, že tento kód v Node.js očekává žádost HTTP s hodnotou *name* předanou buď v textu zprávy, nebo v řetězci dotazu. Když je funkce spuštěná, tato hodnota je vrácená v odpovědi.
   
    ![](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)
2. Posuňte se dolů k textovému poli **Text žádosti**, změňte hodnotu vlastnosti *name* na své jméno a klikněte na možnost **Spustit**. Uvidíte, že provedení se aktivuje testovací žádostí protokolu HTTP, do protokolů streamování se zapíšou informace a ve **výstupu** se zobrazí odpověď "hello". 
3. Pokud chcete aktivovat provedení té samé funkce z jiného okna nebo karty prohlížeče, zkopírujte hodnotu adresy **URL funkce** na kartě **Vývoj** a vložte ji do adresního řádku prohlížeče, potom připojte hodnotu řetězce dotazu `&name=yourname` a stiskněte klávesu Enter. Do protokolů se zapíší ty samé informace a v prohlížeči se jako odpověď opět zobrazí text „hello“.

## <a name="next-steps"></a>Další kroky
Tento rychlý start předvádí velmi jednoduché provedení základní funkce protokolu HTTP aktivované triggerem. Další informace o využití možností Azure Functions v aplikacích najdete v těchto tématech.

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné s Azure Functions (včetně plánu služby Dynamic), a výběr správného plánu. 
* [Co je Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
  Azure Functions využívá platformu Azure App Service pro základní funkce, jako jsou nasazení, proměnné prostředí a diagnostika. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->



---
title: Vytvoření funkce pomocí webu Azure Portal | Microsoft Docs
description: Za méně než dvě minuty vytvoříte svoji první funkci Azure, aplikaci bez serveru.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: ''
tags: ''

ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/08/2016
ms.author: glenga

---
# Vytvoření funkce na webu Azure Portal
## Přehled
Azure Functions je výpočetní prostředí na vyžádání založené na událostech. Rozšiřuje stávající platformu aplikací Azure o funkce pro implementaci kódu aktivovaného událostmi, ke kterým dochází v jiných službách Azure, produktech SaaS a místních systémech. Díky Azure Functions se aplikace škálují na vyžádání a vy platíte jenom za využité prostředky. Azure Functions umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma vám ukáže, jak pomocí webu Azure Portal vytvořit jednoduchou funkci Azure „hello world“ v Node.js, která se vyvolá triggerem protokolu HTTP. Před vytvořením funkce na webu Azure Portal musíte explicitně vytvořit Function App v Azure App Service. Pokud chcete, aby se aplikace Function App vytvořila automaticky, projděte si [stručný úvodní kurz k Azure Functions](functions-create-first-azure-function.md), který je pro začátek jednodušší a obsahuje video.

## Vytvoření Function App
Function App hostuje provádění vašich funkcí v Azure. Při vytváření Function App na webu Azure Portal použijte tyto pokyny.

Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).

1. Přejděte na web [Azure Portal](https://portal.azure.com) a přihlaste se pomocí účtu Azure.
2. Klikněte na **+Nové** > **Web + mobilní zařízení** > **Function App**, vyberte vaše **předplatné**, zadejte jedinečný **název aplikace** pro identifikaci Function App a potom zadejte následující nastavení:
   
   * **[Skupina prostředků](../azure-portal/resource-group-portal.md)**: Vyberte **Vytvořit novou** a zadejte název nové skupiny prostředků. Můžete také zvolit existující skupinu prostředků, v takovém případě ale pravděpodobně nebude možné pro Function App vytvořit dynamický plán služby App Service.
   * **[Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**: Vyberte *dynamický* nebo *klasický* plán. 
     * **Dynamický**: Tento typ je pro Azure Functions výchozí. Když zvolíte dynamický plán, musíte také zvolit **Umístění** a nastavit **Přidělení paměti** (v MB). Informace o tom, jak přidělení paměti ovlivňuje náklady, najdete v tématu [Ceny Azure Functions](https://azure.microsoft.com/pricing/details/functions/). 
     * **Classic**: Klasický plán služby App Service vyžaduje, abyste vytvořili **umístění/plán služby App Service**, nebo vybrali stávající. Tato nastavení určují [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci.  
   * **Účet úložiště**: Každá aplikace Function App vyžaduje účet úložiště. Můžete buď zvolit existující účet úložiště, nebo vytvořit nový. 
     
     ![Vytvoření nové aplikace Function App na webu Azure Portal](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)
3. Po kliknutí na **Vytvořit** se nová aplikace Function App zřídí a nasadí.  

Teď je aplikace Function App zřízená a můžete vytvořit první funkci.

## Vytvoření funkce
Tyto kroky vytvoří funkci pomocí rychlého startu Azure Functions.

1. Na kartě **Rychlý start** klikněte na **WebHook + API** a **JavaScript** a potom klikněte na **Vytvořit funkci**. Vytvoří se nová předdefinovaná funkce v Node.js. 
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)
2. (Volitelné) V rychlém startu teď můžete absolvovat krátkou prohlídku funkcí Azure Functions na portálu.   Až prohlídku dokončíte nebo přeskočíte, můžete novou funkci otestovat pomocí triggeru protokolu HTTP.

## Testování funkce
Rychlé starty Azure Functions obsahují funkční kód, a proto můžete novou funkci hned otestovat.

1. Na kartě **Vývoj** si prohlédněte okno **Kód** a všimněte si, že tento kód v Node.js očekává žádost HTTP s hodnotou *name* předanou buď v textu zprávy, nebo v řetězci dotazu. Když je funkce spuštěná, tato hodnota je vrácená v odpovědi.
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-develop-tab-testing.png)
2. Posuňte se dolů k textovému poli **Text žádosti**, změňte hodnotu vlastnosti *name* na své jméno a klikněte na **Spustit**. Uvidíte, že provedení se aktivuje testovací žádostí HTTP, do protokolů streamování se zapíšou informace a ve **výstupu** se zobrazí odpověď „hello“. 
3. Pokud chcete aktivovat provedení té samé funkce z jiného okna nebo karty prohlížeče, zkopírujte hodnotu **URL funkce** na kartě **Vývoj** a vložte ji do adresního řádku prohlížeče, potom připojte hodnotu řetězce dotazu `&name=yourname` a stiskněte klávesu Enter. Do protokolů se zapíší ty samé informace a v prohlížeči se jako odpověď opět zobrazí text „hello“.

## Další kroky
Tento rychlý start předvádí velmi jednoduché provedení základní funkce aktivované HTTP. Další informace o využití možností Azure Functions v aplikacích najdete v těchto tématech.

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné pro Azure Functions (včetně dynamického plánu služby), a výběr správného plánu. 
* [Co je Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
  Služba Azure Functions využívá platformu Azure App Service pro základní funkce, jako jsou nasazení, proměnné prostředí a diagnostika. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]

<!--HONumber=sep12_HO2-->



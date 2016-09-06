<properties
   pageTitle="Vytvoření první funkce Azure | Microsoft Azure"
   description="Vytvořte svoji první funkci Azure, aplikaci bez serveru, za méně než dvě minuty."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="hero-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/22/2016"
   ms.author="glenga"/>

#Vytvoření první funkce Azure

##Přehled
Řešení Azure Functions je prostředí typu „výpočetní prostředí jako služba“ řízené událostmi, které rozšiřuje stávající platformu aplikací Azure o funkce pro implementaci kódu aktivovaného událostmi, ke kterým dochází v jiných službách Azure, produktech SaaS a místních systémech. Díky Azure Functions se aplikace škálují na základě poptávky a vy platíte jen za využité prostředky. Azure Functions umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma vám ukáže, jak pomocí rychlého startu Azure Functions na portálu Azure Functions vytvořit jednoduchou funkci "hello world" v Node.js, která se vyvolá triggerem protokolu HTTP. Můžete také shlédnout krátké video a podívat se, jak se tyto kroky na portálu provádějí.

## Přehrát video

Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

[AZURE.VIDEO create-your-first-azure-function-simple]

##Vytvoření funkce z rychlého startu

Provádění funkcí v Azure je hostováno v aplikaci funkce. Podle těchto kroků můžete vytvořit novou aplikaci funkce a novou funkci. Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.

2. Zadejte jedinečný **název** nové aplikace funkce nebo přijměte vygenerovaný název, vyberte upřednostňovanou **Oblast** a klikněte na možnost **Vytvořit a začít**. 

3. Na kartě **Rychlý start** klikněte na položku **WebHook + API** > **Vytvořit funkci**. Vytvoří se nová předdefinovaná funkce v Node.js. 

4. (Volitelné) V rychlém startu teď můžete absolvovat krátkou prohlídku funkcí Azure Functions na portálu.   Až prohlídku dokončíte nebo přeskočíte, můžete novou funkci otestovat pomocí triggeru protokolu HTTP.

##Testování funkce

Rychlé starty Azure Functions obsahují funkční kód, a proto můžete novou funkci hned otestovat.

1. Na kartě **Vývoj** si prohlédněte okno **Kód** a všimněte si, že tento kód v Node.js očekává žádost protokolu HTTP s hodnotou *name* předanou buď v textu zprávy, nebo v řetězci dotazu. Když je funkce spuštěná, tato hodnota se vrátí ve zprávě s odpovědí.

2. Posuňte se dolů k textovému poli **Text žádosti**, změňte hodnotu vlastnosti *name* na své jméno a klikněte na možnost **Spustit**. Uvidíte, že provedení se aktivuje testovací žádostí protokolu HTTP, do protokolů streamování se zapíšou informace a ve **výstupu** se zobrazí odpověď "hello". 

3. Pokud chcete aktivovat provedení té samé funkce z jiného okna nebo karty prohlížeče, zkopírujte hodnotu adresy **URL funkce** na kartě **Vývoj** a vložte ji do adresního řádku prohlížeče, potom připojte hodnotu řetězce dotazu `&name=yourname` a stiskněte klávesu Enter. Do protokolů se zapíší ty samé informace a v prohlížeči se jako odpověď opět zobrazí text "hello".

##Další kroky

Tento rychlý start předvádí velmi jednoduché provedení základní funkce protokolu HTTP aktivované triggerem. Další informace o využití možností Azure Functions v aplikacích najdete v těchto tématech.

+ [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
+ [Testování funkcí Azure Functions](functions-test-a-function.md)  
Toto téma popisuje různé nástroje a techniky pro testování funkcí.
+ [Postup škálování Azure Functions](functions-scale.md)  
Toto téma popisuje plány služby, které jsou dostupné s Azure Functions (včetně plánu služby Dynamic), a výběr správného plánu. 
+ [Co je Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
Azure Functions využívá platformu Azure App Service pro základní funkce, jako jsou nasazení, proměnné prostředí a diagnostika. 

[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=ago16_HO5-->



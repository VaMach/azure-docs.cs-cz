---
title: "Azure Notification Hubs – pokyny pro diagnostiku"
description: "Pokyny o tom, jak diagnostikovat běžné problémy s Azure Notification Hubs."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 32e3a2e6f840afd865375a622cfae0d33ba65090
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hubs – pokyny pro diagnostiku
## <a name="overview"></a>Přehled
Jeden z nejčastějších dotazů, které jsme od vás odpověď od zákazníků Azure Notification Hubs je jak zjistěte, proč se nezobrazí oznámení odeslaných z jejich back-end aplikace zobrazí na klientském zařízení – kde a proč byly vyřazeny oznámení a jak tento problém opravit. V tomto článku jsme se projít různých důvodů, proč oznámení může získat vyřadit ani nekončí na zařízeních. Podíváme se také prostřednictvím způsoby, ve kterém můžete analyzovat a zjistěte hlavní příčinu. 

První řadě je zásadně důležité pochopit, jak Azure Notification Hubs doručí se oznámení do zařízení.
![][0]

V toku typické odesílání oznámení, je zpráva odeslána z **back-end aplikace** k **Azure oznámení centra (NH)** na které pak se některé zpracování na všechny registrace vezme v úvahu nakonfigurované značky & značky výrazy k určení "cíle" znamená všechny registrace, které je potřeba příjem nabízených oznámení. Mezi některé nebo všechny naše podporované platformy – iOS, Google, Windows, Windows Phone, může mít rozsah tyto registrace Kindle a Baidu pro Android v Číně. Po vytvoření cíle jsou NH pak nabízených oznámení se oznámení, rozdělit do několika batch registrací pro konkrétní platformu zařízení **služby nabízených oznámení (PNS)** – např. pro Apple APNS, GCM pro Google atd. NH ověřuje s příslušnými systém PNS podle přihlašovacích údajů, které se nastavují v portálu Azure Classic na stránce Konfigurace centra oznámení. Systém PNS pak předá oznámení do příslušné **klientských zařízení**. Toto je platforma doporučená způsob, jak doručovat nabízená oznámení a Všimněte si, že konečné fáze doručení oznámení dochází mezi Správou platformy a zařízení. Proto máme čtyři hlavní součásti - *klienta*, *back-end aplikace*, *Azure Notification Hubs (NH)* a *služby nabízených oznámení (PNS)* a některý z těchto může způsobit oznámení získávání vyřadit. Další podrobnosti o této architektury je k dispozici na [přehledu této služby].

Nepodařilo se doručit oznámení mohlo dojít během počáteční testovací nebo pracovní fáze, které mohou znamenat problém konfigurace nebo k tomu může dojít v produkčním prostředí, kde všechny nebo některé z oznámení může být získávání vynechaných uvádí některé hlubší aplikace nebo vzor problém pro zasílání zpráv. V části níže se podíváme na různé scénáře vynechaných oznámení od běžné vzácnějších druh, některé z nich může být zřejmé a jiná není mnoho. 

## <a name="azure-notifications-hub-mis-configuration"></a>Chybná konfigurace centra oznámení Azure
Centra oznámení Azure potřebuje ke svému ověření v kontextu vývojáře pro aplikace, abyste mohli úspěšně odesílat oznámení do příslušných systém PNS. To je umožněno vytvoření vývojářského účtu pomocí příslušné platformy (Google, Apple Windows atd.) a pak registraci své aplikace, kde získat přihlašovací údaje, které je třeba nakonfigurovat v části Konfigurace centra oznámení na portálu vývojáře. Pokud se prostřednictvím žádná oznámení, by mělo být prvním krokem správné přihlašovací údaje musí být nastaveny v centru oznámení zjišťování shody s aplikací vytvořil v rámci svého účtu vývojáře konkrétní platformu. Zjistíte, naše [získávání kurzů] užitečné projít tento proces způsobem krok za krokem. Zde jsou některé běžné chybné konfigurace:

1. **Obecné**
   
    (a) ujistěte se, že je vaším jménem centra oznámení (bez překlepům) stejný:
   
   * Pokud registrujete z klienta, 
   * Kde jsou odesílání oznámení z back-end,  
   * Kde jste nakonfigurovali přihlašovací údaje systému PNS a 
   * Jehož pověření SAS jste nakonfigurovali na klientovi a back-end. 
     
     b) ujistěte se, že používáte správné konfigurace řetězce SAS na klientovi a back-end aplikace. Jako existuje pravidlo, je nutné použít **DefaultListenSharedAccessSignature** na straně klienta a **DefaultFullSharedAccessSignature** na back-end aplikace, (který poskytuje oprávnění, abyste mohli odeslat oznámení NH)
2. **Konfigurace Apple Push Notification Service (APNS)**
   
    Musíte udržovat dvě různé centra – jednu pro produkční a druhý pro testování účel. To znamená, odesílání na certifikát, který chcete použít v prostředí izolovaného prostoru k rozbočovači samostatné a certifikát, který chcete použít v produkčním prostředí k rozbočovači samostatné. Nepokoušejte se nahrát různé typy certifikátů ke stejnému rozbočovači, jak může způsobit selhání oznámení dolů na řádku. Pokud na pozici, kde jste omylem odeslali různé typy certifikátů na stejném centru najít sami, se doporučuje odstranit rozbočovače a začít pracovat. Pokud z nějakého důvodu, nejste schopni odstranit centrum pak v každém, je nutné odstranit všechny existující registrace z rozbočovače. 
3. **Konfigurace zasílání zpráv cloudu Google (GCM)** 
   
    (a) ujistěte se, že povolíte "Google Cloud Messaging pro Android" v části projektu cloudu. 
   
    ![][2]
   
    b) zkontrolujte vytvořte klíč"Server" při získání přihlašovacích údajů, které NH bude používat k ověření pomocí služby GCM. 
   
    ![][3]
   
    c) ujistěte se, že jste nakonfigurovali v klientovi, který je zcela číselné entita, která můžete získat z řídicího panelu "Projektu ID":
   
    ![][1]

## <a name="application-issues"></a>Problémy s aplikací
1) **Značky / značka výrazy**

Pokud používáte značky nebo značky výrazy segmentovat cílovou skupinu, vždycky je možné, že při odesílání oznámení, že žádný cíl byl nalezen založené na výrazech značky nebo značky, který určujete v odesílání volání. Je vhodné zkontrolovat vaše registrace do zajistěte, aby značky, které se shodují při odesílání oznámení a ověří přijetí oznámení pouze z klientů s tyto registrace. Například Pokud všechny vaše registrace s NH měla pracovat vyslovení značka "Politika" a odesílání oznámení pomocí značky "Sports", nebudou odeslány na jakékoli zařízení. Komplexní případu může patřit značky výrazy, kde můžete registrovat jenom s "Značky A" nebo "Značky B", ale při odesílání oznámení, kterou cílíte "Značky A a a značky B". V následující části Samoobslužné diagnostikovat tipy jsou způsoby, ve kterém můžete zkontrolovat vaše registrace společně se značkami, které mají. 

2) **Problémy se šablonou**

Pokud používáte šablony a ujistěte se, že jsou následující podle pokynů popsaných na [šablony pokyny]. 

3) **Neplatný registrace**

Za předpokladu, že Centrum oznámení byla nakonfigurována správně a žádný z výrazů značky nebo značky používaly správně výsledkem najít platný cíle, do kterých muset odeslat oznámení, NH vyvolá několik zpracování dávky paralelně - každé dávce, odesílání zpráv na sadu registrací. 

> [!NOTE]
> Vzhledem k tomu, že jsme provést paralelní zpracování, jsme nezaručují pořadí, ve kterém budou doručeny oznámení. 
> 
> 

Nyní centra oznámení Azure je optimalizovaná pro model doručení zprávy "jednou na většinu". To znamená, jsme pokus deaktivace duplikace tak, aby žádná oznámení se doručují více než jednou na zařízení. Aby to jsme projděte registrace a ujistěte se, že pouze jeden zprávy podle identifikátoru zařízení před skutečného odeslání zprávy do systém PNS. Každé dávky je odeslán systém PNS, který naopak je přijetí a ověření registrace, je možné, že systém PNS zjistí chybu s jedním nebo více registrací v dávce, vrátí chybu Azure NH a ukončí zpracování, a tím zcela vyřazení tohoto batch. To platí hlavně službou APNS, který používá protokol TCP datového proudu. I když jsme jsou optimalizované pro na většinu po doručení, v tomto případě jsme odebrat chybující registraci z našich databáze a opakujte odeslání oznámení pro zbytek zařízení v této dávce.

Můžete získat informace o chybě při pokusu o doručení selhání proti registrace pomocí rozhraní API REST centra oznámení Azure: [za zpráva Telemetrie: získání Telemetrických zpráv oznámení](https://msdn.microsoft.com/library/azure/mt608135.aspx) a [zpětnou vazbu systém PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). Najdete v článku [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) například kódu.

## <a name="pns-issues"></a>Systém PNS problémy
Jakmile byla přijata zpráva oznámení pomocí příslušných systém PNS je jeho odpovědností pro doručení oznámení do zařízení. Azure Notification Hubs je mimo zde na obrázku a nemá žádnou kontrolu případě nebo pokud oznámení má doručit do zařízení. Vzhledem k tomu, že se poměrně robustní služby platformy oznámení, oznámení mívají k dosažení zařízení během několika sekund z systém PNS. Pokud ale omezení systém PNS pak Azure Notification Hubs použijte exponenciální regrese strategie a pokud systém PNS zůstává nedostupný pro 30 min potom jsme zavedené zásady vypršení platnosti a trvale vyřadit tyto zprávy. 

Pokud systém PNS pokusí doručit oznámení, ale zařízení je offline, je oznámení ukládaná systém PNS po omezenou dobu a doručit do zařízení, až bude k dispozici. Je uložena pouze jedno poslední oznámení pro konkrétní aplikace. Pokud více oznámení se odesílají, když je zařízení offline, každé nové oznámení způsobí, že předchozí oznámení budou zahozeny. Toto chování, aby pouze nejnovější oznámení se označuje jako slučování oznámení v APNS a sbalení v GCM (který se používá ztenčeného klíč). Pokud zařízení zůstane v režimu offline delší dobu, žádné oznámení, které jste právě uložili pro něj se zahodí. Source – [APNS pokyny] & [GCM pokyny]

S Azure Notification Hubs – můžete předat slučování klíč prostřednictvím záhlaví HTTP pomocí Obecné `SendNotification` rozhraní API (např. v případě sady .NET SDK – `SendNotificationAsync`) což také trvá hlavičky protokolu HTTP, které jsou předány jako má odpovídající systém PNS. 

## <a name="self-diagnose-tips"></a>Samoobslužné diagnostikovat tipy
Zde vyzkoušíme různé cesty pro diagnostiku a kořenový způsobit problémy s Centrum oznámení:

### <a name="verify-credentials"></a>Ověření přihlašovacích údajů
1. **Systém PNS portál pro vývojáře**
   
    Ověření je na příslušné systém PNS developer portal (GCM, APNS WNS atd) pomocí našich [získávání kurzů].
2. **Portál Azure Classic**
   
    Přejděte na kartu Konfigurace a zkontrolujte shodovat s pověřeními získanými z portálu pro vývojáře systému PNS. 
   
    ![][4]

### <a name="verify-registrations"></a>Ověření registrace
1. **Visual Studio**
   
    Pokud používáte Visual Studio pro vývoj můžete připojit k Microsoft Azure a zobrazení a správa bunch služeb Azure včetně centra oznámení z "Průzkumníka serveru". To je užitečné hlavně pro vaše prostředí pro vývoj/testování. 
   
    ![][9]
   
    Můžete zobrazit a spravovat všechny registrace v centru, které jsou klasifikovány vhodně pro platformu, nativní nebo šablony registrace, všechny značky, systém PNS identifikátor, id registrace a datum vypršení platnosti. Můžete taky upravit registraci za chodu – což je užitečné, například pokud chcete upravit všechny značky. 
   
    ![][8]
   
   > [!NOTE]
   > Visual Studio funkce Upravit registrace lze používat pouze během vývoje/testování s omezený počet registrací. Pokud je potřeba opravit vaší registrace hromadné potřeby zvažte použití exportu/importu registrace funkcí popsaných v tomto poli - [exportu/importu registrace](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Průzkumník služby Service Bus**
   
    Mnoho zákazníků použit sběrnice explorer popsané v tomto - [Explorer sběrnice] pro zobrazování a správu jejich centra oznámení. Je k dispozici z code.microsoft.com - opensourcový projekt [sběrnice Průzkumník kódu]

### <a name="verify-message-notifications"></a>Ověřte oznamování pomocí zpráv
1. **portál Azure Classic**
   
    Můžete přejít na kartu "Debug" Odeslat zkušební oznámení pro klienty bez nutnosti jakéhokoli back-endu služby nahoru a spouštění. 
   
    ![][7]
2. **Visual Studio**
   
    Můžete také odeslat zkušební oznámení z comforts sady Visual Studio:
   
    ![][10]
   
    Další informace o funkcích Průzkumníka Visual Studio oznámení centra Azure tady - 
   
   * [Přehled Průzkumníka serveru VS]
   * [Příspěvek blogu Průzkumníka serveru VS - 1]
   * [Příspěvek blogu Průzkumníka serveru VS - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Ladění chybných oznámení / zkontrolujte výsledek oznámení
**Vlastnost EnableTestSend**

Při odesílání oznámení prostřednictvím centra oznámení, nejdřív ho jenom získá zařazen do fronty pro NH udělat zpracování a pokuste se zjistit všechny jeho cíle a pak nakonec NH odešle ji do systém PNS. To znamená, že při použití rozhraní REST API nebo některou z klienta SDK, úspěšné návrat volání odesílání znamená pouze to, že zpráva má byl úspěšně zařazen do fronty pomocí centra oznámení. Nedává ho lépe pochopit, co se stalo při NH nakonec tu k odeslání zprávy do systému PNS. Pokud není oznámení přicházejících na klientském zařízení, je možné, že při pokusu o doručení zprávy do systému PNS NH, došlo k chybě, například velikost datové části překročila maximální povolenou systém PNS nebo nakonfigurované v NH přihlašovací údaje jsou neplatné atd. Pokud chcete získat přehled o systém PNS chyby, jsme zavedli vlastnost s názvem [EnableTestSend funkce]. Tato vlastnost je automaticky povolen při odesílání zkušebních zpráv z portálu nebo klienta Visual Studia a proto vám umožní zobrazit podrobné informace o ladění. Můžete použít prostřednictvím rozhraní API trvá v příkladu .NET SDK tam, kde je k dispozici nyní a přidá všechny klientské sady SDK nakonec. Používat toto volání REST, jednoduše připojte querystring parametr s názvem "test" na konci volání odesílání například 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Příklad (.NET SDK)*

Předpokládejme, že jsou pomocí sady .NET SDK k odesílání oznámení s informační zprávou nativní:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`bude jednoduše stavu `Enqueued` na konci provádění bez všechny aspekty co se stalo s vaší push. Nyní můžete pomocí `EnableTestSend` vlastnost typu boolean při inicializaci `NotificationHubClient` a může získat podrobné informace o stavu týkající se Správou chyb došlo při odesílání oznámení. Sem odeslání volání prodlouží dobu vrátit vzhledem k tomu, že ji vrací pouze po NH má doručit oznámení na systém oznámení platformy a určit výsledek. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Ukázkový výstup*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Tato zpráva znamená buď neplatné přihlašovací údaje jsou nakonfigurované v centru oznámení nebo problém s registrací v centru a doporučené během by mohla být tato registrace odstraňte a znovu ji vytvořte před odesláním zprávy klienta. 

> [!NOTE]
> Všimněte si, že použití této vlastnosti je výraznou omezené a proto musíte použít tento v prostředí pro vývoj/testování s omezenou sadu registrací. Pouze odešleme ladění oznámení na 10 zařízení. Máme také limit zpracování ladění odešle jako 10 za minutu. 
> 
> 

### <a name="review-telemetry"></a>Zkontrolujte telemetrie
1. **Použijte portál Azure Classic**
   
    Na portálu můžete získat rychlý přehled všechny aktivity na vaše Centrum oznámení. 
   
    (a) na kartě "řídicího panelu" můžete zobrazit souhrnné zobrazení registrace, oznámení, jakož i chyby na každou platformu. 
   
    ![][5]
   
    b) můžete také přidat mnoho dalších platformy určité metriky na kartě "Sledování" provést hlubší podívejte se na konkrétní chyby systému PNS vrácené když NH pokusí odeslat oznámení systém PNS zvlášť. 
   
    ![][6]
   
    c) byste měli začít s kontrola **příchozí zprávy**, **registrace Operations**, **úspěšné oznámení** a pak přejděte na kartu platformy ke kontrole konkrétní chyby systému PNS. 
   
    d) Pokud máte centra oznámení správně nakonfigurovaný s nastavení ověřování, zobrazí se Správou chyby ověřování. Toto je dobrá indikace toho zkontrolujte přihlašovací údaje systému PNS. 

2) **Programový přístup**

Zde – podrobnosti 

* [Telemetrie programový přístup]
* [Telemetrie přístup přes rozhraní API ukázka] 

> [!NOTE]
> Několik telemetrii související funkce, jako jsou **exportu/importu registrace**, **Telemetrie přístup přes rozhraní API** jsou dostupné v úrovni Standard jenom atd. Pokud se pokusíte tyto funkce používají, pokud jste v volné nebo úroveň Basic bude získat zpráva o výjimce za tímto účelem při použití sady SDK a protokolu HTTP 403 (zakázáno) je použití přímo z rozhraní REST API. Ujistěte se, že jste přesunuli až Standard vrstvy prostřednictvím portálu Azure Classic.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[přehledu této služby]: notification-hubs-push-notification-overview.md
[získávání kurzů]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[šablony pokyny]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS pokyny]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM pokyny]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[Explorer sběrnice]: http://msdn.microsoft.com/library/dn530751.aspx
[sběrnice Průzkumník kódu]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Přehled Průzkumníka serveru VS]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Příspěvek blogu Průzkumníka serveru VS - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Příspěvek blogu Průzkumníka serveru VS - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend funkce]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Telemetrie programový přístup]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrie přístup přes rozhraní API ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel


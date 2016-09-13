<properties
    pageTitle="Azure Notification Hubs"
    description="Podívejte se, jak používat nabízená oznámení v Azure. Ukázky kódu jsou vytvořeny v C# s použitím .NET API."
    authors="wesmc7777"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="wesmc"/>


#Azure Notification Hubs

##Přehled

Služba Azure Notification Hubs poskytuje snadno použitelnou, multiplatformní a horizontálně škálovanou infrastrukturu, která vám umožní odesílat mobilní nabízená oznámení z jakékoli back-endu (cloudového nebo lokálního) na libovolnou mobilní platformu.

S Notification Hubs můžete snadno odesílat multiplatformní a personalizovaná nabízená oznámení, která překrývají rozdíly v detailním nastavení různých systémů oznámení platforem (PNS). Pomocí jednoho volání rozhraní API můžete cílit na jednotlivé uživatele nebo na celé segmenty zahrnující miliony uživatelů, a to na všech jejich zařízeních.

Notification Hubs můžete využívat ve scénářích odpovídajícím potřebám velkých firem i individuálních uživatelů. Například:

- Odesílání oznámení o aktuálních zprávách milionům příjemcům s nízkou latencí (Služba Notification Hubs využívá aplikace Bing, které jsou předinstalované na všech zařízeních s Windows a Windows Phone.)
- Odesílání kupónů závislých na aktuální poloze celým uživatelským segmentům
- Odesílání oznámení o událostech uživatelům nebo skupinám v aplikacích z oblasti sportu, financí nebo her
- Upozorňování uživatelů na firemní události, jako jsou nové zprávy či e-maily nebo prodejní tipy
- Zasílání jednorázových hesel potřebných pro vícefaktorové ověřování



##Co jsou nabízená oznámení?

Smartphony a tablety můžou „oznámit“ uživateli, že došlo k nějaké události. Tato oznámení mohou mít mnoho forem.

V aplikacích pro Windows Store a Windows Phone může mít toto oznámení podobu _informační zprávy_ (anglicky „toast“): nové oznámení se zobrazí v nemodálním okně a zazní zvukový signál. Podporovány jsou i další typy oznámení, mezi které patří _dlaždice_, _neupravené oznámení_ a _oznámení „badge“_. Další informace o typech oznámení podporovaných na zařízeních s Windows najdete v tématu [Dlaždice a oznámení](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Na zařízeních s Apple iOS se nabízené oznámení uživateli zobrazí obdobně, tzn. jako dialogové okno s výzvou k zobrazení nebo zavření oznámení. Kliknutím na **Zobrazit** uživatel otevře aplikaci, která danou zprávu přijala. Další informace o těchto oznámeních najdete v tématu [Oznámení v iOS](http://go.microsoft.com/fwlink/?LinkId=615245).

Díky nabízeným oznámením mohou mobilní zařízení zobrazovat čerstvé informace a zachovat si přitom energetickou efektivitu. Back-end systémy mohou oznámení do mobilních zařízení odesílat i v případě, že odpovídající aplikace na zařízení nejsou aktivní. Nabízená oznámení jsou důležitou součástí uživatelských aplikací, kde slouží ke zvýšení aktivity a využití ze strany uživatelů. Oznámení jsou také užitečná pro firmy, kde aktuální informace zvyšují schopnost zaměstnanců reagovat na obchodní události.

Zde je několik konkrétních scénářů, jak přispívají k lepšímu využití mobilních zařízení:

1.  Zobrazení aktuálních finančních informací na dlaždici ve Windows 8 nebo Windows Phone
2.  Upozornění uživatele formou informační zprávy, že mu byla přiřazena určitá pracovní položka, a to vše v rámci firemní aplikace založené na pracovním postupu
3.  Zobrazení oznámení „badge“ s aktuálním počtem prodejních tipů v aplikaci CRM (jako je Microsoft Dynamics CRM)

##Jak nabízená oznámení fungují

Nabízená oznámení se doručují prostřednictvím infrastruktur specifických pro platformy, které se označují jako _systémy oznámení platforem_ (PNS). Systém PNS nabízí jen nejzákladnější funkce (to znamená, že nepodporuje vysílání či přizpůsobení) a nemá žádné společné rozhraní. Pokud například chcete odeslat oznámení aplikaci pro Windows Store, vývojář musí kontaktovat službu WNS (Windows Notification Service). Stejný vývojář musí při odesílání oznámení do zařízení s iOS kontaktovat službu APNS (Apple Push Notification Service) a odeslat zprávu podruhé. Služba Azure Notification Hubs to usnadňuje tím, že poskytuje společné rozhraní a další funkce pro podporu nabízených oznámení napříč platformami.

Avšak na nejvyšší úrovni všechny systémy oznámení platforem postupují podle stejného vzoru:

1.  Klientská aplikace kontaktuje systém oznámení platformy a načte si jeho _popisovač_. Typ popisovače závisí na systému. U WNS je to identifikátor URI nebo „kanál pro oznámení“. U APNS jde o token.
2.  Klientská aplikace si tento popisovač uloží v _back-endu_ aplikace pro pozdější použití. U WNS je back-endem obvykle cloudová služba. U Applu se tento systém označuje jako _„provider“_.
3.  Pokud chcete odeslat nabízené oznámení, back-end aplikace kontaktuje systém PNS s použitím popisovače, který odpovídá konkrétní instanci klientské aplikace.
4.  Systém PNS předá oznámení do zařízení určeného popisovačem.

![][0]

##Obtíže spojené s nabízenými oznámeními

Tyto systémy jsou sice velmi výkonné, nadále však ponechávají spoustu práce na vývojáři aplikace, a to i při implementaci běžných scénářů, jako je například vysílání nabízených oznámení nebo jejich odeslání uživatelům v jednotlivých segmentech.

Nabízená oznámení jsou jednou z nejžádanějších funkcí v cloudových službách pro mobilní aplikace. Důvodem je to, že infrastruktura potřebná pro jejich fungování je poměrně složitá a z velké části nemá vztah k hlavní obchodní logice aplikace. Zde jsou některé z problémů při vytváření infrastruktury nabízených oznámení na vyžádání:

- **Závislost na platformě:** Aby bylo možné odesílat oznámení do zařízení na různých platformách, musí být v back-endu kódováno více rozhraní. Nejenže se liší v detailech nastavení, ale na platformě je závislý dokonce i způsob prezentace oznámení (dlaždice, informační zpráva nebo oznámení „badge“). Tyto rozdíly by mohly vést k tomu, že back-endový kód bude velmi složitý a jeho údržba bude obtížná.

- **Škálování:** Škálování této infrastruktury má dva aspekty:
    + Podle pokynů pro systém PNS je nutné tokeny zařízení obnovovat při každém spuštění aplikace. To vede k velkému objemu přenosů (a následným přístupům do databáze) jen v souvislosti udržováním tokenů zařízení v aktuálním stavu. Když počet zařízení naroste (případně až na miliony), náklady na vytvoření a údržbu této infrastruktury nejsou zanedbatelné.

    + Většina systému PNS nepodporuje vysílání na více zařízení. Z toho vyplývá, že při vysílání na miliony zařízení se produkují miliony volání do systémů PNS. Schopnost škálovat podle těchto požadavků není triviální, protože vývojáři aplikací obvykle chtějí udržet nízkou celkovou latenci. Například by zpráva neměla na poslední zařízení, které ji dostane, přijít po více než 30 minutách od odeslání oznámení, protože v mnoha případech by vůbec nemělo smysl nabízená oznámení používat.
- **Směrování:** Systémy PNS poskytují způsob, jak odeslat zprávu do zařízení. Ve většině aplikací jsou však oznámení určena pro určité uživatele nebo specifické skupiny (například všechny zaměstnance přiřazené k určitému zákaznickému účtu). Aby tedy bylo možné oznámení směrovat na správná zařízení, musí back-end aplikace udržovat registr, v němž jsou konkrétní uživatelské skupiny přidruženy k tokenům zařízení. Tato dodatečná režie prodlužuje celkovou dobu nutnou pro publikování a navyšuje náklady na údržbu aplikace.

##Proč používat Notification Hubs?

Služba Notification Hubs eliminuje složitost: není nutné překonávat zmíněné výzvy spojené s nabízenými oznámeními. Místo toho můžete použít centrum oznámení. Služba Notification Hubs využívá multiplatformní a horizontálně škálovanou infrastrukturu pro nabízená oznámení a výrazně redukuje kód specifický pro jejich odesílání, který musí běžet na back-endu aplikace. Služba Notification Hubs implementuje všechny funkce infrastruktury nabízených oznámení. Zařízení zodpovídají pouze za registraci popisovačů systému PNS a back-end zodpovídá za zasílání zpráv, které jsou nezávislé na platformě, uživatelům nebo uživatelským skupinám, jak je znázorněno na následujícím obrázku:

![][1]


Služba Notification Hubs poskytuje infrastrukturu nabízených oznámení připravenou k použití, která má následující výhody:

- **Multiplatformní použití:**
    +  Podpora pro všechny hlavní mobilní platformy. Služba Notification Hubs může nabízená oznámení odesílat do aplikací pro Windows Store, iOS, Android a Windows Phone.

    +  Služba Notification Hubs poskytuje společné rozhraní pro odesílání oznámení na všechny podporované platformy. Nejsou nutné protokoly specifické pro platformy. Back-end aplikace může nabízená oznámení odesílat ve formátech specifických pro platformu, nebo nezávislých na platformě. Aplikace komunikuje pouze se službou Notification Hubs.

    +  Správa popisovačů zařízení. Služba Notification Hubs udržuje registr popisovačů a zpětnou vazbu ze systémů PNS.

- **Možnost použití s libovolným back-endem**: cloudový nebo lokální, .NET, PHP, Java, Node atd.

- **Škálování:** Služba Notification Hubs provádí škálování na miliony zařízení bez nutnosti přepracování nebo komplikovaného skládání architektury.


- **Bohatá sada schémat doručování**:

    - *Vysílání*: Umožňuje téměř simultánní vysílání na miliony zařízení na základě jednoho volání rozhraní API.

    - *Jednosměrové a vícesměrové vysílání*: Oznámení můžete odesílat na základě značek představujících jednotlivé uživatele, což bude zahrnovat všechna jejich zařízení; nebo pro širší skupinu, například pro různé typy zařízení (tablet versus telefon).

    - *Segmentace*: Oznámení můžete odesílat složitým segmentům definovaným výrazy ve značkách (například na všechna zařízení v Praze, která sledují určitý fotbalový tým).

    Každé zařízení může při odesílání svého popisovače do centra oznámení uvést jednu nebo více _značek_. Projděte si další informace o [značek]. Značky není třeba předem zřizovat nebo nastavovat. Značky poskytují jednoduchý způsob, jak oznámení odesílat určitým uživatelům nebo skupinám. Vzhledem k tomu, že značky mohou obsahovat identifikátor specifický pro aplikaci (například ID uživatele nebo skupiny), je díky jejich použití back-end aplikace zbaven povinnosti uchovávat a spravovat popisovače zařízení.

- **Personalizace**: Každé zařízení může mít jednu nebo více šablon pro zajištění lokalizace a personalizace podle zařízení, aniž by to mělo dopad na kód back-endu.

- **Zabezpečení**: Sdílený tajný přístupový klíč (SAS) nebo federovaného ověřování.

- **Bohatá telemetrie**: K dispozici na portálu a programově.


##Integrace s App Service Mobile Apps

Pro zajištění plynulého a sjednocujícího prostředí napříč službami Azure nabízí [App Service Mobile Apps] integrovanou podporu pro nabízená oznámení prostřednictvím služby Notification Hubs. Služba [App Service Mobile Apps] nabízí vysoce škálovatelnou a globálně dostupnou platformu pro vývoj mobilních aplikací určenou pro vývojáře a integrátory systémů ve velkých firmách. Přináší bohatou sadu funkcí pro vývojáře pro mobilní zařízení.

Vývojáři v Mobile Apps mohou službu Notification Hubs využívat v rámci následujícího pracovního postupu:

1. Načtení popisovače systému PNS zařízení
2. Registrace zařízení a [šablon] v Notification Hubs prostřednictvím snadno použitelného rozhraní API pro registraci Mobile Apps Client SDK
    + Mějte na paměti, že služba Mobile Apps odstraní z bezpečnostních důvodů při registraci všechny značky. Služba Notification Hubs vám umožní přiřadit značky k zařízením přímo z back-endu.
3. Odesílání oznámení z back-endu aplikace pomocí Notification Hubs

Zde jsou některé výhody, které vývojáři získají díky této integraci:

- **Sady Mobile Apps Client SDK:** Tyto multiplatformní sady SDK poskytují jednoduchá rozhraní API pro registraci a automaticky komunikují s centrem oznámení propojeným s touto mobilní aplikací. Vývojáři se nemusí zabývat přihlašovacími údaji pro Notification Hubs a pracovat s další službou.
    + Tyto sady SDK automaticky označí dané zařízení ověřeným ID uživatele Mobile Apps, díky čemuž je možné uskutečnit scénář nabízených oznámení pro uživatele.
    + Sady SDK automaticky používají instalační ID Mobile Apps jako identifikátor GUID pro registraci v Notification Hubs, což vývojáře zbavuje nutnosti starat se o identifikátory GUID pro několik služeb.
    
- **Instalační model:** Služba Mobile Apps pracuje s nejnovějším modelem nabízených oznámení v Notification Hubs, který reprezentuje všechny vlastnosti nabízených oznámení související se zařízením v instalaci JSON, což vyhovuje Službě nabízených oznámení a nabízí snadné použití.

- **Flexibilita:** Vývojáři se vždy mohou rozhodnout pracovat přímo s Notification Hubs, a to i když je tato služba integrována.

- **Integrované prostředí na webu [Azure Portal].** Nabízená oznámení jsou v Mobile Apps vizuálně reprezentována jako funkce a vývojáři mohou přes Mobile Apps snadno pracovat s přidruženým centrem oznámení.



##Další kroky

Další informace o Notification Hubs naleznete v těchto tématech:

+ **[Jak zákazníci používají Notification Hubs]**

+ **[Kurzy a příručky k Notification Hubs]**

+ **Kurzy Začínáme s Notification Hubs** ([iOS], [Android], [Univerzální pro Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Zde jsou odkazy na relevantní spravovaná rozhraní API pro .NET:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Jak zákazníci používají Notification Hubs]: http://azure.microsoft.com/services/notification-hubs
  [Kurzy a příručky k Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Univerzální pro Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [šablon]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure Portal]: https://portal.azure.com
  [značek]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)



<!--HONumber=sep16_HO1-->



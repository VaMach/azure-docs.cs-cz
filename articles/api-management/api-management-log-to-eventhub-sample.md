---
title: "Sledování rozhraní API pomocí Azure API Management, Event Hubs a Runscope | Microsoft Docs"
description: "Ukázkovou aplikaci ukázka zásad protokolu eventhub připojování Azure API Management, Azure Event Hubs a Runscope pro protokol HTTP, protokolování a monitorování"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 70ee752c5639c90f77dde104ce85eec0a1062300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope
[Služba API Management](api-management-key-concepts.md) poskytuje mnoho možností pro zlepšení zpracování požadavky HTTP odeslané na rozhraní API HTTP. Existenci požadavky a odpovědi jsou však přechodný. Zadání požadavku a ven prochází přes službu API Management na váš back-end rozhraní API. Rozhraní API zpracuje požadavek a odpověď toků zpátky pomocí rozhraní API příjemci. Služba API Management udržuje některých důležitých statistik o rozhraní API pro zobrazení v řídicím panelu portálu vydavatele, ale i mimo, že podrobnosti jsou pryč.

Pomocí [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zásad](api-management-howto-policies.md) ve službě API Management můžete odesílat žádné informace z požadavku a odpovědi na [centra událostí Azure](../event-hubs/event-hubs-what-is-event-hubs.md). Existuje mnoho různých důvodů, proč můžete chtít generovat události z protokolu HTTP zprávy odesílané do vašeho rozhraní API. Mezi příklady patří záznam pro audit aktualizací, analýzy využití, výstrahy výjimek a integrace 3. stran.   

Tento článek ukazuje, jak k zaznamenání celé zprávy požadavku a odpovědi protokolu HTTP, odešle do centra událostí a pak tuto zprávu třetí straně služba, která poskytuje HTTP protokolování a monitorování služeb předávání.

## <a name="why-send-from-api-management-service"></a>Proč odeslat z služby API Management?
Je možné zapisovat middleware HTTP, který můžete připojit k rozhraní HTTP API k zaznamenání požadavky a odpovědi HTTP a kanálu je do protokolování a monitorování systémů. Nevýhodou tento přístup je HTTP middleware musí být integrovaná do rozhraní API back-end a platformou rozhraní API se musí shodovat. Pokud máte více rozhraní API, musíte nasadit každé z nich middleware. Často existuje několik důvodů, proč nelze aktualizovat back-end rozhraní API.

Pomocí služby Azure API Management integrovat s infrastrukturou protokolování poskytuje centralizovaný a nezávislé na platformě řešení. Je také škálovatelná částečně kvůli a [geografická replikace](api-management-howto-deploy-multi-region.md) možnosti služby Azure API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Proč odeslat do centra událostí Azure?
Je možné logicky požádat, proč vytvořit zásadu, která je specifická pro Azure Event Hubs? Existuje mnoho různých místech, kde může chcete protokolu Moje žádosti. Proč právě neodesílal žádosti přímo do konečného umístění?  To je možnost. Při provádění protokolování požadavky od služby API management, je však nutné vzít v úvahu, jak protokolování zpráv ovlivní výkon rozhraní API. Postupná nárůst zatížení lze zpracovávat zvýšením dostupných instancí komponent systému nebo přímým geografická replikace. Krátký špičky v provozu však může způsobit žádosti o výrazně odloží Pokud požadavky na infrastrukturu protokolování spustit zpomalit zatížení.

Azure Event Hubs je určena pro příjem příchozích dat obrovské objemy dat, s kapacitou pro plánování práce s daleko vyšší počet událostí, než počet požadavků HTTP většina proces rozhraní API. Centra událostí funguje jako sofistikované vyrovnávací paměti mezi služby API management a infrastruktury, který bude ukládat a zpracovávat zprávy. Tím se zajistí, že nebude z důvodu protokolování infrastruktury sníží výkon vašich rozhraní API.  

Jakmile data byla předána do centra událostí je je trvalá a bude čekat centra událostí příjemci zpracovat. Centra událostí nezáleží na tom, jak bude zpracována, ho záleží jenom tak, že budou úspěšně doručovat zprávy.     

Služba Event Hubs mít možnost datového proudu událostí do několika skupin uživatelů. To umožňuje událostí ke zpracování úplně jiné systémy. To umožňuje podporovat mnoho scénáře integrace bez uvedení Přidání zpoždění na zpracování požadavku rozhraní API v rámci služby API Management, jako je třeba vytvořit pouze jednu událost.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Zásada pro odesílání zpráv application/http
Centra událostí přijme data událostí jako jednoduchý řetězec. Obsah tento řetězec je zcela na vás. Abyste mohli zabalit požadavku HTTP a odesílat do centra událostí potřebujeme se naformátovat řetězec s informacemi o požadavku nebo odpovědi. V situacích, jako to pokud je existující formát můžeme opakovaně a potom jsme nemusí mít k zápisu vlastní analýze kódu. Původně I považována za použití [HAR](http://www.softwareishard.com/blog/har-12-spec/) pro odesílání požadavků a odpovědí HTTP. Tento formát je však optimalizovaná pro ukládání pořadí požadavků HTTP ve formátu JSON na základě. Obsahuje povinné prvky, které přidány nepotřebné složitější scénář předávání zpráv HTTP prostřednictvím sítě.  

Alternativní možnost byla používat `application/http` typ média, jak je popsáno v specifikace protokolu HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230). Tento typ média používá přesný stejný formát, který se používá ve skutečnosti odesílat zprávy HTTP prostřednictvím sítě, ale celá zpráva může být v těle další požadavek HTTP put. V našem případě jsme právě budete používat text jako našem zpráv k odeslání do centra událostí. Pohodlně, je analyzátor, který již existuje v [Microsoft ASP.NET Web API 2.2 klienta](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) knihovny, které můžete tento formát analyzovat a převádět je do nativního `HttpRequestMessage` a `HttpResponseMessage` objekty.

Abyste mohli vytvořit tuto zprávu, potřebujeme, abyste mohli využívat jazyka C# na základě [výrazy zásad](https://msdn.microsoft.com/library/azure/dn910913.aspx) v Azure API Management. Zde je zásady, které odešle zprávu požadavku HTTP k Azure Event Hubs.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>Zásady deklarace
Existuje několik věcí konkrétní důležité zmínit, o tomto výrazu zásad. Zásady protokolu eventhub má atribut volá protokolovač id, které odkazuje na název protokolovacího nástroje, který byl vytvořen v rámci služby API Management. Podrobnosti o tom, jak nastavit protokolovač centra událostí ve službě API Management najdete v dokumentu [jak do protokolu událostí Azure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md). Druhý atribut je volitelný parametr, který se dá pokyn centra událostí, které k uložení zpráv v oddílu. Služba Event Hubs slouží k povolení scalabilty a vyžadují minimálně dva oddíly. Seřazené doručení zpráv z jenom záruku, že se v rámci oddílu. Pokud jsme vyzvat centra událostí, ve kterém oddílu umístit zprávu, použije algoritmus kruhového dotazování distribuovat zátěž. Však mohou způsobit některé z našich zprávy, které mají být zpracovány mimo pořadí.  

### <a name="partitions"></a>Oddíly
Aby našem zpráv se dodávají k příjemce v pořadí a využívat možnosti distribuce zatížení oddílů, se rozhodli poslat jeden oddíl a zpráv odpovědí HTTP na druhý oddíl zprávy požadavků HTTP. Tím bude zajištěno jako distribučního i zatížení a jsme může zaručit, že všechny požadavky se budou v pořadí a všechny odpovědi se budou v pořadí. Je možné pro odpověď, který se má používat před odpovídající požadavku, ale jako, který se nejedná o problém, protože máme jiný mechanismus pro korelace žádosti odpovědí a víme, že požadavky vždy dřívější než odpovědi.

### <a name="http-payloads"></a>Datové části HTTP
Po sestavení `requestLine` jsme zkontrolujte, pokud by se zkrátila textu požadavku. Text žádosti se zkrátí na pouze 1024. To může být vyšší, ale jednotlivé zprávy centra událostí jsou omezeny na 256KB, takže je pravděpodobné, že některé zprávy HTTP subjekty se nevejdou do jedné zprávy. Při provádění modulu protokolování a analýza významné množství informací může být odvozen od právě řádek požadavku HTTP a hlavičky. Také mnoho žádostí o rozhraní API vrátit pouze malé subjekty a stejně tak ztrátě informační hodnotu zkrácením velké těla poměrně minimální oproti snížení přenos, zpracování a náklady na úložiště, které chcete zachovat veškerý obsah textu. Poslední OneNote o zpracování textu je, že musíme předat `true` do As<string>() metoda vzhledem k tomu, že jsme čtou text obsahu, avšak byla také chcete back-end rozhraní API, abyste mohli ke čtení textu. Předáním hodnotu PRAVDA, aby tato metoda způsobit jsme text do uložených do vyrovnávací paměti, takže lze číst ještě jednou. To je důležité si uvědomit Pokud máte rozhraní API, který nemá odesílání velmi velké soubory nebo používá dlouhým dotazováním. V těchto případech je vyhýbat se vůbec čtení textu.   

### <a name="http-headers"></a>Hlavičky protokolu HTTP
Hlavičky protokolu HTTP můžete jednoduše přenosu do formátu zprávy ve formátu pár klíč hodnota. Jsme se rozhodli nepoužijí určitá citlivé pole zabezpečení, aby se zabránilo zbytečně úniku přihlašovacích údajů. Není pravděpodobné, že klíče rozhraní API a jiná pověření budou použita pro účely analýzy. Pokud nám chcete provést analýzu na uživatele a konkrétní produkt používají pak jsme může dojít, která z `context` objektu a přidat ke zprávě.     

### <a name="message-metadata"></a>Zpráva metadat
Při vytváření dokončení zpráva k odeslání do centra událostí, první řádek není ve skutečnosti součástí `application/http` zprávy. První řádek je tvořený, zda je daná zpráva požadavku nebo odpovědi zprávu a id zprávy, který se používá ke koordinaci požadavků na odpovědi na dalších metadat. Id zprávy je vytvořená pomocí jiné zásady, které vypadá takto:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Jsme může mít vytvořil zprávu požadavku, který uložené v proměnné, dokud odpověď se vrátí a jednoduše odeslaný žádosti a odpovědi jako do jedné zprávy. Ale odesílání žádostí a odpovědí nezávisle a použitím id zprávy ke korelaci dvou, se nám získat o něco větší flexibilitu v velikost zprávy, umožňuje využít výhod více oddílů, zatímco zachování pořadí zpráv a požadavek se zobrazí v našem protokolování řídicí panel dříve. Je také možné některých scénářích, kdy platnou odpověď se nikdy neodesílá do centra událostí, pravděpodobně z důvodu chyby závažná žádost ve službě API Management, ale stále pomůžeme záznam požadavku.

Zásadu odeslat zprávu odpovědi HTTP velmi podobná na žádost a tak konfiguraci dokončení zásad vypadá takto:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

`set-variable` Zásad vytvoří hodnotu, která je přístupný pro oba `log-to-eventhub` zásad v `<inbound>` části a `<outbound>` části.  

## <a name="receiving-events-from-event-hubs"></a>Přijímání události ze služby Event Hubs
Přijetí události z centra událostí Azure pomocí [protokolu AMQP](http://www.amqp.org/). Tým Microsoft Service Bus provedli klientské knihovny, které jsou k dispozici pro usnadnění náročné události. Existují dva různé přístupy, které jsou podporovány, jednu, která má být *přímý příjemce* a druhý je použití `EventProcessorHost` třídy. Příklady tyto dva přístupy lze nalézt v [Průvodce programováním centra událostí](../event-hubs/event-hubs-programming-guide.md). Je zkrácený rozdíly, `Direct Consumer` umožňuje úplnou kontrolu a `EventProcessorHost` nepodporuje některé úkoly vložení pro ale díky předem určité domněnky o tom, jak bude zpracovávat události.  

### <a name="eventprocessorhost"></a>EventProcessorHost
V této ukázce používáme `EventProcessorHost` pro jednoduchost, ale může není nejlepší volbou pro tento konkrétní scénář. `EventProcessorHost`nemá náročné práce tak, že nemusíte si dělat starosti o problémy v rámci třídy procesoru určitá událost dělení na vlákna. Přesto však v tomto scénáři jsou jednoduše převod zprávy do jiného formátu a předání podél do jiné služby pomocí asynchronní metody. Není nutné pro aktualizaci sdíleného stavu a proto žádné riziko problémy dělení na vlákna. Pro většinu scénářů `EventProcessorHost` je pravděpodobně nejlepší volbou a je určitě jednodušší možnost.     

### <a name="ieventprocessor"></a>IEventProcessor
Při použití centrální koncept `EventProcessorHost` je vytvoření implementace `IEventProcessor` rozhraní, které obsahuje metodu `ProcessEventAsync`. Zobrazí se zde je zásadní podpora této metody:

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

Seznam objektů EventData se předávají do metody a jsme iterace v tomto seznamu. Počet bajtů jednotlivých metod jsou analyzovány do HttpMessage objektu a tento objekt je předána instanci IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Instance obsahuje tři druhy dat:

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

`HttpMessage` Instance obsahuje `MessageId` identifikátor GUID, který umožňuje nám se připojit k odpovídající odpověď HTTP a logickou hodnotu, která označuje, jestli objekt obsahuje instanci objektu HttpRequestMessage a objekt HttpResponseMessage požadavek HTTP. Pomocí předdefinovaných ve třídách HTTP z `System.Net.Http`, bylo možné využívat výhod `application/http` analýza kódu, který je součástí `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Instance je předán implementace `IHttpMessageProcessor` což je rozhraní po vytvoření oddělit přijetí a interpretace události z centra událostí Azure a vlastní zpracování ho.

## <a name="forwarding-the-http-message"></a>Předávání zpráv protokolu HTTP
Tato ukázka rozhodli je zajímavé nabízená požadavku HTTP přes [Runscope](http://www.runscope.com). Runscope je cloudové služby, který se specializuje na protokolu HTTP, ladění, protokolování a monitorování. Mají volné vrstvy, takže je snadné a zkuste to a umožňuje nám najdete v části požadavky HTTP v reálném čase předávaných mezi naše služba API Management.

`IHttpMessageProcessor` Implementace vypadá to,

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent to Runscope");
   }
}
```

Bylo možné využívat [existující klientské knihovny pro Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) který usnadňuje nabízené `HttpRequestMessage` a `HttpResponseMessage` instance až do své služby. Chcete-li získat přístup k rozhraní API Runscope budete potřebovat účet a klíč rozhraní API. Pokyny pro získání klíč rozhraní API naleznete v [vytvoření aplikace API Runscope přístup](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) záznam dění na monitoru.

## <a name="complete-sample"></a>Ucelenou ukázku
[Zdrojový kód](https://github.com/darrelmiller/ApimEventProcessor) a testy pro ukázce na Githubu. Budete potřebovat [služby API Management](api-management-get-started.md), [připojeného centra událostí](api-management-howto-log-event-hubs.md)a [účet úložiště](../storage/common/storage-create-storage-account.md) ke spuštění ukázky sami.   

Ukázka je stejně jednoduché konzolovou aplikaci, která naslouchá pro události pocházející z centra událostí, je do převede `HttpRequestMessage` a `HttpResponseMessage` objekty a předává je na rozhraní API Runscope.

Na následujícím obrázku animovaný se zobrazí žádost o odkazy na rozhraní API v portálu pro vývojáře, konzolové aplikace zobrazuje zprávy přijaté, zpracován a předávat a pak požadavku a odpovědi zobrazovat na inspector Runscope provoz.

![Ukázka požadavku předávaná Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Souhrn
Služba Azure API Management poskytuje ideální místo pro zachycení provozu HTTP cestách do a z vašich rozhraní API. Azure Event Hubs je vysoce škálovatelnou a nenákladné řešení pro zaznamenání tento přenos a vložené ho do sekundární zpracování dat pro protokolování, sledování a další pokročilé analýzy. Připojení k monitorování systémů jako Runscope je jednoduchý jako několik desítek řádků kódu 3. stran provoz.

## <a name="next-steps"></a>Další kroky
* Další informace o Azure Event Hubs
  * [Začínáme s Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Přijímat zprávy pomocí třídy EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Průvodce programováním pro službu Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Další informace o integraci API Management a služby Event Hubs
  * [Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management](api-management-howto-log-event-hubs.md)
  * [Odkaz na entitu protokolovacího nástroje](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [referenční informace o protokolu eventhub zásad](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)

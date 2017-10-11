---
title: "Azure Service Bus ověřování s podpisy sdíleného přístupu | Microsoft Docs"
description: "Přehled ověřování Service Bus pomocí sdílené přístupové podpisy přehled, podrobnosti o ověřování SAS s Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a2760072acb7c62204759f3ec0d3cb9899460f2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Ověřování služby Service Bus s podpisy sdíleného přístupu

*Sdílené přístupové podpisy* (SAS) jsou primární zabezpečení mechanismus pro zasílání zpráv Service Bus. Tento článek popisuje SAS, jak fungují a jak je používat způsobem, bez ohledu na platformu.

SAS ověřování umožňuje aplikacím ke svému ověření u služby Service Bus pomocí přístupový klíč konfigurovány v oboru názvů, nebo u entity zasílání zpráv (fronty nebo téma) ke které jsou přidružené konkrétní práva. Pak můžete tento klíč k vygenerování tokenu SAS, který zase můžou klienti používat ke svému ověření u služby Service Bus.

Podpora ověřování SAS je součástí sady Azure SDK verze 2.0 nebo novější.

## <a name="overview-of-sas"></a>Přehled SAS

Sdílené přístupové podpisy jsou mechanismus ověřování na základě zabezpečeného hodnoty hash SHA-256 nebo identifikátory URI. Přidružení zabezpečení je velmi výkonný mechanismus, který je používán všechny služby Service Bus. Při skutečném použití SAS má dvě součásti: *sdílené zásady přístupu* a *sdílený přístupový podpis* (často říká *tokenu*).

Ověřování SAS v Service Bus zahrnuje konfiguraci kryptografického klíče s přidružená práva na prostředku služby Service Bus. Klienti deklarace přístup k prostředkům služby Service Bus prezentací SAS token. Tento token se skládá z identifikátoru URI přistupuje prostředku a vypršení platnosti podepsaný pomocí nakonfigurovaný klíč.

Podpis sdíleného přístupu autorizační pravidla můžete konfigurovat v Service Bus [předává](service-bus-fundamentals-hybrid-solutions.md#relays), [fronty](service-bus-fundamentals-hybrid-solutions.md#queues), a [témata](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS ověřování používá následující prvky:

* [Sdílený přístup autorizační pravidlo](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): volitelný sekundární klíč, název klíče a přidružená práva A 256 bitů primární kryptografický klíč ve formátu Base64 reprezentace (kolekce *naslouchání*, *odeslat*, nebo *spravovat* oprávnění).
* [Sdílený přístupový podpis](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) tokenu: vygenerování pomocí HMAC-SHA256 řetězce prostředků, který se skládá z identifikátoru URI prostředku, který je přístupný a vypršela platnost, pomocí kryptografického klíče. Podpis a další prvky popsané v následujících částech jsou formátovány na řetězec k vytvoření tokenu SAS.

## <a name="shared-access-policy"></a>Zásada sdíleného přístupu

Důležité pochopit o tokenu SAS je, začne se zásadami. Pro každou zásadu rozhodnete na tři údaje: **název**, **oboru**, a **oprávnění**. **Název** je právě toto; jedinečný název v rámci tohoto oboru. Rozsah je dostatečně snadno: je identifikátor URI prostředku nejistá. V případě oboru názvů Service Bus oboru je plně kvalifikovaný název domény (FQDN), jako například `https://<yournamespace>.servicebus.windows.net/`.

Jsou k dispozici oprávnění pro zásady z velké části není potřeba vysvětlovat:

* Odeslat
* Naslouchání
* Spravovat

Po vytvoření zásady, je přiřazen *primární klíč* a *sekundární klíč*. Toto jsou kryptograficky silnou klíče. Nemusíte je ztratí nebo je úniku – vždy budete mít k dispozici v [portál Azure][Azure portal]. Můžete použít buď generovaného klíče a můžete je obnovit kdykoli. Ale pokud chcete znovu vygenerovat nebo změnit primární klíč v zásadách, budou všechny sdílené přístupové podpisy na ní neplatné.

Když vytvoříte obor názvů sběrnice, zásady se automaticky vytvoří pro celý obor názvů názvem **RootManageSharedAccessKey**, a tato zásada nemá všechna oprávnění. Nemáte přihlásíte jako **kořenové**, takže nemusíte tuto zásadu použít, pokud dobře důvody. Můžete vytvořit další zásady v **konfigurace** kartě pro obor názvů na portálu. Je důležité si uvědomit, úrovně jediného stromu v Service Bus (obor názvů, fronty atd.) může mít pouze až 12 zásady, které jsou k němu připojen.

## <a name="configuration-for-shared-access-signature-authentication"></a>Konfigurace pro ověřování sdíleného přístupového podpisu
Můžete nakonfigurovat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) pravidlo na obory názvů Service Bus, fronty a témata. Konfigurace [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) na Service Bus se aktuálně nepodporuje předplatné, ale nakonfigurovaná v oboru názvů nebo téma pravidla můžete použít k zabezpečení přístupu k odběrům. Pracovní vzorku, který znázorňuje tento postup, najdete v článku [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) ukázka.

Nesmí být delší než 12 tato pravidla lze konfigurovat v oboru názvů Service Bus, fronta nebo téma. Pravidla, které jsou nakonfigurované na oboru názvů Service Bus se vztahují na všechny entity v daném oboru názvů.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

Na tomto obrázku *manageRuleNS*, *sendRuleNS*, a *listenRuleNS* autorizační pravidla, které se týkají fronta F1 a téma T1, při *listenRuleQ*  a *sendRuleQ* se vztahují pouze na frontu F1 a *sendRuleT* se vztahují pouze k tématu T1.

Klíče parametry [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) jsou následující:

| Parametr | Popis |
| --- | --- |
| *KeyName* |Řetězec, který popisuje sadu autorizačních pravidel. |
| *PrimaryKey* |Kódováním base64 256 bitů primární klíč pro přihlašování a ověřování tokenu SAS. |
| *Sekundární klíč* |Kódováním base64 256 bitů sekundární klíč pro přihlašování a ověřování tokenu SAS. |
| *AccessRights* |Seznam přístupová práva udělují autorizační pravidlo. Tato práva, může být jakékoli kolekce naslouchání, odeslání a Správa práv. |

Při zřízení oboru názvů Service Bus [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), s [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) nastavena na **RootManageSharedAccessKey**, se vytvoří ve výchozím nastavení.

## <a name="generate-a-shared-access-signature-token"></a>Vygenerování sdíleného přístupového podpisu (token)

Zásady samotné není přístupového tokenu pro Service Bus. Je objekt, ze které se generuje přístupový token - pomocí buď primární nebo sekundární klíč. Libovolného klienta, který má přístup k podpisový klíč zadaný v autorizační pravidlo sdíleného přístupu můžete vygenerovat SAS token. Vygenerování tokenu tím, že pečlivě vytvoří řetězec ve formátu:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Kde `signature-string` je hodnota hash SHA-256 oboru tokenu (**oboru** jak je popsáno v předchozí části) s Line FEED připojí a čas vypršení platnosti (v sekundách od epoch: `00:00:00 UTC` na 1. ledna pod hodnotou 1970). 

> [!NOTE]
> Abyste se vyhnuli čas krátké vypršení platnosti tokenu, se doporučuje, zakódovat hodnota času vypršení platnosti jako celé číslo bez znaménka alespoň 32-bit, nebo pokud možno celé číslo dlouho (64 bitů).  
> 
> 

Hodnota hash bude vypadat podobně jako následující kód pseudo a vrátí 32 bajtů.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Rozdělí hodnoty jsou v **SharedAccessSignature** řetězec tak, aby příjemce můžete vypočítat hodnotu hash se stejnými parametry, ujistěte se, že vrací stejného výsledku. Identifikátor URI Určuje obor, a název klíče identifikuje zásady, které mají být použity k výpočtu hodnoty hash. To je důležité z hlediska zabezpečení. Pokud podpis neodpovídá, vypočítá příjemce (Service Bus), byl odepřen přístup. Může být v tomto okamžiku se, že odesílatel měli přístup ke klíči a by měla být udělena práva určená v zásadách.

Všimněte si, že mají používat zakódovaný identifikátor URI pro tuto operaci. Identifikátor URI je úplný identifikátor URI prostředku Service Bus, ke kterému je požadován přístup. Například `http://<namespace>.servicebus.windows.net/<entityPath>` nebo `sb://<namespace>.servicebus.windows.net/<entityPath>`, tj `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

Autorizační pravidlo sdíleného přístupu, který se používá k podepisování musí být nakonfigurované na entitu zadanou tento identifikátor URI, nebo pomocí jedné z jejích hierarchické nadřazených tříd. Například `http://contoso.servicebus.windows.net/contosoTopics/T1` nebo `http://contoso.servicebus.windows.net` v předchozím příkladu.

SAS token je platná pro všechny prostředky pod `<resourceURI>` používány `signature-string`.

[KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) SAS token odkazuje **keyName** sloužící ke generování token pravidla autorizace sdíleného přístupu.

*URL kódovaný resourceURI* musí být stejný jako identifikátor URI použitých v řetězci přihlášení během výpočtu podpisu. Měla by být [kódováním procent](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Doporučujeme pravidelně obnovovat klíčů používaných v [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektu. Aplikace by měly používat obecně [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) pro vygenerování tokenu SAS. Při opakovaném generování klíčů, měli byste nahradit [sekundární klíč](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) s z původního primárního klíče a vygenerovat nový klíč jako nový primární klíč. Umožňuje pokračovat v používání tokeny k ověřování, které byly vydány s původní primární klíč a ještě, nevypršela jejich platnost.

Pokud dojde k narušení klíč a budete mít k odvolání klíče, můžete obě obnovit [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) a [sekundární klíč](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) z [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), ve které nahradíte je s novými klíči. Tento postup by způsobila neplatnost všechny tokeny, které jsou podepsány pomocí starého klíče.

## <a name="how-to-use-shared-access-signature-authentication-with-service-bus"></a>Použití sdíleného přístupového podpisu ověřování službou Service Bus

Následující scénáře zahrnují konfigurace autorizačních pravidel, generování tokenů SAS a autorizací klienta.

Pro úplnou pracovní vzorek aplikaci Service Bus, která ukazuje, konfigurace a používá autorizace SAS, najdete v části [sdíleného přístupového podpisu ověřování službou Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Související příklad, který znázorňuje použití SAS autorizační pravidla, která je nakonfigurovaná na obory názvů nebo témata zabezpečit odběry služby Service Bus je k dispozici zde: [pomocí sdíleného přístupového podpisu (SAS) ověřování pomocí předplatných Service Bus](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Pravidla přístupu k autorizaci sdíleného přístupu u oboru názvů

Operace v kořenovém oboru názvů Service Bus vyžadují ověřování pomocí certifikátu. Je potřeba načíst certifikát pro správu pro vaše předplatné Azure. Nahrání certifikátu pro správu, postupujte podle kroků [sem](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate)pomocí [portál Azure][Azure portal]. Další informace o certifikátech správy Azure najdete v tématu [přehled Azure certifikátů](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Koncový bod pro přístup k sdíleného přístupu autorizační pravidla na obor názvů sběrnice je následující:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

Chcete-li vytvořit [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt v oboru názvů Service Bus, provést operaci POST na tento koncový bod s informace o pravidle serializovanou jako XML nebo JSON. Například:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

Podobně použijte operaci GET na koncový bod čtení autorizační pravidla nakonfigurované na oboru názvů.

Pokud chcete aktualizovat nebo odstranit konkrétní autorizační pravidlo, použijte následující koncový bod:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Přístup sdíleného přístupu autorizační pravidla s entitou

Dostanete [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) nakonfigurované na fronty sběrnice nebo téma prostřednictvím objektu [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) kolekce v odpovídající [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) nebo [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

Následující kód ukazuje, jak přidat autorizační pravidla pro frontu.

```csharp
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Použití autorizačních sdíleného přístupového podpisu

Aplikace pomocí .NET SDK služby Azure na knihovny Service Bus .NET můžete použít autorizace SAS prostřednictvím [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) třídy. Následující kód ukazuje použití zprostředkovatel tokenu k odesílání zpráv do fronty Service Bus.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Aplikace můžete také použít SAS pro ověřování pomocí SAS připojovací řetězec v metodách, které přijímají připojovací řetězce.

Všimněte si, že použití autorizace SAS s předávací služby Service Bus, můžete použít klíče SAS, které jsou nakonfigurované na oboru názvů Service Bus. Pokud vytvoříte relé explicitně na obor názvů ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) s [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) objektu, budete moct nastavit pravidla SAS jen pro tento předávání. Pokud chcete používat autorizace SAS s odběry služby Service Bus, můžete použít klíče SAS, které jsou nakonfigurované na obor názvů sběrnice nebo téma.

## <a name="use-the-shared-access-signature-at-http-level"></a>Použití sdíleného přístupového podpisu (na úrovni protokolu HTTP)

Teď, když víte, jak vytvořit podpisy sdíleného přístupu pro všechny entity v Service Bus, jste připravení na provedení HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Pamatujte si, že se tento postup funguje pro vše. Můžete vytvořit SAS pro fronty, tématu nebo předplatného. 

Pokud poskytnete odesílatele nebo klienta SAS token, nemají klíč přímo a jejich nelze vrátit hodnotu hash k jeho získání. Jako takový budete mít kontrolu nad co přístupem a jak dlouho. Třeba mít na paměti je, pokud změníte primární klíč v zásadách, bude zrušena žádné sdílené přístupové podpisy vytvořit z něj.

## <a name="use-the-shared-access-signature-at-amqp-level"></a>Použití sdíleného přístupového podpisu (na úrovni AMQP)

V předchozí části znáte postup použití tokenu SAS s požadavek HTTP POST k odesílání dat do služby Service Bus. Jak víte, můžete přistupovat pomocí Advanced zpráv služby Řízení front protokol (AMQP), je upřednostňovanou protokol bude použit z důvodů výkonu v mnoha scénářích Service Bus. Využití tokenu SAS s AMQP je popsané v dokumentu [AMQP Claim-Based zabezpečení verze 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) který je v pracovní koncept od 2013 ale dobře nepodporuje Azure ještě dnes.

Než začnete používat k odesílání dat do služby Service Bus, vydavatele musí poslat tokenu SAS uvnitř zprávu AMQP dobře definovaný AMQP uzel s názvem **$cbs** (zobrazí se jako "speciální" fronty služba používá k získání a ověření všech SAS tokeny). Musíte zadat vydavatele **ReplyTo** pole uvnitř zpráva AMQP; Toto je uzel, ve kterém služba reaguje na vydavatele s výsledek ověření tokenu (jednoduché požadavek nebo odpověď vzor mezi vydavatele a služby ). Tento uzel odpověď se vytvoří "na za chodu," o "dynamické vytvoření vzdáleném uzlu" a Mluvte podle specifikace protokolu AMQP 1.0. Po zkontrolování, že je platný SAS token, můžete vydavatele vpřed a začít odesílat data do služby.

Následující kroky ukazují, jak odeslat token SAS s použitím protokolu AMQP [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) knihovny. To je užitečné, pokud nemůžete použít oficiální Service Bus SDK (například na WinRT, rozhraní .net Compact Framework, rozhraní .net Micro Framework a Mono) vývoj v jazyce C\#. Tato knihovna je samozřejmě užitečné při pochopit, jak založené na deklaracích identity zabezpečení funguje na úrovni protokolu AMQP, jako jste viděli, jak to funguje na úrovni protokolu HTTP (s požadavek HTTP POST a SAS token odeslaný v hlavičce "Autorizace"). Pokud nepotřebujete takové hluboké znalosti o AMQP, můžete použít oficiální Service Bus SDK s rozhraním .net Framework aplikace, které bude to pro vás.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Metoda přijímá *připojení* (instance třídy připojení AMQP jako poskytované [AMQP .NET Lite knihovny](https://github.com/Azure/amqpnetlite)), která představuje připojení TCP a *sasToken* parametr, který je SAS token k odeslání. 

> [!NOTE]
> Je důležité, že připojení je vytvořen s **SASL ověřovací mechanismus nastavit na externí** (a ne prostý výchozí uživatelské jméno a heslo použít, když je nebudete muset odeslat SAS token).
> 
> 

V dalším kroku vydavatele vytvoří dva odkazy AMQP pro tokenu SAS odesílání a příjmu odpovědi (výsledek ověření tokenu) ze služby.

AMQP zpráva obsahuje sadu vlastností a další informace než jednoduché zprávy. SAS token je do těla zprávy (pomocí jeho konstruktoru). **"ReplyTo"** je nastavena na název uzlu pro příjem výsledek ověření na tento odkaz příjemce (pokud, můžete změnit její název chcete a bude nutné vytvořit dynamicky službou). Poslední tři aplikace nebo vlastní vlastnosti používá službu k označení, jaký druh operace má spustit. Jak je popsáno ve specifikaci CBS koncept, musí být **název operace** ("put-token"), **typ tokenu** (v tomto případě "servicebus.windows.net:sastoken") a **"název" Cílová skupina** na kterou se vztahuje token (celý entity).

Po odeslání tokenu SAS na odkaz odesílatele, musí vydavatele čtení odpovědi na tento odkaz příjemce. Odpověď je jednoduchý AMQP zprávou s vlastností aplikace s názvem **"kód stavu"** obsahující stejné hodnoty jako stavový kód HTTP.

## <a name="rights-required-for-service-bus-operations"></a>Práva potřebná pro operace služby Service Bus

V následující tabulce jsou uvedeny přístupová práva potřebná pro různé operace na prostředcích služby Service Bus.

| Operace | Požadované deklarace identity | Deklarace oboru |
| --- | --- | --- |
| **Namespace** | | |
| Konfigurovat autorizační pravidlo na obor názvů |Spravovat |Každou adresu, obor názvů |
| **Služba registru** | | |
| Zobrazení výčtu privátní zásady |Spravovat |Každou adresu, obor názvů |
| Zahájit naslouchání na obor názvů |Naslouchání |Každou adresu, obor názvů |
| Odesílání zpráv pro naslouchací proces v oboru názvů |Odeslat |Každou adresu, obor názvů |
| **Fronty** | | |
| Vytvoření fronty |Spravovat |Každou adresu, obor názvů |
| Odstranění fronty |Spravovat |Každou adresu, platný fronty |
| Zobrazení výčtu fronty |Spravovat |$Prostředky nebo fronty |
| Získat popis fronty |Spravovat |Každou adresu, platný fronty |
| Konfigurovat autorizační pravidlo pro frontu |Spravovat |Každou adresu, platný fronty |
| Odeslat do fronty |Odeslat |Každou adresu, platný fronty |
| Příjem zpráv z fronty |Naslouchání |Každou adresu, platný fronty |
| Chyby nebo celé zprávy po přijetí zprávy v režimu zamknutí funkce Náhled |Naslouchání |Každou adresu, platný fronty |
| Odložení zprávu pro pozdější načtení |Naslouchání |Každou adresu, platný fronty |
| Tato zpráva |Naslouchání |Každou adresu, platný fronty |
| Zjištění stavu související s relací fronty zpráv |Naslouchání |Každou adresu, platný fronty |
| Nastavit stav související s relací fronty zpráv |Naslouchání |Každou adresu, platný fronty |
| **Téma** | | |
| Vytvoření tématu |Spravovat |Každou adresu, obor názvů |
| Odstranit téma |Spravovat |Každou adresu, platný tématu |
| Zobrazení výčtu témata |Spravovat |$Prostředky nebo témata |
| Získání popisu tématu |Spravovat |Každou adresu, platný tématu |
| Konfigurovat autorizační pravidlo pro téma |Spravovat |Každou adresu, platný tématu |
| Odeslat do tématu |Odeslat |Každou adresu, platný tématu |
| **Předplatné** | | |
| Vytvoření odběru |Spravovat |Každou adresu, obor názvů |
| Odstranit odběr |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Zobrazení výčtu odběrů |Spravovat |.. / myTopic/odběrů |
| Získat předplatné popis |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Chyby nebo celé zprávy po přijetí zprávy v režimu zamknutí funkce Náhled |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Odložení zprávu pro pozdější načtení |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Tato zpráva |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Zjištění stavu související s relací tématu |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| Nastavit stav související s relací tématu |Naslouchání |.. /myTopic/Subscriptions/mySubscription |
| **Pravidla** | | |
| Vytvoření pravidla |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Odstranění pravidla |Spravovat |.. /myTopic/Subscriptions/mySubscription |
| Zobrazení výčtu pravidel |Spravovat nebo naslouchání |.. /myTopic/Subscriptions/mySubscription/Rules 

## <a name="next-steps"></a>Další kroky

Pokud se o přenosu zpráv přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Fronty, témata a odběry služby Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak používat fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak používat témata a odběry Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com
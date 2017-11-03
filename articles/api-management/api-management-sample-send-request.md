---
title: "Použití služby API Management pro generování požadavků HTTP"
description: "Naučit se používat zásady žádostí a odpovědí ve službě API Management volat externí služby z rozhraní API"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 6b7f1268ea4893307713931e7288f5d38c5ee080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Pomocí externích služeb ze služby Azure API Management
Zásady, které jsou dostupné ve službě Azure API Management service může provádět široké škály užitečné pracovní výhradně na základě příchozího požadavku, odchozí odpovědi a informace o základní konfiguraci. Ale schopnost komunikovat s externích služeb z rozhraní API správy zásad mnoho většího počtu možností otevře.

Jsme předtím viděli, jak jsme mohou komunikovat s [centra událostí Azure služby pro protokolování, sledování a analýza](api-management-log-to-eventhub-sample.md). V tomto článku jsme ukazují, že zásady, které vám umožní komunikovat s žádné externí HTTP na základě služby. Tyto zásady lze pro spuštění vzdáleného události nebo pro načtení informací, který se použije k manipulaci s původní žádosti a odpovědi nějakým způsobem.

## <a name="send-one-way-request"></a>Odeslání jeden způsob požadavků
Může být nejjednodušší externí interakce je ještě efektivněji a zapomněli styl požadavek, který umožňuje externí služba mají být informování určitého druhu důležité události. Můžeme použít zásady řízení toku `choose` ke zjištění jakékoliv podmínky, že jsme mají zájem o a poté, pokud je splněna podmínka, abychom mohli provádět k externí HTTP žádosti pomocí [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) zásad. To může být žádost o zasílání zpráv systému jako Hipchat nebo Slack nebo pošty rozhraní API jako sendgrid vám umožňuje nebo MailChimp, nebo pro kritické podpory incidentů něco jako PagerDuty. Všechny tyto systémy zasílání zpráv mají jednoduché rozhraní API HTTP, který lze snadno vyvolat.

### <a name="alerting-with-slack"></a>Výstrahy s Slack
Následující příklad ukazuje, jak odeslat zprávu do Slack chatovací místnosti, pokud stavový kód odpovědi HTTP je větší než nebo rovna hodnotě 500. Rozsah 500 Chyba znamená problém s naše back-end rozhraní API, klient našem rozhraní API nelze vyřešit sami. Obvykle vyžaduje nějaký druh zásahu na našem část.  

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>
```

Slack má představu o háky příchozí webové. Při konfiguraci příchozí webové háku, generuje Slack speciální adresy URL, která umožňuje provést jednoduchý požadavek POST a předávat zprávy do kanálu Slack. Text JSON, který vytváříme je založena na formátu definované Slack.

![Slack webové háku](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Je ještě efektivněji a zapomněli dostatečně vhodné?
Existují určité kompromisy při použití styl ještě efektivněji a zapomněli požadavku. Pokud z nějakého důvodu, žádost skončí s chybou a potom selhání, nebudou ohlášena. V takovém případě konkrétní není oprávněná složitost toho, že sekundární selhání sestav systému a další dopady čekání na odpověď. Pro scénáře, kdy je nezbytně nutné, aby Zkontrolujte odpověď, a pak se [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zásad je lepší volbou.

## <a name="send-request"></a>Odeslání žádosti
`send-request` Zásad umožňuje pomocí externí služby k provedení funkce komplexní zpracování a vrátit data správy rozhraní API služby, který lze použít pro další zpracování zásad.

### <a name="authorizing-reference-tokens"></a>Autorizace tokeny odkazů
Hlavní funkce API Management chrání back-endové prostředky. Pokud server ověřování používá vaše rozhraní API vytvoří [tokeny JWT](http://jwt.io/) jako součást jeho tok OAuth2, jako [Azure Active Directory](../active-directory/active-directory-aadconnect.md) nemá, můžete použít `validate-jwt` zásad k ověření platnosti tokenu. Ale některé servery autorizace vytvořit, co se nazývají [odkazovat tokeny](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , nelze ověřit bez provedení volání zpět na server ověřování.

### <a name="standardized-introspection"></a>Standardizovaná introspection
V minulosti byl nijak standardizované ověřit token odkazu se serverem ověřování. Ale nedávno navrhovaný standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publikoval IETF, která definuje, jak server prostředků můžete ověřit platnost tokenu.

### <a name="extracting-the-token"></a>Extrahování tokenu
Prvním krokem je extrahuje token z autorizační hlavičky. Hodnota hlavičky by měl být naformátován pomocí `Bearer` autorizace schéma a jedna mezera a autorizaci token dle [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Bohužel existují případech, kdy je vynechán schéma autorizace. Chcete-li pro tento účet při analýze, jsme rozdělit hodnota hlavičky v prostoru a vyberte možnost poslední řetězec vrácený pole řetězců. To poskytuje řešení chybně formátovaný autorizační hlavičky.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Vytvoření ověření požadavku
Jakmile jsme autorizačním tokenem, abychom mohli provádět ověření tokenu. RFC 7662 volá tento proces introspection a vyžaduje, aby vám `POST` formuláře HTML k introspection prostředku. Formuláře HTML musí obsahovat alespoň dvojici klíč/hodnota s klíčem `token`. Tento požadavek server ověřování musí být ověřeny také zajistit, že škodlivý klientů nelze přejít vlečným pro platné tokeny.

```xml
<send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
  <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
  <set-method>POST</set-method>
  <set-header name="Authorization" exists-action="override">
    <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
  </set-header>
  <set-header name="Content-Type" exists-action="override">
    <value>application/x-www-form-urlencoded</value>
  </set-header>
  <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
</send-request>
```

### <a name="checking-the-response"></a>Kontrola odpovědi
`response-variable-name` Atribut se používá k poskytnout přístup vrácený odpovědi. Název definovaný v této vlastnosti lze použít jako klíč do `context.Variables` slovník pro přístup `IResponse` objektu.

Z objektu odpovědi můžeme načíst tělo a RFC 7622 víme, že odpověď musí být objekt JSON a musí obsahovat aspoň vlastnost s názvem `active` tedy logickou hodnotu. Když `active` má hodnotu true, pak token je považován za platný.

### <a name="reporting-failure"></a>Generování sestav selhání
Používáme `<choose>` zásady a zjistit, jestli je neplatný token a pokud ano, vrátí odpovědi 401.

```xml
<choose>
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
    <return-response response-variable-name="existing response variable">
      <set-status code="401" reason="Unauthorized" />
      <set-header name="WWW-Authenticate" exists-action="override">
        <value>Bearer error="invalid_token"</value>
      </set-header>
    </return-response>
  </when>
</choose>
```

Dle [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) který popisuje, jak `bearer` slouží tokeny, také vrátíme `WWW-Authenticate` hlavička s odpovědi 401. WWW-Authenticate je určený dáte pokyn, aby klient o tom, jak vytvořit žádost o správně autorizované. Z důvodu širokou škálu přístupy možné pomocí rozhraní OAuth2 je obtížné komunikaci všechny potřebné informace. Naštěstí existují úsilí zpracováváme na pomoc [klienti zjišťovat správně autorizace požadavky na server prostředků](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Konečné řešení
Spojení všech částí společně, se nám získat tyto zásady:

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
    <set-method>POST</set-method>
    <set-header name="Authorization" exists-action="override">
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
    </set-header>
    <set-header name="Content-Type" exists-action="override">
      <value>application/x-www-form-urlencoded</value>
    </set-header>
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
  </send-request>

  <choose>
          <!-- Check active property in response -->
          <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
              <!-- Return 401 Unauthorized with http-problem payload -->
              <return-response response-variable-name="existing response variable">
                  <set-status code="401" reason="Unauthorized" />
                  <set-header name="WWW-Authenticate" exists-action="override">
                      <value>Bearer error="invalid_token"</value>
                  </set-header>
              </return-response>
          </when>
      </choose>
  <base />
</inbound>
```

Toto je pouze jeden z mnoha příklady jak `send-request` zásadu lze použít k integraci užitečné externích služeb do procesu požadavků a odpovědí prostřednictvím služby API Management.

## <a name="response-composition"></a>Složení odpovědi
`send-request` Zásadu lze použít pro zlepšení primární požadavek na back-end systému, jako jsme viděli v předchozím příkladu, nebo můžete využít jako kompletní nahrazení pro volání back-end. Touto technikou jsme můžete snadno vytvořit složené prostředky, které jsou seskupeny z několika různými systémy.

### <a name="building-a-dashboard"></a>Vytváření řídicího panelu
Někdy budete chtít mít ke zveřejnění informací, že existuje více back-end systémy, třeba, aby jednotka řídicí panel. Klíčové ukazatele výkonu pocházet z všechny různé back EndY, ale chcete raději nechcete poskytuje přímý přístup k nim a bude dobrý, všechny informace, které se nepovedlo načíst v jedné žádosti. Možná některé z informací back-end vyžaduje některé segmentování a analyzování a úpravě trochu nejprve! Schopnost mezipaměti složené prostředku by být užitečné ke snížení zatížení back-end, jako je zřejmé, že uživatelé mají která podporují ražením klávesu F5, aby bylo možné zjistit, zda se může změnit jejich underperforming metriky.    

### <a name="faking-the-resource"></a>Faking prostředku
Prvním krokem k vytváření prostředků naše řídicí panel je konfigurace nového operace v portál vydavatele služby API Management. Bude jím operaci na zástupný symbol slouží ke konfiguraci zásad naše složení k sestavení naše dynamické prostředků.

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Provedení žádosti
Jednou `dashboard` operace byla vytvořena jsme můžete nakonfigurovat zásadu speciálně pro tuto operaci. 

![Operace řídicí panel](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Prvním krokem je extrahovat žádné parametry dotazu z příchozího požadavku, takže jsme může předávat na našem back-end. V tomto příkladu se naše řídicí panel zobrazuje informace založené na určitou dobu tedy `fromDate` a `toDate` parametr. Můžeme použít `set-variable` zásad k extrahování informací z adresy URL žádosti.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Jakmile tyto informace bychom mohli požadavky na back-end systémy. Každý požadavek vytvoří novou adresu URL s informace o parametru a volá jeho příslušný server a ukládá odpovědi v kontextové proměnné.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Tyto požadavky bude spuštěn v pořadí, což není ideální. V budoucích vydání jsme zavádí nové zásady názvem `wait` , povolí všechny tyto požadavky na spouštění paralelně.

### <a name="responding"></a>Neodpovídá
K vytvoření složeného odpovědi můžeme použít [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásad. `set-body` Element výraz můžete použít k vytvoření nové `JObject` s všechny součásti reprezentace vložených jako vlastnosti.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Dokončení zásad vypadá takto:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

V konfiguraci zástupného textu operaci nakonfigurujeme řídicí panel prostředků do mezipaměti alespoň jednu hodinu, protože jsme pochopení povahy dat znamená, že i když je zastaralý, že budou mít pořád dostatečně efektivní ke sdělení cenné informace pro uživatele hodinu.

## <a name="summary"></a>Souhrn
Služba Azure API Management poskytuje flexibilní zásady, které je možné selektivně použít pro provoz protokolu HTTP a umožňuje složení služeb back-end. Jestli chcete vylepšit bránu rozhraní API s výstrahy funkce, ověření, možnosti ověření nebo vytvořit nové složené prostředky v závislosti na více služeb back-end `send-request` a související zásady otevřete široké možnosti.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video s přehledem těchto zásad
Další informace o [odesílání jeden způsob požadavků](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [odeslán požadavek](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), a [vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásady popsaná v tomto článku prosím v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 


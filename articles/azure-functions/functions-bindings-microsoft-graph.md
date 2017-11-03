---
title: "Azure vazby funkcí Microsoft Graph | Microsoft Docs"
description: "Pochopit, jak používat Microsoft Graph triggerů a vazeb v Azure Functions."
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/19/2017
ms.author: mahender
ms.openlocfilehash: 8cf2e4e9e9007549dbdc931b4485c4230c536479
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-microsoft-graph-bindings"></a>Azure vazby funkcí Microsoft Graph
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a práce s Microsoft Graph triggerů a vazeb v Azure Functions.
Pomocí těchto, můžete použít funkce Azure pro práci s daty, přehledy a události z [Microsoft Graph](https://graph.microsoft.io).

Rozšíření Microsoft Graph poskytuje následující vazby:
- [Token ověření vstupu vazby](#token-input) umožňuje pracovat s žádné Microsoft Graph API.
- [Tabulky aplikace Excel vstupní vazby](#excel-input) umožňuje číst data z Excelu.
- [Tabulky aplikace Excel výstup vazby](#excel-output) umožňuje upravovat data z Excelu.
- [OneDrive souboru vstupní vazby](#onedrive-input) umožňuje číst soubory z Onedrivu.
- [OneDrive souboru výstup vazby](#onedrive-output) umožňuje zápis do souborů na Onedrivu.
- [Zprávy Outlooku výstup vazby](#outlook-output) umožní vám odesílat e-mailu prostřednictvím aplikace Outlook.
- Kolekce [Microsoft Graph webhooku triggerů a vazeb](#webhooks) umožňuje reagování na události z Microsoft Graph.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note] 
> Microsoft Graph vazby jsou aktuálně ve verzi preview.

## <a name="setting-up-the-extensions"></a>Nastavení rozšíření

Microsoft Graph vazby jsou k dispozici prostřednictvím _vazby rozšíření_. Vazba rozšíření jsou volitelné součásti modulu runtime Azure Functions. V této části vám ukáže, jak nastavit Microsoft Graph a ověření tokenu rozšíření.

### <a name="enabling-functions-20-preview"></a>Povolení funkce 2.0 preview

Vazba rozšíření jsou pouze k dispozici pouze pro 2.0 funkce Azure preview. 

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Další informace najdete v tématu [jak mít verze modulu runtime Azure Functions](functions-versions.md).

### <a name="installing-the-extension"></a>Instalaci rozšíření

Chcete-li nainstalovat rozšíření na portálu Azure, přejděte na šablonu nebo vazby, která odkazuje na. Vytvoření nové funkce a v obrazovku pro výběr šablony, zvolte scénář "Microsoft Graph". Vyberte jednu z šablony tento scénář. Alternativně můžete přejděte na kartu "Integrací" existující funkce a vyberte jednu z vazby zahrnuté v tomto tématu.

V obou případech se zobrazí upozornění, která určuje příponu k instalaci. Klikněte na tlačítko **nainstalovat** získat rozšíření.

> [!Note] 
> Každé rozšíření stačí nainstalovat jednou pro každé aplikaci funkce. Proces instalace v portálu může trvat až 10 minut na plánu spotřeby.

Pokud používáte Visual Studio, můžete získat rozšíření nainstalováním těchto balíčků NuGet:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-app-service-authentication--authorization"></a>Konfigurace služby App Service ověřování / autorizace

Vazby uvedené v tomto tématu vyžadují identity má být použit. To umožňuje Microsoft Graph vynutit oprávnění a auditovat interakce. Identita může být uživatel přístup k aplikaci nebo vlastní aplikace. Konfigurace tuto identitu, budete muset nastavit [aplikace služby ověřování / autorizace](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) službou Azure Active Directory. Musíte se také s žádostí o oprávnění všech prostředků, které vyžadují funkcí.

> [!Note] 
> Rozšíření Microsoft Graph podporuje jenom ověřování AAD. Uživatelé se muset přihlásit pomocí pracovního nebo školního účtu.

Pokud pomocí portálu Azure pod výzva k potvrzení instalace rozšíření, zobrazí se upozornění, kde je konfigurace aplikace služby ověřování / autorizace a žádost o všechna oprávnění, šablony nebo vazba vyžaduje. Klikněte na tlačítko **konfigurace AAD teď** nebo **teď přidejte oprávnění** podle potřeby.






<a name="token-input"></a>
## <a name="auth-token-input-binding"></a>Ověření tokenu vstupní vazby

Tato vazba získá token AAD pro daný prostředek a poskytuje ji jako řetězec v kódu. Prostředek může být libovolná, pro který má oprávnění aplikace. 

### <a name="configuring-an-auth-token-input-binding"></a>Konfigurace ověřování tokenu vstupní vazbu

Vazba, samotné nevyžaduje žádné oprávnění AAD, ale v závislosti na tom, jak se používá token, budete muset požádat o další oprávnění. Zkontrolujte požadavky na prostředku, který chcete s tímto tokenem přístupu.

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro ověření tokenu. V tématu [pomocí tokenu ověření vstupu vazba z kódu](#token-input-code).|
|**Typ**|Vyžaduje - musí být nastavena na `token`.|
|**směr**|Vyžaduje - musí být nastavena na `in`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**prostředek**|Požadovaná adresu URL prostředku AAD, pro které je požadováno token.|

<a name="token-input-code"></a>
### <a name="using-an-auth-token-input-binding-from-code"></a>Pomocí ověřování tokenu vstupní vazbu z kódu

Token se vždy zobrazí kód jako řetězec.

#### <a name="sample-getting-user-profile-information"></a>Ukázka: Získávání informací o profilu uživatele

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP tokenu vstupní vazby:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# na základě tokenu pro volání protokolu HTTP pro Microsoft Graph a vrátí výsledek:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

Následující ukázka JS volání protokolu HTTP pro Microsoft Graph používá token a vrátí výsledek. V `function.json` vyšší, změnit `$return` k `res` první.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```





<a name="excel-input"></a>
## <a name="excel-table-input-binding"></a>Vazba vstupní tabulky aplikace Excel

Tato vazba přečte obsah tabulky aplikace Excel uložených na Onedrivu.

### <a name="configuring-an-excel-table-input-binding"></a>Konfigurace vazbu vstupní tabulky aplikace Excel

Tato vazba vyžaduje následující AAD oprávnění:
|Prostředek|Oprávnění|
|--------|--------|
|Microsoft Graph|Čtení souborů uživatele|

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro tabulky aplikace Excel. V tématu [pomocí tabulky aplikace Excel Vstupní vazba z kódu](#excel-input-code).|
|**Typ**|Vyžaduje - musí být nastavena na `excel`.|
|**směr**|Vyžaduje - musí být nastavena na `in`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**Cesta**|Požadovaná cesta k sešitu aplikace Excel na OneDrive.|
|**worksheetName**|Na listu, ve které se nacházejí v tabulce.|
|**Název tabulky**|Název tabulky. Pokud není zadaný, použije se obsah listu.|

<a name="excel-input-code"></a>
### <a name="using-an-excel-table-input-binding-from-code"></a>Pomocí tabulky aplikace Excel vstupní vazbu z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- řetězec [] –]
- Microsoft.Graph.WorkbookTable
- Typy vlastních objektů (pomocí vazby modelu strukturální)

#### <a name="sample-reading-an-excel-table"></a>Ukázka: Čtení tabulky aplikace Excel

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP s vazbou vstupní aplikace Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# přidá přečte obsah zadané tabulky a vrátí je pro uživatele:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

Následující ukázka JS přidá přečte obsah zadané tabulky a vrátí je pro uživatele. V `function.json` vyšší, změnit `$return` k `res` první.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

<a name="excel-output"></a>
## <a name="excel-table-output-binding"></a>Vazba výstupní tabulky aplikace Excel

Tato vazba upravuje obsah tabulky aplikace Excel uložených na Onedrivu.

### <a name="configuring-an-excel-table-output-binding"></a>Konfigurace tabulky aplikace Excel výstup vazby

Tato vazba vyžaduje následující AAD oprávnění:
|Prostředek|Oprávnění|
|--------|--------|
|Microsoft Graph|Úplný přístup k souborům uživatele|

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro ověření tokenu. V tématu [pomocí tabulky aplikace Excel výstup vazba z kódu](#excel-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `excel`.|
|**směr**|Vyžaduje - musí být nastavena na `out`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**Cesta**|Požadovaná cesta k sešitu aplikace Excel na OneDrive.|
|**worksheetName**|Na listu, ve které se nacházejí v tabulce.|
|**Název tabulky**|Název tabulky. Pokud není zadaný, použije se obsah listu.|
|**Typ aktualizace**|Požadovaná typ změny, aby se do tabulky. Může být jedna z následujících hodnot:<ul><li><code>update</code>-Nahradí obsah tabulky ve Onedrivu.</li><li><code>append</code>-Přidá datové části na konec tabulky OneDrive vytvořením nové řádky.</li></ul>|

<a name="excel-output-code"></a>
### <a name="using-an-excel-table-output-binding-from-code"></a>Pomocí tabulky aplikace Excel výstup vazba z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- řetězec [] –]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Typy vlastních objektů (pomocí vazby modelu strukturální)

#### <a name="sample-adding-rows-to-an-excel-table"></a>Ukázka: Přidávání řádků do tabulky aplikace Excel

Předpokládejme, že máte následující výstup vazby function.json, která definuje aktivační procedury HTTP pomocí aplikace Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```


Následující ukázka C# přidá nový řádek v tabulce (předpokládá, že jeden sloupec) na základě žádostí ze řetězec dotazu:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

Následující ukázka JS přidá nový řádek do tabulky (předpokládá, že jeden sloupec) založených na vstupu z řetězce dotazu. V `function.json` vyšší, změnit `$return` k `res` první.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```




<a name="onedrive-input"></a>
## <a name="onedrive-file-input-binding"></a>Vazba vstupní soubor OneDrive

Tato vazba přečte obsah souboru uložených na Onedrivu.

### <a name="configuring-a-onedrive-file-input-binding"></a>Konfigurace vazeb vstupní soubor OneDrive

Tato vazba vyžaduje následující AAD oprávnění:
|Prostředek|Oprávnění|
|--------|--------|
|Microsoft Graph|Čtení souborů uživatele|

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro soubor. V tématu [pomocí souboru OneDrive Vstupní vazba z kódu](#onedrive-input-code).|
|**Typ**|Vyžaduje - musí být nastavena na `onedrive`.|
|**směr**|Vyžaduje - musí být nastavena na `in`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**Cesta**|Požadovaná cesta k souboru na OneDrive.|

<a name="onedrive-input-code"></a>
### <a name="using-a-onedrive-file-input-binding-from-code"></a>Použití souboru OneDrive vstupní vazby z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- Byte]
- Datový proud
- Řetězec
- Microsoft.Graph.DriveItem

#### <a name="sample-reading-a-file-from-onedrive"></a>Ukázka: Načítání souboru z Onedrivu

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP vazbu vstupní OneDrive:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# načte soubor zadaný v řetězci dotazu a jeho délka protokoly:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

Následující ukázka JS načte soubor zadaný v řetězci dotazu a vrátí jeho délka. V `function.json` vyšší, změnit `$return` k `res` první.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```


<a name="onedrive-output"></a>
## <a name="onedrive-file-output-binding"></a>Výstupní soubor OneDrive vazby

Tato vazba změní obsah souboru uložených na Onedrivu.

### <a name="configuring-a-onedrive-file-output-binding"></a>Konfigurace OneDrive výstupní soubor vazby

Tato vazba vyžaduje následující AAD oprávnění:
|Prostředek|Oprávnění|
|--------|--------|
|Microsoft Graph|Úplný přístup k souborům uživatele|

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro soubor. V tématu [pomocí souboru OneDrive výstup vazba z kódu](#onedrive-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `onedrive`.|
|**směr**|Vyžaduje - musí být nastavena na `out`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**Cesta**|Požadovaná cesta k souboru na OneDrive.|

<a name="onedrive-output-code"></a>
### <a name="using-a-onedrive-file-output-binding-from-code"></a>Použití OneDrive výstupní soubor vazba z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- Byte]
- Datový proud
- Řetězec
- Microsoft.Graph.DriveItem

#### <a name="sample-writing-to-a-file-in-onedrive"></a>Ukázka: Zápis do souboru na Onedrivu

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP s OneDrive výstup vazby:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# načte text z řetězce dotazu a zapíše ho do textového souboru (FunctionsTest.txt jak je definována v konfiguračním výše) v kořenovém adresáři volajícího onedrivu:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```
Následující ukázka JS načte text z řetězce dotazu a zapíše ho do textového souboru (FunctionsTest.txt definovaným v souboru config výše) v kořenovém adresáři volajícího onedrivu. V `function.json` vyšší, změnit `$return` k `res` první.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```



<a name="outlook-output"></a>
## <a name="outlook-message-output-binding"></a>Zprávy Outlooku výstup vazby

Odešle e-mailovou zprávu prostřednictvím aplikace Outlook.

### <a name="configuring-an-outlook-message-output-binding"></a>Konfigurace zprávy Outlooku výstup vazby

Tato vazba vyžaduje následující AAD oprávnění:
|Prostředek|Oprávnění|
|--------|--------|
|Microsoft Graph|Odesílat poštu jménem uživatele|

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro e-mailovou zprávu. V tématu [pomocí zprávy Outlooku výstup vazba z kódu](#outlook-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `outlook`.|
|**směr**|Vyžaduje - musí být nastavena na `out`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |

<a name="outlook-output-code"></a>
### <a name="using-an-outlook-message-output-binding-from-code"></a>Pomocí zprávy Outlooku výstup vazba z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- Řetězec
- Typy vlastních objektů (pomocí vazby modelu strukturální)

#### <a name="sample-sending-an-email-through-outlook"></a>Ukázka: Odesílání e-mailu prostřednictvím aplikace Outlook

Předpokládejme, že máte následující výstup vazby function.json, která definuje aktivační procedury HTTP s zprávy aplikace Outlook:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# odešle poštu volající příjemce určeného v řetězci dotazu:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

Následující ukázka JS odešle poštu volající příjemce určeného v řetězci dotazu:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```





<a name="webhooks"></a>
## <a name="microsoft-graph-webhook-bindings"></a>Microsoft Graph webhooku vazby

Webhooky umožňují reagování na události v aplikaci Microsoft Graph. Pro podporu webhooků, jsou funkce potřebné k vytvoření, aktualizace a reagovat na _webhooku odběry_. Dokončení webhooku řešení vyžaduje kombinaci následující vazby:
- A [aktivační události webhooku Microsoft Graph](#webhook-trigger) umožňuje reagovat na příchozí webhooku.
- A [webhooku předplatného Microsoft Graph vstupní vazby](#webhook-input) umožňuje seznamu existující odběry a můžete je aktualizovat.
- A [webhooku předplatného Microsoft Graph výstup vazby](#webhook-output) umožňuje vytvořit nebo odstranit odběry webhooku.

Vazby sami nevyžadují žádné oprávnění AAD, ale budete muset požádat o oprávnění, které jsou relevantní pro typ prostředku, který chcete reagovat na. Seznam, které jsou potřeba oprávnění pro každý typ prostředku, naleznete v části [předplatné oprávnění](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Další informace o webhooky najdete v tématu [práce s webhooky v aplikaci Microsoft Graph].





<a name="webhook-trigger"></a>
### <a name="microsoft-graph-webhook-trigger"></a>Aktivační události webhooku Microsoft Graph

Této aktivační události umožňuje funkce, která se reagovat na příchozí webhooku z Microsoft Graph. Každá instance této aktivační události může reagovat na některý typ prostředku Microsoft Graph.

#### <a name="configuring-a-microsoft-graph-webhook-trigger"></a>Konfigurace aktivační události webhooku Microsoft Graph

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro e-mailovou zprávu. V tématu [pomocí zprávy Outlooku výstup vazba z kódu](#outlook-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `graphWebhook`.|
|**směr**|Vyžaduje - musí být nastavena na `trigger`.|
|**Typ prostředku**|Požadovaná prostředků grafu, pro kterou má tato funkce Odpovědět na webhooky. Může být jedna z následujících hodnot:<ul><li><code>#Microsoft.Graph.Message</code>-změny provedené v aplikaci Outlook zprávy.</li><li><code>#Microsoft.Graph.DriveItem</code>-změny provedené v OneDrive kořenové položky.</li><li><code>#Microsoft.Graph.Contact - changes made to personal contacts in Outlook.</code></li><li><code>#Microsoft.Graph.Event - changes made to Outlook calendar items.</code></li></ul>|

> [!Note]
> Funkce aplikace může mít pouze jednu funkci, která je registrována proti danou `resourceType` hodnotu.

#### <a name="using-a-microsoft-graph-webhook-trigger-from-code"></a>Pomocí aktivační události webhooku Microsoft Graph z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- Microsoft Graph SDK typy, které jsou relevantní pro prostředek typu, například Microsoft.Graph.Message, Microsoft.Graph.DriveItem
- Typy vlastních objektů (pomocí vazby modelu strukturální)

Příklady použití této vazby v kódu, najdete v tématu [Microsoft Graph webhooku ukázky](#webhook-samples).



<a name="webhook-input"></a>
### <a name="microsoft-graph-webhook-subscription-input-binding"></a>Microsoft Graph webhooku předplatné vstupní vazby

Tato vazba umožňuje načíst seznam předplatných spravuje této funkce aplikace. Vazba čte z úložiště funkce aplikace a nemusí odpovídat odběr vytvořené z mimo aplikaci.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-input-binding"></a>Konfigurace vazeb vstupní předplatné webhooku Microsoft Graph

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro e-mailovou zprávu. V tématu [pomocí zprávy Outlooku výstup vazba z kódu](#outlook-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `graphWebhookSubscription`.|
|**směr**|Vyžaduje - musí být nastavena na `in`.|
|**Filtr**| Pokud nastavena na `userFromRequest`, pak vazby pouze načte odběry vlastněných uživatelem volání (platný pouze s [triggeru protokolu HTTP]).| 

#### <a name="using-a-microsoft-graph-webhook-subscription-input-binding-from-code"></a>Použití Microsoft Graph webhooku předplatné vstupní vazby z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- řetězec]
- Typ pole vlastních objektů
- [Newtonsoft.Json.Linq.JObject]
- [Microsoft.Graph.Subscription]

Příklady použití této vazby v kódu, najdete v tématu [Microsoft Graph webhooku ukázky](#webhook-samples).


<a name="webhook-output"></a>
### <a name="microsoft-graph-webhook-subscription-output-binding"></a>Předplatné Microsoft Graph webhooku výstup vazby

Tato vazba umožňuje vytvořit, odstranit a aktualizovat webhook odběry v aplikaci Microsoft Graph.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-output-binding"></a>Konfigurace Microsoft Graph webhooku předplatné výstup vazby

Vazba podporuje následující vlastnosti:

|Vlastnost|Popis|
|--------|--------|
|**Jméno**|Požadovaná proměnná používá v kódu funkce pro e-mailovou zprávu. V tématu [pomocí zprávy Outlooku výstup vazba z kódu](#outlook-output-code).|
|**Typ**|Vyžaduje - musí být nastavena na `graphWebhookSubscription`.|
|**směr**|Vyžaduje - musí být nastavena na `out`.|
|**identity**|Požadovaná identity, která se použije k provedení akce. Může být jedna z následujících hodnot:<ul><li><code>userFromRequest</code>-Jedinými platnými s [triggeru protokolu HTTP]. Použije identitu volajícího uživatele.</li><li><code>userFromId</code>-Použije identitu dříve přihlášeného uživatele se zadaným ID. Najdete v článku <code>userId</code> vlastnost.</li><li><code>userFromToken</code>-Použije identitu reprezentována zadaný token. Najdete v článku <code>userToken</code> vlastnost.</li><li><code>clientCredentials</code>-Použije identitu aplikace funkce.</li></ul>|
|**ID uživatele** |V případě potřeby a pouze v případě _identity_ je nastaven na `userFromId`. ID objektu zabezpečení uživatele přidružené k dříve přihlášeného uživatele.|
|**userToken**|V případě potřeby a pouze v případě _identity_ je nastaven na `userFromToken`. Token platný pro funkce aplikace. |
|**Akce**|Požadováno – určuje má provést akce vazby. Může být jedna z následujících hodnot:<ul><li><code>create</code>-Zaregistruje nové předplatné.</li><li><code>delete</code>-Odstraní určený odběr.</li><li><code>refresh</code>-Aktualizuje určený odběr Zabraňte vypršení platnosti.</li></ul>|
|**subscriptionResource**|V případě potřeby a pouze v případě _akce_ je nastaven na `create`. Určuje prostředek Microsoft Graph, které se budou monitorovat změny. V tématu [práce s webhooky v aplikaci Microsoft Graph]. |
|**ChangeType –**|V případě potřeby a pouze v případě _akce_ je nastaven na `create`. Určuje typ změny v odebírané prostředek, který vyvolá oznámení. Podporované hodnoty jsou: `created`, `updated`, `deleted`. Více hodnot lze spojovat pomocí seznam oddělený čárkami.|

#### <a name="using-a-microsoft-graph-webhook-subscription-output-binding-from-code"></a>Pomocí aplikace Microsoft Graph webhooku předplatné výstup vazba z kódu

Vazba zveřejňuje následující typy, které mají funkce .NET:
- Řetězec
- Microsoft.Graph.Subscription

Příklady použití této vazby v kódu, najdete v tématu [Microsoft Graph webhooku ukázky](#webhook-samples).
 
<a name="webhook-samples"></a>
### <a name="microsoft-graph-webhook-samples"></a>Ukázky webhooku Microsoft Graph

#### <a name="sample-creating-a-subscription"></a>Ukázka: Vytváření odběru

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP s výkonem předplatné vazby pomocí akce vytvořit:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# zaregistruje webhook, jehož oznámí této funkce aplikace při volání uživatel obdrží zprávu aplikace Outlook:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

Následující ukázka JS zaregistruje webhook, jehož oznámí této funkce aplikace při volání uživatel obdrží zprávu aplikace Outlook:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

#### <a name="sample-handling-notifications"></a>Ukázka: Zpracování oznámení

Předpokládejme, že máte následující function.json, která definuje aktivační události webhooku grafu pro zpracování zpráv aplikace Outlook:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# reaguje na příchozí e-mailových zpráv a protokoly text těchto poslal příjemce a obsahuje "Azure Functions" v předmětu:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

Následující ukázka JS reaguje na příchozí e-mailových zpráv a protokoly text těchto poslal příjemce a obsahuje "Azure Functions" v předmětu:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

#### <a name="sample-deleting-subscriptions"></a>Ukázka: Odstranění odběrů

Předpokládejme, že máte následující function.json, která definuje aktivační procedury HTTP s vazbu vstupní předplatného a předplatné výstup, vazbu pomocí akce odstranění:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# získá všechna předplatná pro volání uživatele a poté se odstraní je:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

Následující ukázka JS získá všechna předplatná pro volání uživatele a poté se odstraní je:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

#### <a name="sample-refreshing-subscriptions"></a>Ukázka: Aktualizace odběrů

Aktualizace odběrů dvěma způsoby:
- Pomocí identity aplikace řešit všechny odběry. To bude vyžadovat souhlas ze Správce Azure Active Directory. Tímto lze ve všech jazycích, které podporuje Azure Functions.
- Použít identitu přidruženou každé předplatné pomocí ruční vytvoření vazby každé ID uživatele. To bude vyžadovat některé vlastní kód k provedení vazby. Můžete použít pouze pomocí funkce .NET.

Níže jsou uvedeny obě možnosti.

**Pomocí identity aplikace**

Předpokládejme, že máte následující function.json, která definuje aktivační událost časovače předplatné výstup vstupní vazby předplatné, vazbu, pomocí identity aplikace:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# aktualizuje odběry časovače, pomocí identity aplikace:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

Následující ukázka JS aktualizuje odběry časovače, pomocí identity aplikace:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

**Použití dynamické uživatelských identit**

Pro možnost alternativní odložit vazby k předplatnému Předpokládejme, že máte následující function.json, která definuje aktivační událost časovače, vstup vazbu na kód funkce:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Následující ukázka C# aktualizuje odběry časovače, pomocí identity jednotlivých uživatelů tím, že vytvoříte výstupní vazby v kódu:
```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.Info($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```



[triggeru protokolu HTTP]: functions-bindings-http-webhook.md
[práce s webhooky v aplikaci Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks

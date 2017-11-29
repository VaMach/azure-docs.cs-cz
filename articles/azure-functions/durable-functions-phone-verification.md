---
title: "Zásahem ze strany a vypršení časových limitů trvanlivý funkcí – Azure"
description: "Zjistěte, jak zpracovat zásahem ze strany a vypršení časových limitů v rozšíření trvanlivý funkce pro Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 822abf5cd09a0cd0d66441acfe4ae114c6ba73eb
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="human-interaction-in-durable-functions---phone-verification-sample"></a>Zásahem ze strany v trvanlivý funkce – Ukázka ověření telefonu

Tento příklad znázorňuje, jak vytvořit [trvanlivý funkce](durable-functions-overview.md) orchestration, který zahrnuje zásahem ze strany obsluhy. Vždy, když je skutečný člověk zahrnutých v automatizovaného procesu, musí být tento proces může odeslat oznámení uživateli a přijímat odpovědi asynchronně. Toto pravidlo musí umožňovat také možnost, že osobou, která je k dispozici. (Tento poslední část je, kde je potřeba vypršení časových limitů znát.)

Tato ukázka implementuje služby SMS na telefonu ověřovacího systému. Tyto typy toků se často používají při ověřování zákazníka telefonní číslo nebo pro službu Multi-Factor authentication (MFA). Totiž výkonné příklad celého implementace se provádí pomocí několik malých funkcí. Externích dat úložiště, jako je například databáze, je vyžadován.

## <a name="prerequisites"></a>Požadavky

* Postupujte podle pokynů v [nainstalovat trvanlivý funkce](durable-functions-install.md) nastavit vzorku.
* Tento článek předpokládá, že jste již prošli [Hello pořadí](durable-functions-sequence.md) ukázka návod.

## <a name="scenario-overview"></a>Přehled scénáře

Ověření telefonu se používá k ověření, že koncovým uživatelům vaší aplikace nejsou odesílatelé nevyžádané pošty a že jsou kdo říká se, že jsou. Službu Multi-Factor authentication je běžně používá pro ochranu před hackery uživatelské účty. Problém s implementace vlastního ověření telefonu je, že vyžaduje **stavová interakce** s člověka. Koncový uživatel se většinou poskytuje určitý kód (například číslo 4 číslice) a musí odpovídat **v přiměřené době**.

Obyčejnou Azure Functions jsou bezstavové (jako jsou mnoho dalších cloudu koncových bodů na jiných platformách), takže tyto typy interakce bude zahrnovat explicitně správě externě do databáze nebo některých jiných trvalé úložiště stavu. Kromě toho interakce musí rozdělit do více funkcí, které lze koordinovat společně. Například musíte alespoň jednu funkci pro rozhodnutí týkající se kódu, jeho uložením někde a odesílání do telefonu uživatele. Kromě toho musíte mít alespoň jeden další funkce, obdrží odpověď od uživatele a namapovat je nějakým způsobem zpět na původní volání funkce, aby bylo možné provést ověření kódu. Vypršení časového limitu je také důležité aspekt zajistit zabezpečení. To můžete získat poměrně složitá poměrně rychle.

Při použití trvanlivý funkcí, se výrazně snižuje složitost tohoto scénáře. Jak uvidíte v této ukázce, funkce orchestrator můžete spravovat stavová interakce velmi snadno a bez zásahu žádné externí datová úložiště. Protože jsou funkce orchestrator *trvanlivý*, tyto interaktivní toky jsou také vysokou spolehlivé.

## <a name="configuring-twilio-integration"></a>Konfigurace integrace Twilio

Tato ukázka zahrnuje použití [Twilio](https://www.twilio.com/) službu pro odeslání zpráv serveru SMS na mobilní telefon. Azure Functions již podporu pro Twilio prostřednictvím [Twilio vazby](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio), a ukázku pomocí této funkce.

První věcí, kterou je třeba je účet Twilio. Můžete vytvořit jeden volné na https://www.twilio.com/try-twilio. Jakmile máte účet, přidejte následující tři **nastavení aplikace** do funkce aplikace.

| Název nastavení aplikace | Hodnota Popis |
| - | - |
| **TwilioAccountSid**  | Identifikátor SID účtu Twilio |
| **TwilioAuthToken**   | Token ověření účtu Twilio |
| **TwilioPhoneNumber** | Telefonní číslo přidružené k účtu Twilio. To se používá k odesílání zpráv serveru SMS. |

## <a name="the-functions"></a>Funkce

Tento článek vás provede v ukázkové aplikace následující funkce:

* **E4_SmsPhoneVerification**
* **E4_SendSmsChallenge**

Následující části popisují konfiguraci a kód, který se používá pro vývoj na portálu Azure. Kód pro vývoj v sadě Visual Studio se zobrazí na konci tohoto článku.
 
## <a name="the-sms-verification-orchestration"></a>Orchestration ověření serveru SMS

**E4_SmsPhoneVerification** funkce používá standardní *function.json* pro orchestrator funkce.

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/function.json)]

Tady je kód, který implementuje funkce:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SmsPhoneVerification/run.csx)]

Po spuštění této funkce orchestrator provede následující akce:

1. Získá telefonní číslo, na kterou bude *odeslat* oznámení SMS.
2. Volání **E4_SendSmsChallenge** k odeslání zprávy SMS pro uživatele a vrátí zpět kód očekávané výzvy 4 číslice.
3. Vytvoří trvanlivý časovač této aktivační události 90 sekund od aktuálního času.
4. Paralelně s časovač čeká **SmsChallengeResponse** události od uživatele.

Uživatel obdrží zprávu SMS s kódem čtyři číslice. Mají 90 sekund a odešlete tento stejný kód 4 číslice zpátky instance funkce orchestrator k dokončení procesu ověření. Pokud se odeslat nesprávný kód, získají další tři pokusí získat vpravo (v rámci stejné 90 druhé okno).

> [!NOTE]
> Nemusí být zřejmé na první, ale tato orchestrator je zcela deterministická funkce. Důvodem je, že `CurrentUtcDateTime` vlastnost se používá k výpočtu čas vypršení platnosti časovače a tato vlastnost vrací stejnou hodnotu v každé opakování v tomto okamžiku v kódu produktu orchestrator. To je důležité zajistit, aby stejné `winner` výsledkem každé opakovaná volání `Task.WhenAny`.

> [!WARNING]
> Je důležité [zrušit časovače pomocí CancellationTokenSource](durable-functions-timers.md) Pokud již nepotřebujete, aby vyprší, jako v příkladu výše, pokud je odpověď na výzvu přijat.

## <a name="send-the-sms-message"></a>Odeslat zprávu SMS

**E4_SendSmsChallenge** funkce používá vazby Twilio odeslat zprávu SMS s 4 číslice kódu pro koncového uživatele. *Function.json* je definován následujícím způsobem:

[!code-json[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/function.json)]

A zde je kód, který generuje kód výzvy 4 číslice a odešle zprávu SMS:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E4_SendSmsChallenge/run.csx)]

To **E4_SendSmsChallenge** funkce získá volána pouze jednou, i když dojde k chybě procesu nebo získá přehrány. Toto je funkční, protože nechcete, aby koncový uživatel získávání více zpráv serveru SMS. `challengeCode` Vrátit hodnota se automaticky jako trvalé, aby funkce orchestrator vždy věděli, co je správný kód.

## <a name="run-the-sample"></a>Spustit ukázku

Pomocí funkce aktivované protokolem HTTP zahrnuty ve vzorku, můžete začít orchestration odesláním následující požadavku HTTP POST.

```
POST http://{host}/orchestrators/E4_SmsPhoneVerification
Content-Length: 14
Content-Type: application/json

"+1425XXXXXXX"
```
```
HTTP/1.1 202 Accepted
Content-Length: 695
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"id":"741c65651d4c40cea29acdd5bb47baf1","statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}","terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

Funkce orchestrator přijímá zadané telefonní číslo a okamžitě ji odešle zpráva SMS s ověřovacím náhodně generované 4 číslice kódem &mdash; například *2168*. Funkce potom počká, než 90 sekund pro odpověď.

Pokud chcete odpovědět s kódem, můžete použít `RaiseEventAsync` uvnitř jiné funkce nebo vyvolat **sendEventUrl** webhooku HTTP POST, kterou se odkazuje v 202 odpovědi výše, nahraďte `{eventName}` s názvem události, `SmsChallengeResponse`:

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1/raiseEvent/SmsChallengeResponse?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Length: 4
Content-Type: application/json

2168
```

Odešlete tento časovač vyprší, dokončení orchestration a `output` je nastaveno na `true`, která znamená úspěšné ověření.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/741c65651d4c40cea29acdd5bb47baf1?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```
```
HTTP/1.1 200 OK
Content-Length: 144
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":true,"createdTime":"2017-06-29T19:10:49Z","lastUpdatedTime":"2017-06-29T19:12:23Z"}
```

Pokud necháte časovač vyprší platnost, nebo pokud jste zadali nesprávný kód čtyřikrát, můžete zadat dotaz na stav a v tématu `false` funkce orchestration výstupu označující, že ověření telefonu se nezdařilo.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 145

{"runtimeStatus":"Completed","input":"+1425XXXXXXX","output":false,"createdTime":"2017-06-29T19:20:49Z","lastUpdatedTime":"2017-06-29T19:22:23Z"}
```

## <a name="visual-studio-sample-code"></a>Visual Studio ukázkový kód

Tady je orchestration jako jeden soubor jazyka C# v projektu sady Visual Studio:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/PhoneVerification.cs)]

## <a name="next-steps"></a>Další kroky

Tato ukázka vám ukázal, některé rozšířené možnosti trvanlivý funkcí, zejména `WaitForExternalEvent` a `CreateTimer`. Už víte, jak tyto mohou být kombinovány s `Task.WaitAny` implementovat spolehlivé vypršení časového limitu systému, což je často užitečné pro interakci s skutečné osoby. Další informace o tom, jak použít trvanlivý funkce čtením řada články, které nabízí podrobný krytí konkrétní témata.

> [!div class="nextstepaction"]
> [Přejít na první článek v řadě](durable-functions-bindings.md)

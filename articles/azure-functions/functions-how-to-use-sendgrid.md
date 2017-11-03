---
title: "Jak používat sendgrid vám umožňuje v Azure Functions | Microsoft Docs"
description: "Ukazuje, jak používat sendgrid vám umožňuje v Azure Functions"
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: 563bdac30176de131f6d8fd194713f482ceb75d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a>Jak používat sendgrid vám umožňuje v Azure Functions

## <a name="sendgrid-overview"></a>Přehled sendgrid vám umožňuje

Azure Functions podporuje sendgrid vám umožňuje výstupní vazby funkcí k odesílání e-mailové zprávy s několika řádků kódu a sendgrid vám umožňuje účet povolit.

Chcete-li použít rozhraní API sendgrid vám umožňuje v funkci Azure, musíte mít [sendgrid vám umožňuje účet](http://SendGrid.com). Kromě toho musí mít klíč rozhraní API sendgrid vám umožňuje. Přihlaste se ke svému účtu sendgrid vám umožňuje a klikněte na tlačítko **nastavení** pak **klíč rozhraní API** vygenerovat klíč rozhraní API. Uchovávejte tento klíč k dispozici při používání v nadcházející kroku.

Nyní jste připraveni vytvořit aplikaci funkce Azure.

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App 

Aplikace Azure funkce jsou kontejnery pro jeden nebo více Azure functions. Řešení Azure functions se právě který – funkce. Jednotlivé funkce Azure je vázaný na jedna aktivační událost, která je událost, která způsobí, že funkce spustit.
Jednotlivé funkce může obsahovat libovolný počet vstup nebo výstup vazby. Vazby jsou služby, které můžete použít ve funkci. Sendgrid vám umožňuje je vazbu výstup, které můžete použít k odesílání e-mailu. 

1. Přihlaste se k portálu Azure a [vytvoření aplikace Azure funkce](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) nebo otevřete stávající funkce aplikace. 
2. Vytvoření Azure funkce. Chcete-li jednoduchost, zvolte ruční aktivační události a C#. 

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Sendgrid vám umožňuje nakonfigurovat pro použití v aplikaci Azure – funkce

Musíte si klíč rozhraní API sendgrid vám umožňuje uložit jako nastavení aplikace pro použití ve funkci. Pole ApiKey není klíč skutečné sendgrid vám umožňuje rozhraní API, ale definovat nastavení aplikace, které představuje skutečný klíč rozhraní API. Ukládání klíče tímto způsobem je pro zabezpečení, protože je oddělená od žádné kódu nebo soubory, které může zkontrolovat do správy zdrojového kódu.

- Vytvoření **AppSettings** klíče v aplikaci funkce **nastavení aplikace**.

 ![Vytvoření Azure funkce](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Konfigurovat sendgrid vám umožňuje výstup vazby

Je k dispozici jako Azure sendgrid vám umožňuje funkce výstup vazby. Chcete-li vytvořit sendgrid vám umožňuje výstup vazby:

1. Přejděte na **integrací** karta funkce na portálu Azure.
2. Klikněte na tlačítko **nový výstupní** sendgrid vám umožňuje vytvořit výstup vazby.
3. Vyplňte **klíč rozhraní API** a **název parametru zpráva** vlastnosti. Pokud chcete, můžete zadat další vlastnosti nyní, nebo místo toho je kód. Tato nastavení mohou být použity jako výchozí.

 ![Konfigurovat sendgrid vám umožňuje výstup vazby](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Přidání vazbu na funkci vytvoří soubor s názvem **function.json** ve složce vaší funkce. Tento soubor obsahuje všechny stejné informace, které se zobrazí v Azure funkce **integrací** kartě, ale ve formátu Json formátu. Nastavení **ApiKey**, **zpráva**, a **z** pole vytvořit následující záznamy v **function.json** souboru: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Pokud dáváte přednost, můžete to upravovat soubor sami přímo.

Teď, když jste vytvořili a nakonfigurovali aplikaci funkce a funkce, můžete napsat kód odeslat e-mail.

## <a name="write-code-that-creates-and-sends-email"></a>Psaní kódu, které vytváří a odesílá e-mailu

Rozhraní API sendgrid vám umožňuje obsahuje všechny příkazy, které potřebujete k vytvoření a odeslání e-mailu.  

- Nahraďte kód ve funkci s následujícím kódem:

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change to email of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Všimněte si první řádek obsahuje ```#r``` direktiva, která odkazuje na sestavení sendgrid vám umožňuje. Potom můžete použít ```using``` příkaz více snadný přístup k objektům v daném oboru názvů. V kódu, vytváření instancí ```Mail```, ```Personalization```, a ```Content``` objekty z rozhraní API Sendgridu, které tvoří e-mailu. Při návratu zprávy, přináší sendgrid vám umožňuje ho. 

Podpis funkce je také obsahuje další parametr typu out ```Mail``` s názvem ```message```. Obě vstup a výstup vazby express sami jako parametry funkce v kódu. 

2. Otestujte svůj kód kliknutím **Test** a zadání zprávy do **text žádosti** pole, pak levým na **spustit** tlačítko.

 ![Otestujte svůj kód](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Zkontrolujte e-mailu, chcete-li ověřit, že sendgrid vám umožňuje odeslat e-mailu. Měl by přejděte na adresu v kódu z kroku 1 a obsahovat zprávu od **text žádosti**.

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak pomocí služby sendgrid vám umožňuje vytvořit a odeslat e-mailu. Další informace o používání služby Azure Functions ve svých aplikacích najdete v následujících tématech: 

- [Osvědčené postupy pro Azure Functions](functions-best-practices.md) uvádí některé osvědčené postupy pro použití při vytváření Azure Functions.

- [Referenční informace pro vývojáře Azure Functions](functions-reference.md) referenční informace pro programátory pro kódování funkcí a definování triggerů a vazeb.

- [Testování Azure Functions](functions-test-a-function.md) popisuje různé nástroje a techniky pro testování funkcí.
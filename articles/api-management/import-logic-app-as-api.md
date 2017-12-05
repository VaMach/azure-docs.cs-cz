---
title: "Importovat aplikace logiky jako rozhraní API pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak importovat aplikaci logiky jako rozhraní API pomocí rozhraní API správy (APIM)."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 0bab180fe726eeece9d03883889253825b5e9560
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="import-a-logic-app-as-an-api"></a>Importovat aplikace logiky jako rozhraní API

Tento článek ukazuje, jak importovat aplikaci logiky jako rozhraní API. Článek také ukazuje, jak otestovat APIM rozhraní API.

V tomto článku se dozvíte, jak:

> [!div class="checklist"]
> * Importovat aplikace logiky jako rozhraní API
> * Test rozhraní API na portálu Azure
> * Testování rozhraní API v portálu pro vývojáře

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md)
+ Zkontrolujte, zda že je aplikace logiky v rámci vašeho předplatného. Další informace najdete [vytvoření první aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importu a publikovat rozhraní API back-end

1. Vyberte **rozhraní API** z pod **API MANAGEMENT**.
2. Vyberte **aplikace logiky** z **přidání nového rozhraní API** seznamu.

    ![Aplikace logiky](./media/import-logic-app-as-api/logic-app-api.png)
3. Stiskněte klávesu **Procházet** zobrazíte seznam Logic Apps v rámci vašeho předplatného.
4. Vyberte aplikaci. APIM vyhledá ho načte swagger spojené s vybranou aplikaci a naimportuje ho. 
5. Přidáte přípona adresy URL rozhraní API. Přípona je název, který identifikuje toto konkrétní rozhraní API v této instanci APIM. Musí být v této instanci APIM jedinečné.
6. Rozhraní API publikujete tím, že přidružíte rozhraní API s produktem. V takovém případě "*neomezený*" slouží k produktu.  Pokud chcete použít pro rozhraní API publikovat a být k dispozici pro vývojáře, přidejte ho k produktu. To můžete provést při vytváření rozhraní API nebo nastavit později.

    Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu. Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.

    Ve výchozím nastavení každá instance služby API Management obsahuje dva ukázkové produkty:

    * **Starter**
    * **Unlimited**   
7. Vyberte **Vytvořit**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Testování nového rozhraní API APIM na portálu Azure

Operace lze volat přímo z portálu Azure, který představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API.  

1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Stiskněte **Test** kartě.
3. Vyberte nějaké operace.

    Stránka zobrazí pole pro parametry dotazu a pole hlavičky. Jednu z hlaviček "Ocp-Apim-Subscription-Key", je pro předplatné klíč produktu, který je přidružen toto rozhraní API. Pokud jste vytvořili instanci APIM, jste správcem již, tak klíč se vyplní automaticky. 
1. Stiskněte klávesu **odeslat**.

    Back-end odpoví **200 OK** a některá data.

## <a name="call-operation"></a>Volání operace z portálu pro vývojáře

Operace lze také volat **portál pro vývojáře** k testování rozhraní API. 

1. Vybrat rozhraní API, kterou jste vytvořili v "Import a publikovat rozhraní API back-end" krok.
2. Stiskněte klávesu **portál pro vývojáře**.

    Otevře webu "Portál pro vývojáře".
3. Vyberte **rozhraní API** kterou jste vytvořili.
4. Klikněte na tlačítko operaci, kterou chcete testovat.
5. Stiskněte klávesu **vyzkoušet**.
6. Stiskněte klávesu **odeslat**.
    
    Po vyvolání operace portál pro vývojáře zobrazí **Stav odpovědi**, **Hlavičky odpovědi** a jakýkoli **Obsah odpovědi**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
> Má každý aplikace logiky **ruční vyvolání** operaci. Pokud chcete tvoří rozhraní API více logic Apps, aby se mají kolizí, budete muset přejmenovat funkce.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Transformace a chránit publikované rozhraní API](transform-api.md)
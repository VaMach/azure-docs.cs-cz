---
title: "Model odpovědi rozhraní API pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak nastavit zásady na rozhraní API, tak, aby vracel mocked odpovědi pomocí rozhraní API správy (APIM). Tato metoda endables vývojářům pokračovat v provádění a v případě, že back-end není k dispozici k odeslání odpovědi skutečné testování instanci služby API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: e485071b026c52eb23532639546ad475fc92cde3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="mock-api-responses"></a>Imitované odpovědi rozhraní API

Rozhraní API back-end lze importovat do rozhraní API APIM nebo vytvořit a spravovat ručně. Kroky v tomto kurzu ukazují, jak použít APIM k vytvoření prázdné rozhraní API a spravovat ji ručně. Tento kurz ukazuje, jak nastavit zásady na rozhraní API, tak, aby vracel mocked odpověď. Tato metoda umožňuje vývojářům pokračovat v provádění a testování APIM instance, i když není k dispozici k odeslání odpovědi skutečné back-end. Možnost model až odpovědí může být užitečné v různých scénářích:

+ Když průčelí rozhraní API za slouží nejprve a back-end implementace dodává se později. Nebo back-end je vyvíjen paralelně.
+ Pokud back-end není dočasně provozních nebo nebylo možné škálovat.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření testu rozhraní API 
> * Přidat operaci do rozhraní API testu
> * Povolit mocking odpovědi
> * Testování mocked rozhraní API

![Odpověď mocked operace](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Požadavky

Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-test-api"></a>Vytvoření testu rozhraní API 

Postup v této části ukazují, jak vytvořit prázdnou rozhraní API s žádné back-end. Také ukazuje, jak přidat operaci do rozhraní API. Volání operace po dokončení kroků v této části vyvolá chybu. Zobrazí se žádné chyby a po dokončení kroků v části "Povolit mocking odpověď".

1. Vyberte **rozhraní API** z pod **API MANAGEMENT**.
2. V nabídce vlevo vyberte **+ přidat rozhraní API**.
3. Vyberte **prázdné API** ze seznamu.
4. Zadejte "*Test API*" pro **zobrazovaný název**.
5. Zadejte "*neomezená*" pro **produkty**.
6. Vyberte **Vytvořit**.

## <a name="add-an-operation-to-the-test-api"></a>Přidat operaci do rozhraní API testu

1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Klikněte na tlačítko **+ operace přidání**.

    ![Odpověď mocked operace](./media/mock-api-responses/mock-api-responses02.png)

    |Nastavení|Hodnota|Popis|
    |---|---|---|
    |**Adresa URL** (příkazu HTTP)|GET|Můžete se z jednoho z předdefinovaných příkazy HTTP.|
    |**ADRESA URL** |*/Test*|Cestu adresy URL pro rozhraní API. |
    |**Zobrazovaný název**|*Volání testu*|Název, který se zobrazí v **portál pro vývojáře**.|
    |**Popis**||Zadejte popis, operace, která slouží k poskytování dokumentaci pro vývojáře, kteří používají toto rozhraní API v **portál pro vývojáře**.|
    |**Dotaz** karta||Můžete přidat parametry dotazu. Kromě toho poskytuje název a popis, můžete zadat hodnoty, které lze přiřadit k tento parametr. Jedna z hodnot může být označen jako výchozí (volitelné).|
    |**Žádosti o** karta||Můžete definovat typy obsahu požadavku, příklady a schémat. |
    |**Odpověď** karta|Informace o kroky podle této tabulky.|Zadejte kódy stavu odpovědi, typy obsahu, příklady a schémat.|

3. Vyberte **odpovědi** kartě nachází v adrese URL, zobrazovaný název a popis pole.
4. Klikněte na tlačítko **+ přidat odpovědi**.
5. Vyberte **200 OK** ze seznamu.
6. V části **reprezentace** záhlaví na pravé straně, vyberte **+ přidat reprezentace**.
7. Zadejte "*application/json*" do vyhledávacího pole a vyberte **application/json** typ obsahu.
8. V **ukázka** text zadejte "*{"sampleField":"test"}*".
9. Vyberte **Uložit**.

## <a name="enable-response-mocking"></a>Povolit mocking odpovědi

1. Vyberte rozhraní API, které jste vytvořili v kroku "Vytvoření rozhraní API testu".
2. Vyberte testovací operace, které jste přidali.
2. V okně na pravé straně klikněte **návrhu** kartě.
3. V **zpracování příchozích** okně klikněte na ikonu tužky.
4. V **Mocking** vyberte **statické odpovědi** pro **Mocking chování**.
5. V **API Management vrátí následující odpověď:** textového pole, typ **200 OK, aplikace nebo json**. Tento výběr určuje, že vaše rozhraní API by měla vrátit odpověď ukázku, kterou jste zadali v předchozí části.
6. Vyberte **Uložit**.

## <a name="test-the-mocked-api"></a>Testování mocked rozhraní API

1. Vyberte rozhraní API, které jste vytvořili v kroku "Vytvoření rozhraní API testu".
2. Otevřete **Test** kartě.
3. Ujistěte se, **testování volání** rozhraní API je vybrána.

    > [!TIP]
    > Žlutý pruh s textem **Mocking je povoleno** označuje, že odpovědí vrácená z rozhraní API správy odešle mocking zásady a není odpověď skutečné back-end.

3. Vyberte **odeslat** aby testu volání.
4. **Odpověď HTTP** zobrazí kód JSON zadaný jako ukázka v první části tohoto kurzu.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření testu rozhraní API
> * Přidat operaci do rozhraní API testu
> * Povolit mocking odpovědi
> * Testování mocked rozhraní API

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Transformace a chránit publikované rozhraní API](transform-api.md)

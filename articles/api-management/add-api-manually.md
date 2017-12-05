---
title: "Přidání rozhraní API ručně pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak ručně přidat rozhraní API pomocí rozhraní API správy (APIM)."
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
ms.openlocfilehash: 9426839f88daece1bb688a2079b7854ccaebdc57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="add-an-api-manually"></a>Ruční přidání rozhraní API 

Kroky v tomto článku ukazují, jak pomocí portálu Azure k ručnímu přidání rozhraní API do instance API Management (APIM). Běžný scénář, kdy je vhodné vytvořit prázdnou rozhraní API a definovat ručně je, pokud chcete model rozhraní API. Podrobnosti o mocking rozhraní API najdete v tématu [model rozhraní API odpovědi](mock-api-responses.md).

Pokud chcete importovat existující rozhraní API, přečtěte si téma [Příbuzná témata](#related-topics) části.

V tomto článku jsme vytvořte prázdné rozhraní API a zadejte [httpbin.org](http://httpbin.org) (veřejné testování service) jako rozhraní API back-end.

## <a name="prerequisites"></a>Požadavky

Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Vytvoření rozhraní API

1. Vyberte **rozhraní API** z pod **API MANAGEMENT**.
2. V nabídce vlevo vyberte **+ přidat rozhraní API**.
3. Vyberte **prázdné API** ze seznamu.

    ![Prázdné rozhraní API](media/add-api-manually/blank-api.png)
4. Zadejte nastavení pro rozhraní API.

    ![Nastavení](media/add-api-manually/settings.png)

    |**Název**|**Hodnota**|**Popis**|
    |---|---|---|
    |**Zobrazovaný název**|"*Prázdná rozhraní API*" |Tento název se zobrazí na portálu pro vývojáře.|
    |**Adresu URL webové služby** (volitelné)| "*http://httpbin.org*"| Pokud chcete model rozhraní API, nemusí nic zadejte. <br/>V takovém případě jsme zadejte [http://httpbin.org](http://httpbin.org). Toto je veřejný testování služby. <br/>Pokud chcete importovat rozhraní API, který je namapovaný na back-end, automaticky, najdete v tématech v [Příbuzná témata](#related-topics) části.|
    |**Schéma adresy URL**|"*HTTPS*"|V tomto případě to i v případě, že back-end má nezabezpečené přístup protokolu HTTP, určíme zabezpečený přístup HTTPS APIM k back-end. <br/>Tento druh scénář (HTTPS do HTTP) se nazývá ukončení protokolu HTTPS. Je třeba udělat, pokud vaše rozhraní API existuje v rámci virtuální sítě (kde víte, že přístup je bezpečné, i když se nepoužívá protokol HTTPS). <br/>Můžete chtít použít "Ukončení protokolu HTTPS" pro uložení v některé cyklů procesoru.|
    |**Přípona adresy URL**|"*hbin*"| Přípona je název, který identifikuje toto konkrétní rozhraní API v této instanci APIM. Musí být v této instanci APIM jedinečné.|
    |**Produkty**|"*Neomezená*" |Rozhraní API publikujete tím, že přidružíte rozhraní API s produktem. Pokud chcete použít pro rozhraní API publikovat a být k dispozici pro vývojáře, přidejte ho k produktu. To můžete provést při vytváření rozhraní API nebo nastavit později.<br/><br/>Produkty jsou přidružení jeden nebo více rozhraní API. Může obsahovat několik rozhraní API a poskytněte jim pro vývojáře prostřednictvím portálu pro vývojáře. <br/>Vývojáři musí nejdřív přihlásit k odběru produktu získat přístup k rozhraní API. Pokud se přihlášení k odběru získají předplatné klíč, který je vhodný pro jakéhokoli rozhraní API v produktu. Pokud jste vytvořili instanci APIM, jste správcem již, takže jsou přihlášení k odběru každého produktu ve výchozím nastavení.<br/><br/> Ve výchozím nastavení, každá instance API Management obsahuje dva ukázkové produkty: **Starter** a **neomezený**.| 
5. Vyberte **Vytvořit**.

V tuto chvíli nemáte žádné operace APIM, které se mapují na operace v rozhraní API back-end. Jestliže bude volat operace, který je zveřejněný prostřednictvím back-end, ale ne prostřednictvím APIM, můžete získat **404**. 

>[!NOTE] 
> Ve výchozím nastavení, při přidání rozhraní API, i v případě, že je připojený k některé back endové službě APIM nebude vystavit žádné činnosti, dokud jste povolených je. Operaci služby back-end do seznamu povolených IP adres vytvořit APIM operace, která se mapuje na operaci back-end.
>

## <a name="add-and-test-an-operation"></a>Přidejte a testování operací

V této části ukazuje, jak přidat operace "/ get", aby mapování na operaci "http://httpbin.org/get" back-end.

### <a name="add-the-operation"></a>Přidání operace

1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Klikněte na tlačítko **+ operace přidání**.
3. V **URL**, vyberte **získat** a zadejte "*/get*" v prostředku.
4. Zadejte "*FetchData*" pro **zobrazovaný název**.
5. Vyberte **Uložit**.

### <a name="test-the-operation"></a>Otestování provozu

Operaci testování na portálu Azure. Alternativně můžete otestovat ho **portál pro vývojáře**.

1. Vyberte **Test** kartě.
2. Vyberte **FetchData**.
3. Stiskněte klávesu **odeslat**.

Se zobrazí odpověď, který generuje operaci "http://httpbin.org/get". Pokud chcete k transformaci vaše operace, najdete v části [transformovat a chránit vaše rozhraní API](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Přidat a otestovat parametrizované operace

V této části ukazuje, jak přidat operace, která přebírá parametr. V takovém případě jsme namapovat operace "http://httpbin.org/status/200".

### <a name="add-the-operation"></a>Přidání operace

1. Vyberte rozhraní API, které jste vytvořili v předchozím kroku.
2. Klikněte na tlačítko **+ operace přidání**.
3. V **URL**, vyberte **získat** a zadejte "*/status/ {code}*" v prostředku. Volitelně můžete zadat nějaké informace související s použitím tohoto parametru. Zadejte například "*číslo*" pro **typ**, "*200*" (výchozí) pro **hodnoty**.
4. Zadejte "GetStatus" **zobrazovaný název**.
5. Vyberte **Uložit**.

### <a name="test-the-operation"></a>Otestování provozu 

Operaci testování na portálu Azure.  Alternativně můžete otestovat ho **portál pro vývojáře**.

1. Vyberte **Test** kartě.
2. Vyberte **GetStatus**. Ve výchozím nastavení je hodnota kódu hodnotu "*200*". Můžete ji k testování ostatní hodnoty změnit. Můžete například zadat "*418*".
3. Stiskněte klávesu **odeslat**.

    Se zobrazí odpověď, který generuje operaci "http://httpbin.org/status/200". Pokud chcete k transformaci vaše operace, najdete v části [transformovat a chránit vaše rozhraní API](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Transformace a chránit publikované rozhraní API](transform-api.md)

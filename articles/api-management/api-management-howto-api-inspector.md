---
title: "Ladění vašich rozhraní API pomocí trasování požadavků ve službě Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu se dozvíte, jak zkontrolovat kroky v Azure API Management se zpracováním požadavků."
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
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 7b9bec7927169b9d820c095a7d11705264e7dcfe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="debug-your-apis-using-request-tracing"></a>Ladění vašich rozhraní API pomocí trasování žádostí

Tento kurz popisuje, jak zkontrolovat zpracování požadavků, které vám pomohou s ladění a řešení potíží s vaše rozhraní API. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Trasování volání

![Rozhraní API inspector](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="trace-a-call"></a>Trasování volání

1. Vyberte **rozhraní API**.
2. Klikněte na tlačítko **ukázku konferenční API** ze seznamu rozhraní API.
3. Vyberte **GetSpeakers** operaci.
4. Přepnout **Test** kartě.
5. Nezapomeňte zahrnout záhlaví HTTP pojmenované **Ocp-Apim-trasování** nastavená na hodnotu **true**.
6. Klikněte na tlačítko **"Odeslat"** aby volání rozhraní API. 
7. Počkejte na dokončení volání. 
8. Přejděte na **trasování** ve **rozhraní API konzoly**. Můžete kliknout na některý z následujících odkazů na informace o podrobné trasování: **příchozí**, **back-end**, **odchozí**.

    V **příchozí** části, se zobrazí původní žádost API Management přijatou z volající a všechny zásady uplatněné na žádost, včetně omezení četnosti a zásad hlavičky set jsme přidali v kroku 2.

    V **back-end** části se zobrazí požadavky, API Management odeslané na rozhraní API back-end a obdržel odpověď.
    
    V **odchozí** části, se zobrazí všechny zásady u odpovědi před odesláním zpět k volajícímu.
 
    > [!TIP]
    > Každý krok také ukazuje uplynulý čas, protože přijme požadavek na API Management.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Trasování volání

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Revize použití](api-management-get-started-revise-api.md)

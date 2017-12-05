---
title: "Monitorování publikovaná rozhraní API v Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu se dozvíte, jak sledovat vaše rozhraní API v Azure API Management."
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
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Monitorování publikovaných rozhraní API

Azure monitorování je služba Azure, která poskytuje jednoho zdroje pro monitorování všech prostředků Azure. S Azure monitorování můžete vizualizovat, dotaz, směrovat, archivaci a provádět akce na metriky a protokoly pocházejících z prostředků Azure, jako je například Správa rozhraní API. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zobrazení protokolů aktivit
> * Zobrazení diagnostických protokolů
> * Zobrazit metriky rozhraní API 
> * Nastavit pravidlo výstrahy, když získá neoprávněným volání rozhraní API

Následující video ukazuje, jak monitorovat pomocí monitorování Azure API Management. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Zobrazit protokoly aktivity

Protokoly aktivity získat přehled o činnosti, které byly provedeny v vaše služby API Management. Pomocí protokolů z aktivity, můžete určit ", kdo a kdy" pro všechny operace (PUT, POST, DELETE) na vaše rozhraní API správy služby zápisu. 

> [!NOTE]
> Protokoly aktivity nezahrnují operace čtení (GET) nebo operace provádět na klasickém portálu vydavatele nebo pomocí původní rozhraní API pro správu.

Lze zobrazit protokoly aktivit ve službě API Management nebo přístup k protokolům všechny prostředky Azure v Azure monitorování. 

Chcete-li zobrazit protokoly aktivity:

1. Z vaší **API Management** instance, klikněte na tlačítko **protokol aktivit**.

## <a name="view-diagnostic-logs"></a>Zobrazení diagnostických protokolů

Diagnostické protokoly poskytují bohaté informace o operacích a chyby, které jsou důležité pro auditování i pro účely odstraňování potíží. Diagnostické protokoly se liší od protokoly aktivity. Protokoly aktivity poskytují přehled o činnosti, které byly provedeny v vašich prostředků Azure. Diagnostické protokoly získat přehled o operace, aby prostředku provedeny sám sebe.

Pro přístup k diagnostickým protokolům:

1. Z vaší **API Management** instance, klikněte na tlačítko **protokolů diagnostiky**.

## <a name="view-metrics-of-your-apis"></a>Zobrazit metriky vaše rozhraní API

API Management vysílá metriky každou minutu, která poskytuje téměř v reálném čase přehled o stavu a stavu vašich rozhraní API. Následuje souhrn některých dostupné metriky:

* Kapacita (preview): pomáhá vám při rozhodování o upgradu nebo přechod na starší verzi vaše APIM služby. Metrika je vygenerované za minutu a odráží kapacitu brány v době vytváření sestav. Metrika v rozsahu od 0 až 100 a se vypočítává podle brány prostředků, jako je například využití procesoru a paměti.
* Celkový počet požadavků brány: počet rozhraní API požadavků v období. 
* Úspěšné požadavky brány: počet žádostí o rozhraní API, které přijaly kódy úspěšné odpovědi HTTP včetně 304, 307 a nic menší než 301 (například 200). 
* Neúspěšné požadavky brány: počet žádostí o rozhraní API, které obdržel chybné kódy odpovědi HTTP včetně 400 a nic větší než 500.
* Neoprávněné žádosti brány: počet žádostí o rozhraní API, které přijaly, včetně 401, 403 a 429 kódy odpovědi HTTP. 
* Další požadavky na brány: počet žádostí o rozhraní API, které přijaly kódy odpovědí protokolu HTTP, které nepatří do žádné z výše uvedených skupin (například 418).

Pro přístup k metriky:

1. Vyberte **metriky** v nabídce v dolní části stránky.
2. Z rozevíracího seznamu vyberte metriky, které vás zajímají (můžete přidat více metriky). 
    
    Vyberte například **celkový počet požadavků brány** a **neúspěšné požadavky brány** ze seznamu dostupných metrik.
3. Graf zobrazuje celkový počet volání rozhraní API. Také ukazuje počet volání rozhraní API, které se nezdařilo. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Nastavit pravidlo výstrahy pro neoprávněného požadavku

Můžete nakonfigurovat přijímat výstrahy založené na protokolech metriky a aktivity. Azure monitorování umožňuje nakonfigurovat výstrahu při aktivaci, proveďte následující:

* Odeslat oznámení e-mailu
* Volat webhook, jehož
* Vyvolání aplikace logiky Azure

Konfigurace výstrah:

1. Vyberte **výstrah pravidla** z řádku nabídek v dolní části stránky.
2. Vyberte **přidat metriky upozornění**.
3. Zadejte **název** pro tuto výstrahu.
4. Vyberte **neoprávněné žádosti brány** jako metriku k monitorování.
5. Vyberte **e-mailu vlastníci, přispěvatelé a čtenáři**.
6. Stiskněte **OK**.
7. Zkuste pro volání rozhraní API naší konference bez klíč rozhraní API. Jako vlastník této služby API Management obdržíte e-mailové výstrahy. 

    > [!TIP]
    > Pravidlo výstrahy můžete také volat háku Web nebo aplikaci logiky Azure, když se aktivuje na pozadí.

    ![množství set-upozornění](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Zobrazení protokolů aktivit
> * Zobrazení diagnostických protokolů
> * Zobrazit metriky rozhraní API 
> * Nastavit pravidlo výstrahy, když získá neoprávněným volání rozhraní API

Přechodu na další kurz:

> [!div class="nextstepaction"]
> [Trasování volání](api-management-howto-api-inspector.md)
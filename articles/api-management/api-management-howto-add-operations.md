---
title: "Přidání operací do rozhraní API v Azure API Management | Microsoft Docs"
description: "Postup přidání operací do rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 8b047c0826590d1cb6a79a2f14ca07764dc2b409
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Přidání operací do rozhraní API v Azure API Management
Před použitím rozhraní API ve službě API Management, je nutné přidat operace. Tato příručka ukazuje, jak přidávat a konfigurovat různé typy operací do rozhraní API ve službě API Management.

## <a name="add-operation"></a>Přidání operace
Operace jsou přidány a nakonfigurovat tak, aby rozhraní API na portálu vydavatele. Chcete-li získat přístup k portálu vydavatele, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][Create an API Management service instance] v kurzu [Začínáme se službou Azure API Management][Get started with Azure API Management].
> 
> 

Vyberte požadované rozhraní API na portálu vydavatele a pak vyberte **Operations** kartě. 

![Operace][api-management-operations]

Klikněte na tlačítko **přidat operace** přidat novou operaci. **Operaci nového** se zobrazí a **podpis** ve výchozím nastavení bude vybraná karta.

![Operace přidání][api-management-add-operation]

Zadejte **příkaz HTTP** výběrem z rozevíracího seznamu.

![Metoda HTTP][api-management-http-method]

<a name="url-template"></a>

Adresa URL šablony definujte zadáním fragment adresy URL, který se skládá z jedné nebo více segmenty cesty adresy URL a nula nebo více parametrů řetězce dotazu. Adresa URL šablony, připojí k základní adresu URL rozhraní API, identifikuje jednu operaci HTTP. Může obsahovat jeden nebo více s názvem proměnné částí, které jsou určeny složené závorky. Tyto proměnné části se nazývají parametry šablony a dynamicky přiřazené hodnoty extrahovat z adresy URL žádosti při zpracování žádosti podle platformy API Management.

> Šablona adresa URL může obsahovat zástupné znaky. Například zadání `/*` bude předávat všechny požadavky pro danou metodu HTTP na stránce do pozadí ukončení služby.

![Adresa URL šablony][api-management-url-template]

<a name="rewrite-url-template"></a>

V případě potřeby zadejte **přepisu adresy URL šablony**. To umožňuje používat standardní šablona adresy URL pro zpracování příchozích požadavků na front-endu, při volání back-end prostřednictvím adresy URL převedený podle šablony přepisování. Parametry šablony ze šablony, adresy URL by měla být použité v šabloně přepisování. Následující příklad ukazuje, jak obsahu typu kódovaný jako segment cesty v webovou službu z předchozího příkladu lze zadat jako parametr dotazu v rozhraní API publikovat prostřednictvím platformy pro správu rozhraní API pomocí šablon adresy URL.

![Přepisování adres URL šablony][api-management-url-template-rewrite]

Operace bude volající formát `/customers?customerid=ALFKI` a to budou mapována na `/Customers('ALFKI')` při volání služby back-end.

**Zobrazení** název a **popis** zadejte popis, operace a používají se pro dokumentaci pro vývojáře, kteří používají toto rozhraní API v portálu pro vývojáře.

![Popis][api-management-description]

Popis operace lze zadat jako prostý text nebo HTML v **popis** textové pole.

## <a name="operation-caching"></a>Operace ukládání do mezipaměti
Ukládání odpovědí do mezipaměti snižuje latence zaznamenatelného k příjemce rozhraní API, snižuje využití šířky pásma a snížení zatížení HTTP webové služby implementaci rozhraní API. 

Pokud chcete rychle a snadno povolit ukládání do mezipaměti pro operaci, vyberte **ukládání do mezipaměti** kartě a zkontrolujte **povolit** zaškrtávací políčko.

![Ukládání do mezipaměti][api-management-caching-tab]

**Doba trvání** Určuje časové období, během které odpověď operace zůstává v mezipaměti. Výchozí hodnota je 3 600 sekund nebo 1 hodina.

Klíče mezipaměti se používají k rozlišení mezi odpovědí tak, aby odpovědi odpovídající každý klíč různé mezipaměti získáte vlastní samostatné hodnotu uloženou v mezipaměti. Volitelně můžete zadat parametrů řetězce dotazu konkrétní nebo hlavičky protokolu HTTP, který se má použít v oblasti výpočetních hodnoty klíče mezipaměti v **podle parametrů řetězce dotazu** a **podle hlaviček** textová pole v uvedeném pořadí. Pokud žádný není zadaný, úplná požadavku na adresu URL a následující hodnoty hlavičky protokolu HTTP se používají v generování klíče mezipaměti: **přijmout** a **Accept-Charset**.

> Další informace o ukládání do mezipaměti a zásady ukládání do mezipaměti najdete v tématu [postupy pro ukládání do mezipaměti operace výsledků v Azure API Management][How to cache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Parametry žádosti
Parametry operace se spravují na kartě Parametry. Parametry zadané v **adresa URL šablony** na **podpis** se přidávají automaticky a lze změnit pouze úpravou šablony adresy URL. Další parametry lze zadat ručně.

Chcete-li přidat nový parametr dotazu, klikněte na tlačítko **přidat parametr dotazu** a zadejte následující informace:

* **Název** -název parametru.
* **Popis** – stručný popis parametru (volitelné).
* **Typ** – typ parametru, v rozevírací nabídce vybrali dolů.
* **Hodnoty** -hodnoty, které lze přiřadit k tento parametr. Jedna z hodnot může být označen jako výchozí (volitelné).
* **Požadované** -nastavit parametr povinný zaškrtnutím políčka. 

![Parametry žádosti][api-management-request-parameters]

## <a name="request-body"></a>Text žádosti
Pokud umožňuje operaci (např. PUT, POST) a vyžaduje text může uveďte příklad ji ve všech podporovaných reprezentace formátů (například json, XML). 

> Textu požadavku se používá pro dokumentaci pouze pro účely a není ověřen.
> 
> 

Chcete-li zadat obsah žádosti, přepněte na **textu** kartě.

Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaného typu obsahu (například application/json), vyberte v rozevíracím seznamu a vložení v příkladu textu požadované žádosti ve vybraném formátu do textového pole. 

![Text žádosti][api-management-request-body]

V další reprezentace, můžete taky určit volitelný popis v **popis** textové pole.

## <a name="responses"></a>Odpovědí
Je dobrým zvykem příklady odpovědi pro všechny stavové kódy, které může vytvořit operaci. Každý kód stavu může mít více než jeden příklad text odpovědi, jeden pro každou podporované typy obsahu. 

Chcete-li přidat odpovědi, klikněte na tlačítko **přidat** a začněte psát kód požadovaného stavu. V tomto příkladu je stavový kód **200 OK**. Jakmile se v rozevíracím seznamu se zobrazí kód, vyberte ho a kód odpovědi je vytvořen a přidán do vaší operaci.

![Kód odpovědi][api-management-response-code]

Klikněte na tlačítko **přidat reprezentace**, začněte psát název požadovaný typ obsahu (například application/json) a vyberte ho v seznamu dolů.

![Typ obsahu textu][api-management-response-body-content-type]

Příklad textu odpovědi ve vybraném formátu vložte do textového pole. 

![Text odpovědi][api-management-response-body]

V případě potřeby přidejte volitelný popis do **popis** textové pole.

Po operaci konfigurace, klikněte na tlačítko **Uložit**.

## <a name="next-steps"></a>Další kroky
Jakmile se operace, které jsou přidány do rozhraní API, dalším krokem je spojit rozhraní API s produktem a publikujete ji tak, aby vývojáři můžou volat jeho operace.

* [Postup vytvoření a publikování produktu][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md

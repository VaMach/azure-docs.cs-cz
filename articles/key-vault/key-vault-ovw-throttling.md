---
ms.assetid: 
title: "Omezení pokyny Azure Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: fe700e22c5323c2a0bdc315e349cd119798bcf40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault omezení pokyny

Omezení je proces, který zahájení, který omezuje počet souběžných volání ve službě Azure, aby se zabránilo nadměrné využití prostředků. Službou Azure Key Vault (AZURE) je určený k řešení k velkému počtu požadavků. Pokud dojde k čtenáře počet požadavků, omezení požadavků vašeho klienta pomáhá udržovat optimální výkon a spolehlivost služby službou AZURE.

Omezení omezení lišit v závislosti na scénáři. Například pokud provádíte značný zápisů, možnost omezení je vyšší než pokud pouze provádíte čtení.

## <a name="how-does-key-vault-handle-its-limits"></a>Jak Key Vault zpracovávat jeho omezení?

Omezení služby v Key Vault existují zabránili zneužití prostředky a zajistit kvality služeb pro všechny klienty Key Vault. Pokud je překročena prahová hodnota služby, limity Key Vault žádné další požadavky z tohoto klienta v časovém intervalu. V takovém případě Key Vault vrátí stavový kód HTTP 429 (příliš mnoho požadavků), a požadavky selžou. Navíc neúspěšné požadavky, které vrátí 429 směrem k mezní hodnoty omezení sledovanými Key Vault. 

Pokud máte platný obchodní případu pro vyšší šířku pásma, kontaktujte nás.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Omezení aplikace v reakci na omezení služby

Následují **osvědčené postupy** omezení aplikace:
- Snižte počet operací na základě požadavku.
- Snižte frekvenci požadavků.
- Vyhněte se okamžitě opakování. 
    - Všechny požadavky nabíhat proti vaší omezení použití.

Pokud implementujete zpracování chyb vaší aplikace, použijte kód chyby HTTP 429 k detekci potřebu omezení na straně klienta. Pokud požadavek selže s kódem chyby HTTP 429 znovu, k stále dochází omezení služby Azure. Nadále používat doporučené na straně klienta omezování metoda, opakování žádosti, dokud neproběhne úspěšně.

### <a name="recommended-client-side-throttling-method"></a>Doporučený způsob omezení na straně klienta

Na kód chyby protokolu HTTP 429 začněte omezení vašeho klienta pomocí exponenciálního omezení rychlosti přístup:

1. Počkejte 1 sekundu, opakujte žádost
2. Pokud stále omezeny počkejte 2 sekundy, opakujte žádost
3. Pokud stále omezeny čekání 4 a víc sekund, opakujte žádost
4. Pokud stále omezeny čekání 8 sekund, opakujte žádost
5. Pokud stále omezeny čekání 16 sekund, opakujte žádost

Nesmí být v tomto okamžiku získávání kódy odpovědí HTTP 429.

## <a name="see-also"></a>Viz také

Hlubší orientaci omezení na cloudu Microsoftu, najdete v části [omezení vzor](https://docs.microsoft.com/azure/architecture/patterns/throttling).


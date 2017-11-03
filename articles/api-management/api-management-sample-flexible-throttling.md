---
title: "Pokročilé omezování požadavků pomocí Azure API Management"
description: "Zjistěte, jak vytvořit a použít flexibilní kvóty a míra omezení zásad Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 356f98aec072a1295915ae0701a3e3cd793aba07
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Pokročilé omezování požadavků pomocí Azure API Management
Schopnost omezení příchozí požadavky je klíčovou roli služby Azure API Management. Buď kontrolou rychlost žádostí a celkový počet požadavků/přenášená data správy rozhraní API umožňuje zprostředkovatelé rozhraní API do příslušných rozhraní API umožňuje chránit proti zneužití a vytvořit hodnotu pro různé úrovně rozhraní API produktu.

## <a name="product-based-throttling"></a>Omezování na základě produktu
Aktuální rychlost omezení možnosti se omezená na se obor pro určitý odběr produktu (v podstatě klíč), definované v portál vydavatele služby API Management. To je užitečné pro poskytovatele rozhraní API za účelem použití omezení na vývojáři, kteří zaregistrovali používat své rozhraní API, ale jeho nepomůže, například v omezení jednotlivých koncoví uživatelé rozhraní API. Je možné, že pro jednotné uživatelské aplikace pro vývojáře spotřebovat celý kvóty a pak zabránit ostatním zákazníkům vývojáři mohli k používání aplikace. Několik zákazníci, kteří mohou vytvořit velký objem požadavků může také omezit přístup k příležitostně uživatele.

## <a name="custom-key-based-throttling"></a>Vlastní klíč na základě omezení
Nové [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady poskytují výrazně flexibilnější řešení pro řízení provozu. Tyto nové zásady umožňují definovat výrazy k identifikaci klíče, které se používají ke sledování využití provozu. Způsob, jakým tento postup funguje je nejsnažší zobrazené na příkladu. 

## <a name="ip-address-throttling"></a>Omezení IP adres
Tyto zásady omezení na IP adresu pro jednoho klienta k jenom 10 volání každou minutu, s celkem 1 000 000 volání a 10 000 kB šířky pásma za měsíc. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Pokud všichni klienti na Internetu použili jedinečnou IP adresu, může se jednat o účinný způsob omezení využití podle uživatele. Je však velmi pravděpodobné, že více uživatelů se jednu veřejnou IP adresu z důvodu je přístup k Internetu prostřednictvím zařízení NAT pro sdílení. Bez ohledu na to, pro rozhraní API umožňující přístup bez ověřování `IpAddress` může být vhodné.

## <a name="user-identity-throttling"></a>Omezení identity uživatele
Pokud koncový uživatel je ověřen a omezení klíč lze generovat na základě informací, která jednoznačně identifikuje, který uživatel.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

V tomto příkladu jsme extrahovat autorizační hlavičky, převeďte ho na `JWT` objektu a použít k identifikaci uživatele a použít je jako míru omezení klíč subjektu tokenu. Pokud je identita uživatele je uložen v `JWT` jako jednu z dalších deklarací pak hodnota může použije na příslušné místo.

## <a name="combined-policies"></a>Kombinovaná zásady
I když nové omezení zásady poskytují větší možnosti než existující zásady omezení, je stále hodnota kombinace obou možností. Omezování klíč předplatného produktu ([omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) je skvělým způsobem, jak povolit monetizing účtováním podle úrovně využití rozhraní API. Přesnější možnosti řízení moci omezení uživatelem je doplňkové a zabrání docházelo k omezení prostředí jiného chování jednoho uživatele. 

## <a name="client-driven-throttling"></a>Řízené omezení klienta
Když je omezení klíče definovaná pomocí [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx), pak je rozhraní API zprostředkovatele, který je výběr, jak je vymezen omezení. Nicméně vývojář možné řídit, jak se omezení přenosové rychlosti vlastní zákazníků. To může lze povolit zprostředkovatelem rozhraní API zavedením vlastní hlavičku pro vývojáře klientská aplikace komunikovat klíč rozhraní API.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

To umožňuje vývojáře pro klientské aplikace zvolte, jak se mají vytvořit nejpomalejší klíč. Pomocí jenom trocha vynalézavosti vývojář klienta vytvářet své vlastní míry vrstev přidělením sady klíčů pro uživatele a otáčení použití klíče.

## <a name="summary"></a>Souhrn
Azure API Management nabízí rychlost a uvozovky, omezení pro ochranu i přidejte hodnotu do vašeho rozhraní API služby. Nové zásady omezení s vlastní oboru pravidla povolit tyto zásady pro vaše zákazníkům umožňují vytvářet ještě lepší aplikací přesnější možnosti řízení. V příkladech v tomto článku ukazují použití tyto nové zásady ve výrobním míru omezení klíče s klientských IP adres, klient vygeneruje hodnoty a identity uživatele. Existují však mnoho dalších částí zprávy, která by bylo možné použít jako uživatelský agent, fragmenty cestu adresy URL, velikost zprávy.

## <a name="next-steps"></a>Další kroky
Prosím sdělte svůj názor v služby disqus pro toto téma. Budeme velmi uslyšíme jiné potenciální klíče hodnoty, které byly logické výběru v vaše scénáře.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video s přehledem těchto zásad
Další informace o [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) zásady popsaná v tomto článku prosím v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 


---
title: "Změní na koncový bod v2.0 Azure AD | Microsoft Docs"
description: "Popis změn provedených u aplikace model v2.0 verzi public preview protokolů."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3d344529d1ac9891e5a961de630dea3c1e4caab4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a>Důležité aktualizace pro ověřovací protokoly v2.0
Vývojáři pozornost! Za další dva týdny budeme do našich v2.0 ověřovací protokoly, které může to znamenat nejnovější změny pro všechny aplikace, který jste napsali naše období preview provádět několik aktualizací.  

## <a name="who-does-this-affect"></a>Kdo to vliv?
Žádné aplikace, které byly napsány pro použití v2.0 konvergované koncový bod ověřování,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Další informace o koncový bod v2.0 můžete najít [zde](active-directory-appmodel-v2-overview.md).

Pokud jste vytvořili aplikaci pomocí kódování přímo na protokol v2.0 koncový bod v2.0, pomocí některé z našich webových middlewares OpenID Connect nebo OAuth, nebo pomocí jiné 3. stran knihovny k provedení ověřování, by měl být přípravy pro testování vašich projektů a v případě potřeby proveďte změny.

## <a name="who-doesnt-this-affect"></a>Kdo nemá to vliv?
Všechny aplikace, která byla zapsána na koncový bod ověřování produkční Azure AD

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Tento protokol je nastaven trvalé a nebude zaznamenat všechny změny.

Kromě toho pokud vaše aplikace **pouze** používá naše knihovna ADAL pro ověřování, nebudete muset nic nezmění.  ADAL má Stíněný aplikaci z změny.  

## <a name="what-are-the-changes"></a>Jaké jsou změny?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Odebráním x5t hodnota hlavičky JWT
Koncový bod v2.0 používá tokeny JWT hojně, které obsahují záhlaví parametry s relevantní metadata o token.  Pokud jste dekódovat záhlaví jednoho z našich aktuální tokeny Jwt, by se najít něco podobného jako:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Kde "x5t" i "dětský" vlastnosti identifikovat veřejný klíč, který se má použít k ověření podpisu tokenu, která byla načtena z koncový bod metadat OpenID Connect.

Změna, kterou tady vydáváme je odeberte vlastnost "x5t".  Mohl nadále používat stejné mechanismy k ověření tokenů, ale spoléhat na vlastnost "dětský" načíst správný veřejný klíč, jak je uvedeno v protokolu OpenID Connect. 

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci x5t hodnota.**
> 
> 

### <a name="removing-profileinfo"></a>Odebrání profile_info
Dříve, koncový bod v2.0 má byla vrací objekt JSON kódováním base64 v tokenu odpovědí názvem `profile_info`.  Žádosti o token přístupu z koncového bodu v2.0 odesláním požadavku na:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Odpověď bude vypadat následující objekt JSON:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Hodnota obsažená informace o uživateli, který podepsal do aplikace - jejich název zobrazení, křestní jméno, příjmení, e-mailovou adresu, identifikátor a tak dále.  Především se stává `profile_info` byl použit pro ukládání tokenu do mezipaměti a zobrazit účely.

Jsou nyní odebrání `profile_info` hodnotu – ale nemusíte si dělat starosti, stále umožňujeme tyto informace pro vývojáře na místě, mírně lišit.  Místo `profile_info`, koncový bod v2.0 bude nyní se vraťte `id_token` v každé odpovědi tokenu:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Může dekódovat a analyzovat požadavku id_token načíst stejné informace, které jste dostali od profile_info.  Požadavku id_token je JSON Web Token (JWT), s obsahem podle specifikace OpenID Connect.  Kód pro tuto činnost proto by mělo být velmi podobné – jednoduše je potřeba extrahovat střední segment (text) požadavku id_token a base64 dekódování ho pro přístup k objektu JSON v rámci.

Za další dva týdny, by měl kód aplikaci načíst informace o uživateli z buď `id_token` nebo `profile_info`; podle toho, co je k dispozici.  Tímto způsobem při provádění změn, vaše aplikace může bezproblémově zpracovat přechod z `profile_info` k `id_token` bez přerušení.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci `profile_info` hodnotu.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Odebrání id_token_expires_in
Podobně jako `profile_info`, jsou také odebrání `id_token_expires_in` parametr z odpovědi.  Dřív by koncového bodu v2.0 vrátit hodnotu `id_token_expires_in` společně s každou požadavku id_token odpověď, například v odpovědi autorizovat:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Nebo v odpovědi tokenu:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Hodnotu by určit počet sekund požadavku id_token by zůstat platné pro.  Nyní jsme se odebrat `id_token_expires_in` hodnotu úplně.  Místo toho můžete použít standardní OpenID Connect `nbf` a `exp` Zkontrolujte platnost požadavku id_token deklarace identity.  Najdete v článku [odkaz tokenu v2.0](active-directory-v2-tokens.md) Další informace o těchto deklaracích identity.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace není závislá na existenci `id_token_expires_in` hodnotu.**
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a>Změna deklarace identity vrátí oborem = openid
Tato změna bude nejvýznamnější – ve skutečnosti, bude to mít vliv téměř každé aplikaci, která používá koncový bod v2.0.  Mnoho aplikací pomocí koncového bodu v2.0 odesílat požadavky `openid` nastavit obor, jako je třeba:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Dnes, když uživatel uděluje souhlas pro `openid` oboru, vaše aplikace obdrží velkému množství informací o uživateli v výsledné požadavku id_token.  Tyto deklarace identity můžou obsahovat jména, upřednostňované uživatelské jméno, e-mailovou adresu, ID objektu a další.

V této aktualizaci měníme informace, `openid` oboru poskytuje přístup vaší aplikace, k lepší comform specifikace OpenID Connect.  `openid` Obor bude pouze povolit přihlášení uživatele ve vaší aplikaci a přijímat identifikátor specifický pro aplikace pro uživatele v `sub` deklarace identity služby požadavku id_token.  Deklarací identity ve požadavku id_token se pouze `openid` rozsah povolen bude nemá žádné identifikovatelné osobní údaje.  Příklad požadavku id_token deklarace jsou:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Pokud chcete získat identifikovatelné osobní údaje (PII) o uživateli ve vaší aplikaci, vaše aplikace bude muset požádat o další oprávnění od uživatele.  Představujeme z specifikace OpenID Connect – podpora pro dva nové obory `email` a `profile` obory – které umožňují učinit.

* `email` Obor je velmi jednoduchá – umožňuje vám přístup aplikace k primární e-mailovou adresu uživatele prostřednictvím `email` deklarací identity v požadavku id_token.  Všimněte si, že `email` deklarace identity vždycky nebude přítomný v id_tokens – pouze budou zahrnuty Pokud je k dispozici v profilu uživatele.
* `profile` Oboru poskytuje přístup k vaší aplikaci pro všechny ostatní základní informace o uživateli – jejich název, upřednostňované uživatelské jméno, ID objektu a tak dále.

To umožňuje kód aplikace způsobem minimální zpřístupnění – uživatel může požádat o právě sadu informace, které vaše aplikace vyžaduje, aby svou úlohu.  Pokud chcete pokračovat, získávání kompletní informace o uživateli, který aktuálně přijímá aplikace, by měla obsahovat všechny tři obory žádostí autorizace:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Aplikace můžete zahájit odesílání `email` a `profile` obory okamžitě a koncového bodu v2.0 přijmout tyto dva obory a začít vyžadování oprávnění od uživatelů podle potřeby.  Však změnu v hodnotě výklad `openid` oboru se neprojeví pro několik týdnů.

> [!IMPORTANT]
> **Úlohu: Přidat `profile` a `email` rozsahy, pokud vaše aplikace vyžaduje informace o uživateli.**  Všimněte si, že ADAL bude obsahovat obě tato oprávnění v žádostech o ve výchozím nastavení. 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a>Odebrání vystavitele koncové lomítko.
Dříve vystavitele hodnotu, která se zobrazí v tokeny z koncového bodu v2.0 trvalo formuláře

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Kde tenantId služby Azure AD byl identifikátor guid klienta, který vydal token.  Tyto změny se změní na hodnotu vystavitele

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

v obou tokeny a zjišťování dokumentu OpenID Connect.

> [!IMPORTANT]
> **Úlohu: Zajistěte, aby vaše aplikace přijímá vystavitele hodnotu s i bez koncové lomítko během ověřování vystavitele.**
> 
> 

## <a name="why-change"></a>Proč změnit?
Primární motivace pro představení tyto změny se má být kompatibilní se specifikací standardní OpenID Connect.  Díky OpenID Connect kompatibilní Doufáme, minimalizovat rozdíly mezi integrace se službou Microsoft identity services a s jinými službami identity v odvětví.  Chceme umožňují vývojářům používat své oblíbené opensourcové knihovny ověřování bez nutnosti ke změně v knihovnách k přizpůsobení rozdílů společnosti Microsoft.

## <a name="what-can-you-do"></a>Co můžete dělat?
K dnešnímu dni můžete začít vytváření všechny změny popsané výše.  Měli byste okamžitě:

1. **Odeberte všechny závislosti pro `x5t` parametr záhlaví.**
2. **Pohodlné zpracování přechod z `profile_info` k `id_token` v odpovědi tokenu.**
3. **Odeberte všechny závislosti pro `id_token_expires_in` parametr odpovědi.**
4. **Přidat `profile` a `email` rozsahy do vaší aplikace, pokud aplikace potřebuje základní uživatelské informace.**
5. **Přijměte vystavitele hodnoty v tokenech s i bez koncové lomítko.**

Naše [dokumentace k protokolu v2.0](active-directory-v2-protocols.md) se už aktualizovala tak, aby odrážela tyto změny, můžete ho použít jako odkaz v pomáhá aktualizace kódu.

Pokud máte další dotazy na obor změny, kontaktujte oslovení nám na Twitteru v @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Jak často bude protokol změnách?
Není jsme předvídáte, že žádné další nejnovější změny ověřovací protokoly.  Tyto změny do jedné verze jsme jsou sdružování záměrně, takže nebudete muset projít tento typ procesu aktualizace znovu kdykoli brzy.  Samozřejmě budeme nadále přidání funkcí do sblíženého v2.0 ověřovací služba, která můžete využít výhod, ale tyto změny by měl být doplňkové a není zalomení existující kód.

Nakonec bychom rádi vyslovení Děkujeme vám za vyzkoušení věcí během období preview.  Přehledy a zkušenosti naše inovátoři být neocenitelnou pomocí doposud a Věříme, že budete nadále sdílet své názory a návrhy.

Kódování radostí!

Microsoft Identity dělení


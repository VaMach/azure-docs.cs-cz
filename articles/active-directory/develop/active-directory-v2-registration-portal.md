---
title: "Témata nápovědy portálu registrace aplikace | Microsoft Docs"
description: "Popis různých funkcí v portálu pro registraci aplikace společnosti Microsoft."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mtillman
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 3340df3fcc2456a355e523bfcf09978a16966036
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="app-registration-reference"></a>Informace o registraci aplikace
Tento dokument obsahuje kontextu a popisy různé funkce, které se nacházejí v portálu pro registraci aplikace Microsoft [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Moje aplikace
Tento seznam obsahuje všechny aplikace zaregistrovaný pro použití s koncovým bodem v2.0 Azure AD.  Tyto aplikace mají možnost přihlášení uživatelů s oba osobní účty z účtu Microsoft a pracovní nebo školní účty ze služby Azure Active Directory.  Další informace o koncový bod v2.0 Azure AD najdete v tématu naše [v2.0 přehled](active-directory-appmodel-v2-overview.md).  Tyto aplikace lze také integrovat s Microsoft účet ověřování koncového bodu, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK aplikace
Tento seznam obsahuje všechny aplikace zaregistrovaný pro použití výhradně pomocí účtu Microsoft.  Pro použití se službou Azure Active Directory jakkoli není povolen.  Toto je, kde najdete všechny aplikace, které byl dříve registrován u MSA portál pro vývojáře v `https://account.live.com/developers/applications`.  Všechny funkce, které jste provedli dříve v `https://account.live.com/developers/applications` provést nyní tento nový portál `https://apps.dev.microsoft.com`.  Pokud máte další dotazy o aplikace účtu Microsoft, kontaktujte nás.

## <a name="application-secrets"></a>Tajné klíče aplikace
Tajné klíče aplikace jsou přihlašovací údaje, které umožňují aplikace provádět spolehlivé [ověření klienta](http://tools.ietf.org/html/rfc6749#section-2.3) s Azure AD.  V OAuth a OpenID Connect tajné klíče aplikace se obvykle označuje jako `client_secret`.  V protokolu v2.0, všechny aplikace, která přijímá token zabezpečení v umístění adresovatelné webové (pomocí `https` schéma) musíte použít tajný klíč aplikace identifikuje do služby Azure AD při uplatnění tohoto tokenu zabezpečení.  Kromě toho všechny nativní klient této recieves tokeny na zařízení bude zakázáno pomocí tajný klíč aplikace k ověřování klienta k bránit úložiště tajných klíčů v nezabezpečené prostředí.

Každá aplikace může obsahovat dva tajné klíče platnou aplikaci v libovolném bodě v čase.  Udržovat dva tajné klíče, máte ablilty k provádět pravidelné výměna klíče na celé prostředí vaší aplikace.  Po přesunutí celého aplikace na nový sdílený tajný klíč, můžete odstranit starý sdílený tajný klíč a zřízení nového.

V tomto okamžiku jsou povolené jenom dva typy tajné klíče aplikace v portálu pro registraci aplikace.  Výběr **generovat nové heslo** bude generovat a ukládat sdílený tajný klíč v úložišti příslušných dat, který můžete použít v aplikaci.  Výběr **vygenerovat nový pár klíč** vytvoří nový pár veřejného a privátního klíče, je možné stáhnout a použít pro ověřování klientů na Azure AD.

## <a name="profile"></a>Profil
Část profilu portálu pro registraci aplikace lze přizpůsobit přihlašovací stránku pro aplikaci.  V tuto chvíli můžete změnit na přihlašovací stránce aplikace loga, podmínky adresa URL služby a zásady ochrany osobních údajů.  Logo musí být transparentní 48 × 48 nebo 50 × 50 pixelů v souboru GIF, PNG nebo JPEG, který je 15 KB nebo menší.  Zkuste změnit hodnoty a zobrazení výsledné přihlašovací stránky!

## <a name="live-sdk-support"></a>Live SDK podpory
Pokud povolíte "podporu SDK Live", žádné aplikace tajné klíče vytvoříte, se zřídí do služby Azure AD a úložišť dat Account Microsoft.  To vám umožní integrovat službu Microsoft Account (login.live.com) aplikace.  Pokud chcete vytvořit aplikaci pomocí Microsoft Account přímo (na rozdíl od použití koncového bodu v2.0 Azure AD), měli byste si ověřit, že je zapnutá podpora Live SDK.

Zakázání podpory Live SDK zajistí, že tajný klíč aplikace je zapisovat pouze do Azure AD data uložit.  Azure AD data úložiště zapadá regulace podnikové úrovni, umožňujících splnit nějaké standardy, jako je například FISMA dodržování předpisů.  Pokud povolíte podporu Live SDK, vaše aplikace nemusí zajištění dodržování předpisů pomocí některé z těchto standardů.

Pokud plánujete pouze někdy použít koncového bodu v2.0 Azure AD, můžete bezpečně zakázat podporu Live SDK.


---
title: 'Azure Active Directory B2C: Registrace aplikace | Dokumentace Microsoftu'
description: "Postup registrace aplikace pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3499ff57e650c70679dfa018eec5dbe1a6173a33
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrace vaší aplikace

> [!IMPORTANT]
> Aplikace vytvořené z okna Azure AD B2C na webu Azure Portal se musí spravovat ze stejného místa. Pokud upravíte aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a nebudou s Azure AD B2C pracovat. Další informace najdete [níže](#faulted-apps).
>

## <a name="prerequisite"></a>Požadavek

Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Po provedení všech kroků v tomto tématu budete mít okno s funkcemi B2C připnuté na Úvodním panelu.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Přejděte do okna s funkcemi B2C.

Pokud máte okno s funkcemi B2C připnuté na Úvodním panelu, okno se zobrazí na [portálu Azure](https://portal.azure.com/) ihned po přihlášení jako Globální správce klienta B2C.

Přejít do okna je možné také kliknutím na **Další služby** a poté vyhledáním položky **Azure AD B2C** v levém navigačním podokně na webu [Azure Portal](https://portal.azure.com/).

> [!IMPORTANT]
> Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C. Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.  Můžete přepnout na svého klienta B2C pomocí přepínače klienta v pravém horním rohu webu Azure Portal.
>
>

## <a name="register-a-web-application"></a>Registrace webové aplikace

1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
1. Klikněte na **Přidat** v horní části okna.
1. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
1. Přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**.
1. Zadejte [správnou](#limitations) hodnotu pro **Adresy URL odpovědí**, což jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44316/`.
1. Pro registraci aplikace klikněte na **Vytvořit**.
1. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.
1. Pokud bude vaše webová aplikace také volat webové rozhraní API zabezpečené pomocí Azure AD B2C, bude nutné:
    1. Vytvořit **Tajný klíč aplikace** – přejděte do okna **Klíče** a klikněte na tlačítko **Vygenerovat klíč**.
    1. Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
>

## <a name="register-a-web-api"></a>Registrace webové rozhraní API

1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
1. Klikněte na **Přidat** v horní části okna.
1. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „API Contoso B2C“.
1. Přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**.
1. Zadejte [správnou](#choosing-a-web-app/api-reply-url) hodnotu pro **Adresy URL odpovědí**, což jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44316/`.
1. Zadejte **Identifikátor URI ID aplikace**. To je identifikátor použitý pro vaše webového rozhraní API. Zadejte například „notes“. Vygeneruje se úplný identifikátor URI.
1. Pro registraci aplikace klikněte na **Vytvořit**.
1. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.
1. Klikněte na **Publikované obory**. Tady můžete definovat oprávnění (obory), která lze udělit ostatním aplikacím.
1. V případě potřeby přidejte další obory. Ve výchozím nastavení bude definován obor „user_impersonation“. To umožňuje jiným aplikacím přístup k tomuto rozhraní API jménem přihlášeného uživatele. Pokud chcete, můžete ho odebrat.
1. Klikněte na **Uložit**.

## <a name="register-a-mobilenative-application"></a>Registrace mobilní/nativní aplikace

1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
1. Klikněte na **Přidat** v horní části okna.
1. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
1. Přepněte přepínač **Zahrnout nativního klienta** na **Ano**.
1. Zadejte **Identifikátor URI přesměrování** s vlastním schématem. Například com.onmicrosoft.contoso.appname://redirect/path. Nezapomeňte zvolit [dobrý identifikátor URI přesměrování](#choosing-a-native-application-redirect-uri) a nepoužívat speciální znaky jako podtržítka.
1. Aby se aplikace registrovala, klikněte na **Uložit**.
1. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.
1. Pokud bude vaše nativní aplikace také volat webové rozhraní API zabezpečené pomocí Azure AD B2C, bude nutné:
    1. Vytvořit **Tajný klíč aplikace** – přejděte do okna **Klíče** a klikněte na tlačítko **Vygenerovat klíč**.
    1. Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
>

## <a name="limitations"></a>Omezení

### <a name="choosing-a-web-appapi-reply-url"></a>Výběr adresy URL odpovědi webové aplikace nebo rozhraní API

Aktuálně je u aplikací zaregistrovaných pomocí Azure AD B2C omezená sada hodnot adresy URL odpovědi. Adresa URL odpovědi pro webové aplikace a služby musí začínat schématem `https` a všechny adresy URL odpovědi musí sdílet jednu doménu DNS. Například nemůžete zaregistrovat webovou aplikaci s některou z těchto adres URL odpovědi:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Registrační systém porovnává celý název DNS stávající adresy URL odpovědi s názvem DNS adresy URL odpovědi, kterou přidáváte. Požadavek na přidání názvu DNS selže, pokud platí některá z následujících podmínek:

* Celý název DNS nové adresy URL odpovědi neodpovídá názvu DNS stávající adresy URL odpovědi.
* Celý název DNS nové adresy URL odpovědi není subdoménou stávající adresy URL odpovědi.

Pokud má aplikace například tuto adresu URL odpovědi:

`https://login.contoso.com`

Můžete ji přidat tímto způsobem:

`https://login.contoso.com/new`

V tomto případě se název DNS přesně shoduje. Nebo můžete provést toto:

`https://new.login.contoso.com`

V tomto případě odkazujete na subdoménu DNS login.contoso.com. Pokud chcete mít aplikaci s adresami URL odpovědi login-east.contoso.com a login-west.contoso.com, musíte tyto adresy URL odpovědi přidat v následujícím pořadí:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Druhé dvě adresy URL odpovědi můžete přidat, protože jsou subdoménami první adresy URL odpovědi contoso.com.

### <a name="choosing-a-native-application-redirect-uri"></a>Výběr identifikátoru URI přesměrování nativní aplikace

Existují dva důležité aspekty při výběru identifikátoru URI přesměrování pro mobilní/nativní aplikace:

* **Jedinečnost:** Schéma identifikátoru URI přesměrování by mělo být pro každou aplikaci jedinečné. V našem příkladu (com.onmicrosoft.contoso.appname://redirect/path) použijeme jako schéma com.onmicrosoft.contoso.appname. Doporučujeme používat tento vzor. Pokud dvě aplikace sdílejí stejné schéma, uživateli se zobrazí dialogové okno pro výběr aplikace. Pokud uživatel použije nesprávnou volbu, přihlášení se nezdaří.
* **Úplnost:** Identifikátor URI přesměrování musí mít schéma a cestu. Cesta musí obsahovat za doménou alespoň jedno lomítko (například //contoso/ bude fungovat a //contoso selže).

Ujistěte se, že identifikátor URI přesměrování neobsahuje žádné speciální znaky jako podtržítka.

### <a name="faulted-apps"></a>Chybné aplikace

Aplikace B2C se NESMÍ upravovat:

* Na jiných portálech pro správu aplikací, jako je [portál Azure Classic](https://manage.windowsazure.com/) a [Portál pro registraci aplikací](https://apps.dev.microsoft.com/).
* Pomocí rozhraní Graph API nebo PowerShellu.

Pokud aplikaci B2C upravíte výše popsaným způsobem a pokusíte se ji znovu upravit v okně funkcí Azure AD B2C na webu Azure Portal, stane se chybnou aplikací a už ji nebude možné použít s Azure AD B2C. Budete muset aplikaci odstranit a znovu ji vytvořit.

Pokud chcete aplikaci odstranit, přejděte na [Portál pro registraci aplikací](https://apps.dev.microsoft.com/) a tam ji odstraňte. Aby byla aplikace viditelná, musíte být vlastníkem aplikace (nestačí být pouze správcem tenanta).

## <a name="next-steps"></a>Další kroky

Nyní, když máte aplikaci registrovanou pomocí Azure AD B2C, můžete dokončit jeden z [našich kurzů pro rychlý start](active-directory-b2c-overview.md#get-started) pro uvedení do provozu.


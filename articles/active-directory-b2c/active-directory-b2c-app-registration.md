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
ms.date: 3/13/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: e04fbd97dd4d5ecaf12edf47d80572b32d29ed00
ms.lasthandoff: 03/23/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrace vaší aplikace

> [!IMPORTANT]
> Aplikace vytvořené z okna Azure AD B2C na webu Azure Portal se musí spravovat ze stejného místa. Pokud upravíte aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a pravděpodobně nebudou s Azure AD B2C pracovat.
> 
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
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
4. Přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**. **Adresy URL odpovědí** jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44316/`.
5. Pro registraci aplikace klikněte na **Vytvořit**.
6. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu. 
7. Pokud vaše webová aplikace také bude volat webové rozhraní API zabezpečené pomocí Azure AD B2C, budete chtít vytvořit **Tajný klíč aplikace**, a to v okně **Klíče** kliknutím na tlačítko **Vygenerovat klíč**.

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
> 
   

## <a name="register-a-web-api"></a>Registrace webové rozhraní API
1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „API Contoso B2C“.
4. Přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**. **Adresy URL odpovědí** jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44316/`.
5. Zadejte **Identifikátor URI ID aplikace**. To je identifikátor použitý pro vaše webového rozhraní API. Zadejte například „notes“. Vygeneruje se úplný identifikátor URI. 
6. Pro registraci aplikace klikněte na **Vytvořit**.
7. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.
8. Klikněte na **Publikované obory**. Tady můžete definovat oprávnění (obory), která lze udělit ostatním aplikacím.
9. V případě potřeby přidejte další obory. Ve výchozím nastavení bude definován obor „user_impersonation“. To umožňuje jiným aplikacím přístup k tomuto rozhraní API jménem přihlášeného uživatele. Pokud chcete, můžete ho odebrat. 
10. Klikněte na **Uložit**.

## <a name="register-a-mobilenative-application"></a>Registrace mobilní/nativní aplikace
1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
4. Přepněte přepínač **Zahrnout nativního klienta** na **Ano**.
5. Zadejte **Identifikátor URI přesměrování** s vlastním schématem. Například com.onmicrosoft.contoso.appname://redirect/path. Zajistěte, abyste zvolili [správný identifikátor URI přesměrování](#choosing-a-redirect-uri).
6. Aby se aplikace registrovala, klikněte na **Uložit**.
7. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.
8. Pokud vaše nativní aplikace také bude volat webové rozhraní API zabezpečené pomocí Azure AD B2C, budete chtít vytvořit **Tajný klíč aplikace**, a to v okně **Klíče** kliknutím na tlačítko **Vygenerovat klíč**.

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
> 

### <a name="choosing-a-redirect-uri"></a>Výběr identifikátoru URI přesměrování
Existují dva důležité aspekty při výběru identifikátoru URI přesměrování pro mobilní/nativní aplikace: 
* **Jedinečnost:** Schéma identifikátoru URI přesměrování by mělo být pro každou aplikaci jedinečné. V našem příkladu (com.onmicrosoft.contoso.appname://redirect/path) použijeme jako schéma com.onmicrosoft.contoso.appname. Doporučujeme používat tento vzor. Pokud dvě aplikace sdílejí stejné schéma, uživateli se zobrazí dialogové okno pro výběr aplikace. Pokud uživatel použije nesprávnou volbu, přihlášení se nezdaří. 
* **Úplnost:** Identifikátor URI přesměrování musí mít schéma a cestu. Cesta musí obsahovat za doménou alespoň jedno lomítko (například //contoso/ bude fungovat a //contoso selže). 

## <a name="build-a-quick-start-application"></a>Sestavení aplikace Rychlý start
Nyní, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z našich kurzů pro rychlý start pro uvedení do provozu. Zde je několik doporučení:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



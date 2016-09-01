<properties
    pageTitle="Azure Active Directory B2C: Registrace aplikace | Microsoft Azure"
    description="Postup registrace aplikace pomocí Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>


# Azure Active Directory B2C: Registrace vaší aplikace

## Požadavek

Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Po provedení všech kroků v tomto tématu budete mít okno s funkcemi B2C připnuté na Úvodním panelu.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Přejděte do okna s funkcemi B2C.

Pokud máte okno s funkcemi B2C připnuté na Úvodním panelu, okno se zobrazí na [portálu Azure](https://portal.azure.com/) ihned po přihlášení jako Globální správce klienta B2C.

Přejít do okna je možné také kliknutím na **Procházet** a poté na **Azure AD B2C** v levém navigačním podokně na [portálu Azure](https://portal.azure.com/).

> [AZURE.IMPORTANT] Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C. Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.  Můžete přepnout na svého klienta B2C pomocí přepínače klienta v pravém horním rohu portálu Azure.

## Registrace aplikace

1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
4. Pokud píšete webovou aplikaci, přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**. **Adresy URL odpovědí** jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44321/`. Pokud vaše webová aplikace také bude volat některé webové rozhraní API zabezpečené pomocí Azure AD B2C, budete chtít vytvořit **Tajný klíč aplikace** a také kliknout na tlačítko **Vygenerovat klíč**.
5. Pokud píšete mobilní aplikaci, přepněte přepínač **Zahrnout nativního klienta** na **Ano**. Poznamenejte si výchozí **Identifikátor URI přesměrování**, který se automaticky vytvoří.
6. Pro registraci aplikace klikněte na **Vytvořit**.
7. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.

> [AZURE.NOTE] **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.

## Sestavení aplikace Rychlý start

Nyní, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z našich kurzů pro rychlý start pro uvedení do provozu. Zde je několik doporučení:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!--HONumber=Aug16_HO4-->



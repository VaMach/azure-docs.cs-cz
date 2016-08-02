<properties
    pageTitle="Azure Active Directory B2C ve verzi Preview: Registrace aplikace | Microsoft Azure"
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
    ms.date="05/16/2016"
    ms.author="swkrish"/>


# Azure Active Directory B2C ve verzi Preview: Registrace vaší aplikace

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Požadavek

Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Po provedení všech kroků v tomto tématu budete mít okno s funkcemi B2C připnuté na Úvodním panelu.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Přejděte do okna s funkcemi B2C.

Přejít do okna s funkcemi B2C můžete z portálu Azure nebo z portálu Azure Classic.

### 1. Přístup z portálu Azure

Pokud máte okno s funkcemi B2C připnuté na Úvodním panelu, okno se zobrazí na [portálu Azure](https://portal.azure.com/) ihned po přihlášení jako Globální správce klienta B2C.

Přejít do okna je možné také kliknutím na **Procházet** a poté na **Azure AD B2C** v levém navigačním podokně na [portálu Azure](https://portal.azure.com/).

Přejít do okna je možné také přímo pomocí odkazu [https://portal.azure.com/ {klient}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/](https://portal.azure.com/{tenant}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/), kde nahraďte **{klient}** názvem zvoleným při vytvoření klienta (například contosob2c). Tento odkaz si můžete uložit do oblíbených pro pozdější použití.

    > [AZURE.IMPORTANT]
    Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C. Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.

### 2. Přístup z portálu Azure Classic

Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) jako Správce předplatného. (Toto je stejný pracovní nebo školní účet, nebo stejný účet Microsoft, který jste použili při registraci k Azure.) Přejděte na rozšíření Active Directory na levé straně a klikněte na svého klienta B2C. Na kartě **Rychlý start** (první karta, která se otevře) klikněte na **Správa nastavení B2C** v sekci **Správa**. Okno s funkcemi B2C se otevře v novém okně nebo záložce prohlížeče.

Odkaz **Správa nastavení B2C** najdete také v oddílu **Správa B2C** na kartě **Konfigurovat**.

## Registrace aplikace

1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
4. Pokud píšete webovou aplikaci, přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**. **Adresy URL odpovědí** jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44321/`. Pokud vaše aplikace obsahuje serverovou komponentu (rozhraní API), která musí být zabezpečená, budete také chtít vytvořit (a poznamenat si) **Tajný klíč aplikace** kliknutím na tlačítko **Generovat klíč**.

    > [AZURE.NOTE]
    **Tajný klíč aplikace** je důležitým bezpečnostním pověřením.

5. Pokud píšete mobilní aplikaci, přepněte přepínač **Zahrnout nativního klienta** na **Ano**. Poznamenejte si výchozí **Identifikátor URI přesměrování**, který se automaticky vytvoří.
6. Pro registraci aplikace klikněte na **Vytvořit**.
7. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.

## Sestavení aplikace Rychlý start

Nyní, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z našich kurzů pro rychlý start pro uvedení do provozu. Zde je několik doporučení:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!--HONumber=Jun16_HO2-->



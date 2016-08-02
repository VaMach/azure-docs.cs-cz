<properties
    pageTitle="Azure Active Directory B2C ve verzi Preview: Vytvoření klienta Azure Active Directory B2C | Microsoft Azure"
    description="Téma o tom, jak vytvořit klienta Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="swkrish"/>

# Azure Active Directory B2C ve verzi Preview: Vytvoření klienta Azure AD B2C

Chcete-li začít používat Microsoft Azure Active Directory (Azure AD) B2C, postupujte podle tří kroků uvedených v tomto článku.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Krok 1: Zaregistrujte si předplatné Azure

Pokud již máte předplatné Azure, tento krok přeskočte. Pokud ne, zaregistrujte si [předplatné Azure](../active-directory/sign-up-organization.md) a získejte přístup k Azure AD B2C.

> [AZURE.NOTE]
V současné době můžete použít Azure AD B2C ve verzi Preview zdarma, ale jeho použití je omezeno na 50 000 uživatelů na klienta. Předplatné Azure je nutné pro přístup k [portálu Azure Classic](http://manage.windowsazure.com/).

## Krok 2: Vytvořte klienta Azure AD B2C

Pomocí následujících kroků vytvořte nového klienta Azure AD B2C. V současné době nelze zapnout funkce B2C v existujících adresářích, pokud už nějaké máte.

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) jako Správce předplatného. Toto je stejný pracovní nebo školní účet, nebo stejný účet Microsoft, který jste použili při registraci k Azure.
2. Klikněte na tlačítko **Nové** > **App Services** > **Active Directory** > **Adresář** > **Vytvořit vlastní**.

    ![Snímek obrazovky začátku tvorby klienta](./media/active-directory-b2c-get-started/new-directory.png)

3. Zvolte **Název**, **Název domény** a **Zemi nebo oblast** svého klienta.
4. Zaškrtněte možnost **Toto je adresář B2C**.
5. Pro dokončení akce klikněte na značku zaškrtnutí.

    ![Snímek obrazovky se značkou zaškrtnutí pro vytvoření adresáře B2C](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Váš klient je nyní vytvořen a zobrazí se v rozšíření Active Directory. Zároveň se stanete globálním správcem klienta. Podle potřeby můžete přidat další globální správce.

## Krok 3: Na portálu Azure přejděte do okna s funkcemi B2C

1. Přejděte na rozšíření Active Directory v navigačním panelu na levé straně.
2. Vyberte svého klienta na kartě **Adresář** a klikněte na něj.
3. Klikněte na kartu **Konfigurovat**.
4. V oddílu **Správa B2C** klikněte na odkaz **Správa nastavení B2C**.

    ![Snímek obrazovky s konfigurací adresáře pro B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. V novém okně nebo kartě prohlížeče se otevře portál Azure se zobrazeným oknem s funkcemi B2C.

    > [AZURE.IMPORTANT]
    Klientu může trvat 2-3 minuty, než bude přístupný na portálu Azure. Opravu provedete opakováním těchto kroků po nějaké době. Pokud ne, kontaktujte prosím Podporu.

6. Připnete toto okno na svůj Úvodní panel pro usnadnění přístupu. (Nástroj připnutí je označen červeně v pravém horním rohu okna s funkcemi)

    ![Snímek obrazovky s oknem s funkcemi B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Na [portálu Azure Classic](https://manage.windowsazure.com/) můžete spravovat uživatele a skupiny, nastavení samoobslužného obnovení hesla a funkce značky společnosti v klientu.

## Další kroky

Zjistěte, jak zaregistrovat aplikaci do Azure AD B2C a jak sestavit aplikaci Rychlý start v tématu [Azure Active Directory B2C ve verzi Preview: Registrace vaší aplikace](active-directory-b2c-app-registration.md).



<!--HONumber=Jun16_HO2-->



<properties
    pageTitle="Přidání vlastního názvu domény do Azure Active Directory | Microsoft Azure"
    description="Postup přidání firemních názvů domén do Azure Active Directory a způsob ověření názvu domény."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="curtand;jeffsta"/>

# Přidání vlastního názvu domény do Azure Active Directory

Vlastníte jeden nebo několik názvů domén, které vaše organizace používá ke své obchodní činnosti, a vaši uživatelé se do podnikové sítě přihlašují pomocí firemního názvu domény. Teď, když používáte Azure Active Directory (Azure AD), můžete do Azure AD přidat i název vaší firemní domény. V adresáři díky tomu můžete přiřazovat uživatelská jména, které jsou pro uživatele srozumitelná, například „alice@contoso.com“. Proces je jednoduchý:

- V průvodci **přidáním domény** na portálu Azure Classic přidejte název domény.

- Z portálu Azure AD Classic nebo z nástroje Azure AD Connect získejte položku DNS.

- Přidejte položku DNS pro název domény do souboru zóny DNS na webu registrátora DNS.

- Ověření názvu domény na portálu Azure AD Classic nebo v nástroji Azure AD Connect


Dokud neověříte vlastní název domény, musí se uživatelé přihlašovat pomocí uživatelských jmen typu „alice@contoso.onmicrosoft.com“, které pro adresář používají váš název domény jen jako začátek doménové části. Pokud potřebujete více vlastních názvů domén, například „contoso.com“ a „contosobank.com“, můžete přidat všechny (až do maximálního počtu 900 názvů domén). K přidání jednotlivých názvů domén použijte stejné kroky popsané v tomto článku.

## Přidání vlastního názvu domény do adresáře

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.

2. V **Active Directory** otevřete adresář a vyberte kartu **Domény**.

3. Na panelu příkazů vyberte **Přidat** a potom zadejte název vlastní domény, například „contoso.com“. Nezapomeňte napsat i příponu .com, .net nebo jinou příponu nejvyšší úrovně.

4. Pokud máte v úmyslu konfigurovat tuto doménu pro [federované přihlašování](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) pomocí místní služby Active Directory, zaškrtněte příslušné políčko.

5. Vyberte **Přidat**.

Teď, když jste název domény přidali, musí Azure AD ověřit, jestli ho vaše organizace vlastní. Aby Azure AD mohl toto ověření provést, musíte přidat položku DNS do souboru zóny DNS pro název domény. To se provádí na webu registrátora názvu domény.

## Získání položek DNS pro název domény

Pokud nepoužíváte federování s místní službou službě Windows Server Active Directory, budou položky DNS na druhé stránce průvodce **přidáním domény**.

Pokud doménu konfigurujete pro federování, budete přesměrováni na stažení nástroje Azure AD Connect. Spusťte nástroj Azure AD Connect a [získejte položky DNS, které potřebujete přidat u registrátora názvu domény](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Nástroj Azure AD Connect současně i ověří název domény pro Azure AD.

## Přidání položky DNS do souboru zóny DNS

1.  Přihlaste se k registrátorovi názvu domény. Pokud nemáte dostatečná oprávnění k aktualizaci položky DNS, požádejte osobu nebo tým s potřebným oprávněním o přidání položky DNS.

2.  Aktualizujte soubor zóny DNS pro doménu tím, že přidáte položku DNS, kterou jste získali od Azure AD. Tato položka DNS umožňuje službě Azure AD ověřit vlastnictví domény. Položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting. Může trvat až jednu hodinu, než se dokončí propagace záznamů DNS.

[Pokyny k přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Ověření názvu domény pomocí služby Azure AD

Po přidání položky DNS je nutné zajistit, že Azure AD ověří název domény. To je poslední krok k úspěšnému dokončení procesu.

Pokud máte ještě otevřeného průvodce **přidáním domény**, vyberte na jeho třetí straně možnost **Ověřit**. Před ověřováním doporučujeme aspoň hodinu počkat na dokončení propagace položky DNS.

Pokud už je průvodce **přidáním domény** zavřený, můžete doménu ověřit na [portálu Azure Classic](https://manage.windowsazure.com/):

1.  Přihlaste se pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.

2.  Otevřete adresář a vyberte kartu **Domény**.

3.  Vyberte doménu, kterou chcete ověřit.

4.  Na panelu příkazů vyberte **Ověřit** a potom v dialogovém okně vyberte **Ověřit**.

Blahopřejeme k úspěchu! Teď můžete [přiřazovat uživatelská jména, která obsahují vlastní název domény](active-directory-add-domain-add-users.md). Pokud jste měli při ověřování názvu domény jakékoli potíže, přečtěte si část [Řešení potíží](#troubleshooting).

## Řešení potíží
Pokud nemůžete ověřit vlastní název domény, může to mít několik příčin. Začneme těmi nejběžnějšími a budeme postupovat až k těm nejméně častým.

- Pokusili jste se název domény ověřit dřív, než se položka DNS stihla rozšířit (dokončila se propagace). Chvíli počkejte a zkuste to znovu.

- Vůbec nebyl zadán záznam DNS. Ověřte položku DNS, počkejte na dokončení propagace a potom to zkuste znovu.

- Název domény už je ověřený v jiném adresáři. Najděte název domény, odstraňte ho z jiného adresáře a zkuste to znovu.

- Záznam DNS obsahuje chybu. Opravte chybu a zkuste to znovu.

- Nemáte dostatečná oprávnění k aktualizaci záznamů DNS. Pošlete položky DNS osobě nebo týmu ve vaší organizaci, kteří tento přístup mají, a požádejte je o přidání položky DNS.


## Další kroky

-   [Přiřazení uživatelských jmen, která obsahují vlastní název domény](active-directory-add-domain-add-users.md)
-   [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
-   [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)
-   [Zobrazení firemního brandingu během přihlašování uživatelů](active-directory-add-company-branding.md)
-   [Používání PowerShellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!--HONumber=Jun16_HO2-->



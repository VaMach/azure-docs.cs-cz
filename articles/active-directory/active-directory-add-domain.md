<properties
    pageTitle="Přidání vlastního názvu domény do Azure Active Directory | Microsoft Azure"
    description="Postup přidání firemních názvů domén do Azure Active Directory a způsob ověření názvu domény."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/18/2016"
    ms.author="curtand;jeffsta"/>

# Přidání vlastního názvu domény do Azure Active Directory

Vlastníte jeden nebo několik názvů domén, které vaše organizace používá ke své obchodní činnosti, a vaši uživatelé se do podnikové sítě přihlašují pomocí firemního názvu domény. Teď, když používáte Azure Active Directory (Azure AD), můžete do Azure AD přidat i název vaší firemní domény. V adresáři díky tomu můžete přiřazovat uživatelská jména, které jsou pro uživatele srozumitelná, například „alice@contoso.com“. Proces je jednoduchý:

1. Přidání vlastního názvu domény do adresáře
2. Přidání položky DNS pro název domény u registrátora názvu domény
3. Ověření vlastního názvu domény v Azure AD

> [AZURE.NOTE] Pokud plánujete nakonfigurovat vlastní název domény pro použití se službou Active Directory Federation Services (AD FS) nebo jinou službou tokenů zabezpečení (STS) ve vaší podnikové síti, postupujte podle pokynů v tématu [Přidání a konfigurace domény pro federování se službou Azure Active Directory](active-directory-add-domain-federated.md). Je to užitečné, pokud plánujete synchronizovat uživatele z podnikového adresáře se službou Azure AD a [synchronizace hodnot hash hesel](active-directory-aadconnectsync-implement-password-synchronization.md) nesplňuje vaše požadavky.

## Přidání vlastního názvu domény do adresáře

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.

2. V **Active Directory** otevřete adresář a vyberte kartu **Domény**.

3. Na panelu příkazů vyberte **Přidat**. Zadejte název vlastní domény, třeba contoso.com. Nezapomeňte uvést příponu (.com, .net nebo jinou příponu nejvyšší úrovně) a ponechat políčko pro jednotné přihlašování (federaci) nezaškrtnuté.

4. Vyberte **Přidat**.

5. Na druhé stránce průvodce Přidat doménu získejte položku DNS, kterou Azure AD použije k ověření, že vaše organizace je vlastníkem příslušného vlastního názvu domény.

Teď, když jste název domény přidali, musí Azure AD ověřit, jestli ho vaše organizace vlastní. Aby Azure AD mohl toto ověření provést, musíte přidat položku DNS do souboru zóny DNS pro název domény. To se provádí na webu registrátora názvu domény.

## Přidání položky DNS pro doménu u registrátora názvu domény

Dalším krokem k použití vlastního názvu domény ve službě Azure AD je aktualizace souboru zóny DNS pro příslušnou doménu. Tím povolíte, aby služba Azure AD ověřila, že vaše organizace je vlastníkem vlastního názvu domény.

1.  Přihlaste se k registrátorovi názvu domény. Pokud k aktualizaci položky DNS nemáte přístup, požádejte osobu nebo tým, kteří přístup mají, aby dokončili krok 2 a dali vám vědět, až bude hotový.

2.  Aktualizujte soubor zóny DNS pro doménu tím, že přidáte položku DNS, kterou jste získali od Azure AD. Tato položka DNS umožňuje službě Azure AD ověřit vlastnictví domény. Položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k přidání položky DNS najdete v článku [Pokyny k přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Ověření názvu domény pomocí služby Azure AD

Po přidání položky DNS jste připravení ověřit název domény pomocí Azure AD.

Pokud máte ještě otevřeného průvodce **přidáním domény**, vyberte na jeho třetí straně možnost **Ověřit**. Když vyberete **Ověřit**, Azure AD vyhledá položku DNS domény v souboru zóny DNS. Služba Azure AD může název domény ověřit až po rozšíření položek DNS. Šíření často trvá jen několik sekund, ale občas může zabrat i hodinu nebo déle. Pokud ověření na první pokus nefunguje, zkuste to později.

Pokud už je průvodce **přidáním domény** zavřený, můžete doménu ověřit na [portálu Azure Classic](https://manage.windowsazure.com/):

1.  Přihlaste se pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.

2.  Otevřete adresář a vyberte kartu **Domény**.

3.  Vyberte název domény, který chcete ověřit, a na panelu příkazů vyberte **Ověřit**.

4. K dokončení ověřování v tomto dialogovém okně vyberte **Ověřit**.

Teď můžete [přiřazovat uživatelská jména, která obsahují vlastní název domény](active-directory-add-domain-add-users.md).

## Řešení potíží

Pokud nemůžete vlastní název domény ověřit, zkuste následující postup. Začneme těmi nejběžnějšími a budeme postupovat až k těm nejméně častým.

1.  **Počkejte hodinu**. Položky DNS se musí nejprve rozšířit a teprve potom může služba Azure AD doménu ověřit. Může to trvat hodinu i déle.

2.  **Zkontrolujte, jestli záznam DNS byl zadaný a jestli je správně**. Tento krok proveďte na webu registrátora názvu domény. Azure AD nemůže ověřit název domény, pokud záznam DNS není uložený v souboru zóny DNS nebo pokud se položka DNS přesně neshoduje s položkou, kterou vám poskytla služba Azure AD. Pokud nemáte přístup k aktualizaci záznamů DNS domény u registrátora názvu domény, poskytněte položku DNS osobě nebo týmu ve vaší organizaci, kteří tento přístup mají, a požádejte, aby položku DNS přidali.

3.  **Odstraňte název domény z jiného adresáře ve službě Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. Další informace o odstraňování názvů domén najdete v článku [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md).


## Přidání dalších vlastních názvů domén

Pokud vaše organizace používá několik vlastních názvů domén, například contoso.com a contosobank.com, můžete je přidat všechny (až do maximálního počtu 900 názvů domén). Všechny názvy domén můžete přidat opakováním postupu popsaného v tomto článku.

## Další kroky

-   [Přiřazení uživatelských jmen, která obsahují vlastní název domény](active-directory-add-domain-add-users.md)
-   [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
-   [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)
-   [Zobrazení firemního brandingu během přihlašování uživatelů](active-directory-add-company-branding.md)
-   [Používání PowerShellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!---HONumber=Aug16_HO4-->



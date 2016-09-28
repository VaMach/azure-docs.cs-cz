<properties
    pageTitle="Přidání vlastního názvu domény a nastavení federovaného přihlašování ve službě Azure Active Directory | Microsoft Azure"
    description="Postup přidání názvů domén vaší společnosti do Azure Active Directory a postup nastavení federovaného přihlašování mezi Azure Active Directory a místním federačním řešením."
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

Můžete si nakonfigurovat vlastní název domény, například contoso.com, aby uživatelé webu contoso.com mohli využívat jednotné federované přihlašování z podnikové sítě. Pokud už využíváte službu AD FS (Active Directory Federation Services) nebo jiný federační server spuštěný v podnikové síti, můžete službu Azure AD nakonfigurovat pomocí nástroje Azure AD Connect tak, aby používala vlastní název domény. Azure AD Connect můžete použít také k nasazení nového prostředí služby AD FS a k jeho konfiguraci pro jednotné federované přihlašování do Azure AD.

Pokud nevyužíváte a neplánujete nasazení služby AD FS nebo jiného federačního serveru, postupujte podle následujících pokynů: [Přidání vlastního názvu domény do Azure Active Directory](active-directory-add-domain.md).

## Přidání vlastního názvu domény do adresáře

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.

2. V **Active Directory** otevřete adresář a vyberte kartu **Domény**.

3. Na panelu příkazů vyberte **Přidat** a potom zadejte název vlastní domény, například „contoso.com“. Nezapomeňte napsat i příponu .com, .net nebo jinou příponu nejvyšší úrovně.

4. Zaškrtněte políčko **Plánuji konfiguraci této domény pro jednotné přihlašování pomocí místní služby Active Directory**.

5. Vyberte **Přidat**.

Spusťte nástroj Azure AD Connect a získejte položku DNS, kterou služba Azure AD použije k ověření domény. Položku DNS uvidíte v průvodci v kroku **Doména služby Azure AD**. [V těchto pokynech](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) uvidíte, jak daný krok v průvodci vypadá. Pokud nástroj Azure AD Connect nemáte, můžete [ho stáhnout tady](http://go.microsoft.com/fwlink/?LinkId=615771).

## Přidání položky DNS u registrátora názvu domény

Pokud chcete používat vlastní název domény ve službě Azure AD, dalším krokem je aktualizace souboru zóny DNS pro takovou doménu. Tím povolíte, aby Azure AD ověřila, že vaše organizace je vlastníkem vlastního názvu domény.

1. Přihlaste se na web registrátora názvu domény. Pokud k této činnosti nemáte přístup, požádejte osobu nebo tým ve vaší organizaci, kteří přístup mají, aby dokončili krok 2 a dali vám vědět, až bude krok hotový.

2. Aktualizujte soubor zóny DNS pro doménu tím, že přidáte položku DNS, kterou jste získali od Azure AD. Tato položka DNS umožňuje službě Azure AD ověřit vlastnictví domény. Položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k tomuto kroku najdete v článku [Pokyny k přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Ověření názvu domény pomocí služby Azure AD

Po přidání položky DNS jste připraveni na ověření názvu domény pomocí Azure AD.

Pokud chcete doménu ověřit, vyberte v průvodci službou Azure AD Connect v kroku **Doména služby Azure AD** možnost **Další**. Azure AD vyhledá položku DNS domény v souboru zóny DNS. Služba Azure AD název domény ověří až po rozšíření záznamů DNS. Šíření často trvá jen několik sekund, ale občas může zabrat hodinu nebo déle. Pokud ověření na první pokus nefunguje, zkuste to později.

Potom pokračujte se zbývající kroky průvodce službou Azure AD Connect. Tímto způsobem provedete synchronizaci uživatelů mezi službou AD na Windows Serveru a službou Azure AD. Synchronizovaní uživatelé v doméně, kterou jste nakonfigurovali pro federaci, budou moct používat jednotné federované přihlašování z podnikové sítě do služby Azure AD.

## Řešení potíží

Pokud nemůžete vlastní název domény ověřit, zkuste následující postup. Začneme těmi nejběžnějšími a budeme postupovat až k těm nejméně častým.

1.  **Počkejte hodinu**. Záznamy DNS se musí nejprve rozšířit a teprve potom může služba Azure AD doménu ověřit. Může to trvat hodinu i déle.

2.  **Zkontrolujte, jestli je zadán záznam DNS a jestli je správně**. Tento krok proveďte na webu registrátora názvu domény. Azure AD nemůže ověřit název domény, pokud není záznam DNS k dispozici v souboru zóny DNS, nebo pokud se položka DNS přesně neshoduje s položkou, kterou vám poskytla služba Azure AD. Pokud nemáte přístup k aktualizaci záznamů DNS domény u registrátora názvu domény, poskytněte položku DNS osobě nebo týmu z vaší organizace, kteří tento přístup mají, a požádejte je, aby položku DNS přidali.

3.  **Odstraňte název domény z jiného adresáře ve službě Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. Další informace o odstraňování názvů domén najdete v článku [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md).

## Přidání dalších vlastních názvů domén

Pokud vaše organizace používá několik vlastních názvů domén, například contoso.com a contosobank.com, můžete je přidat všechny (až do maximálního počtu 900 názvů domén). Všechny názvy domén můžete přidat opakováním postupu popsaného v tomto článku.

## Další kroky

-   [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
-   [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)
-   [Zobrazení firemního brandingu během přihlašování uživatelů](active-directory-add-company-branding.md)
-   [Používání PowerShellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!--HONumber=Sep16_HO3-->



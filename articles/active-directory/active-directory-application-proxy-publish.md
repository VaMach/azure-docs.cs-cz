<properties
    pageTitle="Publikování aplikací pomocí proxy aplikace služby Azure AD | Microsoft Azure"
    description="Publikujte místní aplikace v cloudu pomocí proxy aplikace služby Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>


# Publikování aplikací pomocí proxy aplikace služby Azure AD


Jakmile povolíte proxy aplikace služby Microsoft Azure Active Directory (AD), můžete publikovat místní aplikace, aby se k nim mohli vzdálení uživatelé připojovat i mimo privátní síť.

Tento článek vám ukáže postup, jak publikovat aplikace, které běží v místní síti, a jak poskytnout zabezpečený vzdálený přístup mimo síť. Pokud jste ještě nenastavili proxy aplikace nebo nenainstalovali žádné konektory, postupujte podle kroků v článku o [povolení proxy aplikace v portálu Azure](active-directory-application-proxy-enable.md). Teprve pak se vraťte zpět k tomuto článku.

Pokud proxy aplikace služby Azure AD používáte poprvé, doporučujeme, abyste před publikováním aplikací otestovali konektor tím, že publikujete webovou stránku z vaší privátní sítě.

> [AZURE.NOTE] Proxy aplikace je funkce, která je dostupná jenom v případě, pokud jste upgradovali na edici Premium nebo Basic služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

## Publikování aplikace pomocí průvodce

1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com/).
2. Přejděte do služby Active Directory a vyberte adresář, u kterého jste povolili proxy aplikace.

    ![Active Directory – ikona](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klikněte na kartu **Aplikace** a potom klikněte na tlačítko **Přidat** v dolní části obrazovky

    ![Přidání aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Vyberte **Publikování aplikace, která bude přístupná mimo vaši síť**.

    ![Publikování aplikace, která bude přístupná mimo vaši síť](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Zadejte následující informace o vaší aplikaci:

    - **Název**: Jednoduchý název vaší aplikace. Musí být v rámci adresáře jedinečný.
    - **Interní adresa URL**: Adresa, kterou konektor proxy aplikace používá pro přístup k aplikaci uvnitř vaší privátní sítě. Můžete zadat konkrétní cestu na beck-endovém serveru, kterou chcete publikovat, zatímco zbytek serveru publikovaný nebude. Tímto způsobem můžete publikovat různé stránky ze stejného serveru a použít pro každou z nich vlastní název a pravidla přístupu.
    - **Metoda předběžného ověření**: Způsob, jakým bude proxy aplikace ověřovat uživatele předtím, než jim poskytne přístup k vaší aplikaci. Zvolte jednu z možností z rozevírací nabídky.

        - Azure Active Directory: Proxy aplikace přesměruje uživatele na stránku pro přihlášení ke službě Azure AD, která ověří jejich oprávnění k adresáři a aplikaci.
        - Průchod: Uživatelé pro přístup k aplikaci nepotřebují ověření.

    ![Vlastnosti aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Dokončete průvodce kliknutím na ikonu zaškrtnutí v dolní části obrazovky. Aplikace je nyní definována ve službě Azure AD.


## Přiřazení uživatelů a skupin k aplikaci

Pokud chcete, aby uživatelé měli přístup k publikované aplikaci, musíte jim ho přiřadit – jednotlivě nebo po skupinách. U aplikací, které vyžadují předběžné ověření, tím udělíte oprávnění k používání aplikace. U aplikací, které nevyžadují předběžné ověření, uživatelé oprávnění nepotřebují, ale i tak je třeba přiřadit je k aplikaci, aby se jim zobrazovala v seznamu aplikací.

1. Po dokončení průvodce přidáním aplikace se vám zobrazí stránka „Rychlý start“ vaší aplikace. Pokud chcete nastavit, kdo má mít přístup k aplikaci, vyberte možnost **Uživatelé a skupiny**.

    ![Přiřazování uživatelů ze stránky „Rychlý start“ proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Vyhledejte konkrétní skupinu v adresáři nebo si zobrazte všechny uživatele. Kliknutím na značku zaškrtnutí zobrazíte výsledky.

    ![Hledání skupin nebo uživatelů – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Vyberte jednotlivé uživatele nebo skupiny, které chcete přiřadit k aplikaci a klikněte na **Přiřadit**. Zobrazí se výzva k potvrzení akce.

> [AZURE.NOTE] K aplikacím, které využívají integrované ověřování systému Windows, můžete přiřadit pouze uživatele a skupiny synchronizované s místní službou Active Directory. Hosty a uživatele, kteří se přihlašují pomocí účtu Microsoft, nelze k aplikacím publikovaným pomocí proxy aplikace služby Azure Active Directory přiřadit. Zajistěte, aby se vaši uživatelé přihlašovali pomocí přihlašovacích údajů, které jsou součástí stejné domény jako aplikace, kterou publikujete.


## Pokročilá konfigurace

Na stránce Konfigurace můžete publikované aplikace upravovat nebo u nich nastavovat pokročilé možnosti. Na této stránce si můžete svou aplikaci přizpůsobit změnou názvu nebo nahráním loga. Můžete také spravovat pravidla přístupu, například metodu předběžného ověření nebo vícefaktorové ověřování (Multi-Factor Authentication).

![Pokročilá konfigurace](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Jakmile aplikace publikujete pomocí proxy aplikace služby Azure Active Directory, zobrazí se v seznamu aplikací ve službě Azure AD, odkud je můžete spravovat.

Pokud po publikování aplikací služby proxy aplikace zakážete, aplikace se neodstraní, ale nebudou už přístupné mimo vaši privátní síť.

Pokud si chcete zobrazit aplikaci a ujistit se, že je přístupná, klikněte dvakrát na její název. Pokud je služba proxy aplikace zakázaná a aplikace není k dispozici, zobrazí se v horní části obrazovky varovná zpráva.

Pokud chcete aplikaci odstranit, vyberte ji ze seznamu a klikněte na **Odstranit**.

## Další kroky

- [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
- [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
- [Práce s aplikacemi, které pracují s deklaracemi](active-directory-application-proxy-claims-aware-apps.md)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)



<!--HONumber=Jun16_HO2-->



---
title: "Publikování aplikací pomocí proxy aplikace služby Azure AD | Dokumentace Microsoftu"
description: "Publikujte místní aplikace v cloudu pomocí proxy aplikací služby Azure AD v klasickém portálu."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 96490c0d060fe5486a7235a5aa76380c8d9b5d4f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikování aplikací pomocí proxy aplikace služby Azure AD

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-publish.md)

Proxy aplikace služby Azure AD umožňuje podporu vzdálených pracovních procesů publikováním místních aplikací, aby byly přístupné přes internet. V tomto bodě byste už měli mít [povolenou proxy aplikaci v portálu Azure Classic](active-directory-application-proxy-enable.md). Tento článek vám ukáže postup, jak publikovat aplikace, které běží v místní síti, a jak poskytnout zabezpečený vzdálený přístup mimo síť. Po dokončení tohoto článku budete připravení ke konfiguraci aplikace pomocí individuálních informací nebo požadavků na zabezpečení.

> [!NOTE]
> Proxy aplikace je funkce, která je dostupná jenom v případě, pokud jste upgradovali na edici Premium nebo Basic služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md). Pokud chcete použít proxy aplikace, můžete [Publikovat aplikace na webu Azure Portal](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-the-wizard"></a>Publikování aplikace pomocí průvodce
1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com/).
2. Přejděte do služby Active Directory a vyberte adresář, u kterého jste povolili proxy aplikace.
   
    ![Active Directory – ikona](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Klikněte na kartu **Aplikace** a potom klikněte na tlačítko **Přidat** v dolní části obrazovky
   
    ![Přidání aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Vyberte **Publikování aplikace, která bude přístupná mimo vaši síť**.
   
    ![Publikování aplikace, která bude přístupná mimo vaši síť](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Zadejte následující informace o vaší aplikaci:
   
   * **Název**: Jednoduchý název vaší aplikace. Musí být v rámci adresáře jedinečný.
   * **Interní adresa URL**: Adresa, kterou konektor proxy aplikace používá pro přístup k aplikaci uvnitř vaší privátní sítě. Můžete zadat konkrétní cestu na beck-endovém serveru, kterou chcete publikovat, zatímco zbytek serveru publikovaný nebude. Tímto způsobem můžete publikovat různé stránky ze stejného serveru a použít pro každou z nich vlastní název a pravidla přístupu.
     
     > [!TIP]
     > Pokud publikujete cestu, ujistěte se, že zahrnuje všechny nezbytné obrázky, skripty a šablony stylů pro vaši aplikaci. Pokud například vaše aplikace je v cestě https://yourapp/app a používá obrázky nacházející se v cestě https://yourapp/media, pak byste měli publikujete https://yourapp/ jako cestu.
     > 
     > 
   * **Metoda předběžného ověření**: Způsob, jakým proxy aplikace ověřuje uživatele předtím, než jim poskytne přístup k vaší aplikaci. Zvolte jednu z možností z rozevírací nabídky.
     
     * Azure Active Directory: Proxy aplikace přesměruje uživatele na stránku pro přihlášení ke službě Azure AD, která ověří jejich oprávnění k adresáři a aplikaci.
     * Průchod: Uživatelé pro přístup k aplikaci nepotřebují ověření.
     
     ![Vlastnosti aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. Dokončete průvodce kliknutím na ikonu zaškrtnutí v dolní části obrazovky. Aplikace je nyní definována ve službě Azure AD.

## <a name="assign-users-and-groups-to-the-application"></a>Přiřazení uživatelů a skupin k aplikaci
Pokud chcete, aby uživatelé měli přístup k publikované aplikaci, musíte jim ho přiřadit – jednotlivě nebo po skupinách. (Nezapomeňte přiřadit přístup také sobě.) Každý uživatel, kterého přiřadíte, musí mít licenci Azure úrovně Basic nebo vyšší. Licence můžete přiřadit individuálně, nebo skupinám. Další informace najdete v tématu [Přiřazování uživatelů k aplikaci](active-directory-applications-guiding-developers-assigning-users.md). 

U aplikací, které vyžadují předběžné ověření, uživateli přiřazením udělíte oprávnění k používání aplikace. U aplikací, které nevyžadují předběžné ověření, přiřazení uživatele znamená, že daný uživatel bude k aplikaci moci přistupovat přes přístupový panel.

1. Po dokončení průvodce přidáním aplikace se vám zobrazí stránka „Rychlý start“ vaší aplikace. Pokud chcete nastavit, kdo má mít přístup k aplikaci, vyberte možnost **Uživatelé a skupiny**.
   
    ![Přiřazování uživatelů ze stránky „Rychlý start“ proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Vyhledejte konkrétní skupinu v adresáři nebo si zobrazte všechny uživatele. Kliknutím na značku zaškrtnutí zobrazíte výsledky hledání.
   
      ![Hledání skupin nebo uživatelů – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Vyberte jednotlivé uživatele nebo skupiny, které chcete přiřadit k aplikaci a klikněte na **Přiřadit**. Zobrazí se výzva k potvrzení akce.

> [!NOTE]
> K aplikacím, které využívají integrované ověřování systému Windows, můžete přiřadit pouze uživatele a skupiny synchronizované s místní službou Active Directory. Hosty a uživatele, kteří se přihlašují pomocí účtu Microsoft, nelze k aplikacím publikovaným pomocí proxy aplikace služby Azure Active Directory přiřadit. Zajistěte, aby se vaši uživatelé přihlašovali pomocí přihlašovacích údajů, které jsou součástí stejné domény jako aplikace, kterou publikujete.
> 
> 

## <a name="test-your-published-application"></a>Test publikované aplikace
Po publikování můžete aplikaci otestovat tak, že přejdete na adresu URL, kterou jste publikovali. Ujistěte se, že k ní máte přístup, že se správně vykresluje a že všechno funguje podle očekávání. Pokud máte potíže nebo se zobrazí chybová zpráva, zkuste [průvodce odstraňováním potíží](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurace aplikace
Na stránce Konfigurace můžete publikované aplikace upravovat nebo u nich nastavovat pokročilé možnosti. Na této stránce si můžete svou aplikaci přizpůsobit změnou názvu nebo nahráním loga. Můžete také spravovat pravidla přístupu, například metodu předběžného ověření nebo vícefaktorové ověřování (Multi-Factor Authentication).

![Pokročilá konfigurace](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Jakmile aplikace publikujete pomocí proxy aplikace služby Azure Active Directory, zobrazí se v seznamu aplikací ve službě Azure AD, odkud je můžete spravovat.

Pokud po publikování aplikací služby proxy aplikace zakážete, aplikace už nebudou přístupné mimo vaši privátní síť. Vaši uživatelé stále budou mít místní přístup k aplikacím jako obvykle.

Pokud chcete zobrazit aplikaci a ujistit se, že je přístupná, klikněte dvakrát na její název. Pokud je služba proxy aplikace zakázaná a aplikace není k dispozici, zobrazí se v horní části obrazovky varovná zpráva.

Pokud chcete aplikaci odstranit, vyberte ji ze seznamu a klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
* [Práce s aplikacemi využívajícími deklarace](active-directory-application-proxy-claims-aware-apps.md)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)


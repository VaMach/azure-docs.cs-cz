---
title: "Automatizované SaaS zřizování uživatelů aplikace ve službě Azure AD | Microsoft Docs"
description: "Úvod do Azure AD můžete použít jak pro automatické zřizování, zrušit a průběžně aktualizovat uživatelské účty napříč různými aplikacemi SaaS jiných výrobců."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 3fe57e9c22d04a3557978093ce3fe86613c5c1d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatizovat uživatele zajišťování a rušení zajištění pro aplikace SaaS ve službě Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Co je automatické zřizování uživatelů pro aplikace SaaS?
Azure Active Directory (Azure AD) umožňuje automatizovat vytváření, údržbu a odebírání uživatelských identit v cloudu ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikace, jako je Dropbox, Salesforce, ServiceNow a další.

**Tady jsou některé příklady co tato funkce umožňuje, abyste mohli provádět:**

* Automaticky vytvořte nové účty v pravém systémy pro nové osoby, při připojí váš tým nebo organizace.
* Automaticky deaktivujte účtů v pravém systémech při osoby nechte tým nebo organizace.
* Zajišťují, aby identity aplikace a systémy aktuální na základě změn v adresáři nebo systému lidských zdrojů.
* Zřídit neuživatelských objekty, například skupin, k aplikacím, které je podporují.

**Zřizování uživatelů automatizované také zahrnuje následující funkce:**

* Možnost tak, aby odpovídala stávající identit mezi zdrojovým a cílovým systémem.
* Možnosti přizpůsobení nápovědě Azure AD podle aktuální konfigurace aplikací a systémů, které vaše organizace aktuálně používá.
* Volitelné e-mailové výstrahy pro zřizování chyby.
* Vytváření sestav a aktivity protokolů, které pomáhají s monitorování a řešení potíží.

## <a name="why-use-automated-provisioning"></a>Proč používat automatického zřizování?
Některé běžné motivace pro použití této funkce patří:

* Aby se zabránilo náklady, umožňuje zvýšit efektivitu a lidské chyby související s ručním procesy pro zřizování.
* Aby se zabránilo nákladů spojených s hostování a údržbu zákaznických zřizování řešení a skripty
* Chcete-li okamžitě odebrání identity uživatelů z klíče aplikace SaaS, když opustí organizaci zabezpečit vaši organizaci.
* Snadno importovat číslem hromadně uživatelů do určité aplikaci SaaS nebo systému.
* Abyste mohli využívat pohodlí nutnosti řešení zřizování, spusťte z stejné zásady přístupu aplikace, které definované pro Azure AD jednotné přihlašování.


## <a name="how-does-automatic-provisioning-work"></a>Jak funguje automatické zřizování
    
**Zřizování služby Azure AD** zřídí uživatelům aplikace SaaS a dalšími systémy připojením ke koncovým bodům uživatelské rozhraní API pro správu poskytnutý dodavatelem každé aplikace. Rozhraní API koncových bodů pro tyto uživatele správu povolit službě Azure AD prostřednictvím kódu programu vytvářet, aktualizovat a odebírat uživatele. Pro vybrané aplikace, zřizování služby mohou také vytvářet aktualizovat a odebírat další spojené s objekty, například skupin a rolí. 

![Zřizování](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*obrázek 1: Zřizování služby Azure AD*

![Odchozí zřizování](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*obrázek 2: "Odchozí" uživatele zřizování pracovního postupu z Azure AD oblíbených aplikací SaaS.*

![Příchozí zřizování](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*obrázek 3: zřizování pracovního postupu z oblíbených aplikací lidského kapitálové správy (HCM) do Azure Active Directory a Windows Server Active Directory uživatelů "Příchozí"*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Jaké aplikace a systémy lze použít při zřizování automatické uživatele Azure AD?

Funkce Azure AD předem integrované podpory pro různé oblíbených aplikací SaaS a systémy lidské zdroje a také obecné podporu pro aplikace, které implementují konkrétní části [SCIM 2.0 standard](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-scim-provisioning).

Všechny aplikace "Doporučený" v galerii aplikací Azure AD, podporují zřizování automatizované uživatelů. [Zde můžete zobrazit seznam vybraných aplikací.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Aby aplikace pro podporu automatické zřizování uživatelů se musí nejprve zadejte potřebné uživatelské koncové body správy, které umožňují externí programy automatizovat vytváření, údržbu a odebírání uživatelů. Ne všechny aplikace SaaS jsou proto kompatibilní s touto funkcí. Pro aplikace, které podporují rozhraní API správy uživatelů technický tým služby Azure AD bude moct vytvářet zřizování konektor pro tyto aplikace a činnost prioritu podle potřeb aktuální a potenciální zákazníky. 

Kontaktujte Azure AD technickému týmu požádat o zřizování podporu dalších aplikací, odeslání zprávy prostřednictvím [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning). 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Jak je nastavit automatické zřizování pro aplikace?

Konfigurace služby Azure AD pro vybrané aplikace se spustí v zřizování služby  **[portál Azure](https://potal.azure.com)**. V **Azure Active Directory > podnikové aplikace, které** vyberte **přidat**, pak **všechny**a poté přidejte následující v závislosti na vašem scénáři:

* Všechny aplikace v **vybrané aplikace** části Podpora automatické zřizování

* Pomocí možnosti "bez Galerie aplikace" pro zákaznických SCIM integrace

![Galerie](./media/active-directory-saas-app-provisioning/gallery.png)

Na obrazovce správy aplikací, zřizování je nakonfigurovaný v **zřizování** kartě.

![Nastavení](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Přihlašovací údaje správce** je třeba zadat do služby Azure AD zřizování služby, který vám umožní připojení k rozhraní API poskytuje aplikace pro správu uživatelů.

* **Mapování atributů** lze nakonfigurovat, zadejte, který polí ve zdrojovém systému (Příklad: Azure AD) bude jejich obsah synchronizují do pole, která v cílovém systému (Příklad: ServiceNow). Pokud cílová aplikace podporuje, v této části vám umožní Volitelně lze konfigurovat zřizování skupin kromě uživatelských účtů. "Odpovídající vlastnosti" umožňují vyberte pole, která se používají tak, aby odpovídaly účty mezi systémy. "[Výrazy](active-directory-saas-writing-expressions-for-attribute-mappings.md)" vám umožní změnit a transformace hodnot načten ze zdrojového systému, než se zapisují do cílového systému. Další informace najdete v tématu [přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md).

![Nastavení](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Filtry oborů** říct zřizování služby, kteří uživatelé a skupiny ve zdrojovém systému by měl zřídit nebo zrušit do cílového systému. Existují dva aspekty oborů filtry, které jsou vyhodnocovány společně určující, který je v oboru pro zřizování:

* **Filtr pro hodnoty atributu** -nabídky "Zdrojového objektu obor" v mapování atributů umožňuje filtrování na konkrétní atribut hodnoty. Například můžete zadat, že by měl být jenom uživatelé s atributem "Oddělení" "Prodeje" v oboru pro zřizování.

* **Filtr pro přiřazení** -nabídky "Obor" v funkce zřizování > portálu v oddílu nastavení umožňuje určit, jestli by měl být pouze "přiřazené" Uživatelé a skupiny v oboru pro zřizování, nebo pokud by měly být všechny uživatele v adresáři služby Azure AD zřízení. Informace o "přiřazení" uživatelů a skupin najdete v tématu [přiřadit uživatele nebo skupinu enterprise aplikace v Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Nastavení** řízení operaci zřizování služby pro aplikaci, včetně toho, jestli je aktuálně spuštěna, nebo ne.

* **Protokoly auditu** zadejte záznamy každé operace provádí zřizování služby Azure AD. Další podrobnosti najdete v tématu [zřizování průvodce vytvářením sestav](active-directory-saas-provisioning-reporting.md).

![Nastavení](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

## <a name="what-happens-during-provisioning"></a>Co se stane při zřizování?

1. Když povolíte zřizování pro aplikace pro první, byly provedeny následující akce:
   * Azure AD se pokusí přiřadit všichni stávající uživatelé v aplikaci SaaS pro jejich odpovídající identit v adresáři. Pokud je nalezena shoda uživatele, jsou *není* automaticky povolené pro jednotné přihlašování. Pořadí pro uživatele tak, aby měl přístup k aplikaci, musí být explicitně přiřazena k aplikaci ve službě Azure AD, buď přímo nebo prostřednictvím členství ve skupině.
   * Pokud jste už zadali, kteří uživatelé by se měla přiřadit k aplikaci, a pokud se nepodaří najít existujících účtů pro uživatele Azure AD, Azure AD zřídit nové účty pro ně v aplikaci.
2. Po dokončení počáteční synchronizace se dokončila, jak je popsáno výše, Azure AD zkontroluje každých 20 minut, než následující změny:
   * Pokud noví uživatelé mají přiřazený k aplikaci (buď přímo nebo prostřednictvím členství ve skupinách), pak jsou zřízené pomocí nového účtu v aplikaci SaaS.
   * Pokud byl odebrán přístup uživatele, pak svého účtu v aplikaci SaaS jsou označeny jako zakázané (uživatele jsou odstraněny nikdy plně, která chrání před ztrátou dat v případě nesprávnou konfiguraci).
   * Pokud uživatel byl nedávno přiřazen k aplikaci a už jako účet v aplikaci SaaS, tento účet je označena jako povolené a některé vlastnosti uživatele může být aktualizován, pokud jsou zastaralé, ve srovnání s adresáři.
   * Došlo ke změně informací o uživateli (například telefonní číslo, umístění kanceláře) v adresáři, pak tyto informace budou také aktualizovat v aplikaci SaaS.


## <a name="frequently-asked-questions"></a>Nejčastější dotazy
**Jak často Azure AD zápisu directory změn do aplikace SaaS?**

Po dokončení počáteční úplné synchronizace Azure AD zřizování služby obvykle kontroluje změny každých 20 minut. 

Pokud aplikace SaaS vrací několik chyb, (například jako v případě přihlašovací údaje správce neplatný), pak v Azure AD postupně snížit jeho četnost maximálně jednou za den, dokud se nevyřeší chyby. Pokud k tomu dojde, přejde do stavu "karantény" úlohy zřizování Azure AD a označuje to [zřizování souhrnnou sestavu](active-directory-saas-provisioning-reporting.md).

**Jak dlouho bude trvat ke zřízení Moji uživatelé?**

Po dokončení počáteční úplné synchronizace přírůstkové změny obvykle dojít během 20 40 minut. Pokud chcete zřídit většinu adresáře, pak závisí na počtu uživatelů a skupin, které máte. Výkon je závislé na výkonu rozhraní API, která zřizování služeb používá ke čtení dat ze zdrojového systému a zapsat data do cílového systému správy uživatelů. 

Výkon je taky pomalejší, pokud je mnoho chyb (zaznamenávají [protokoly auditu](active-directory-saas-provisioning-reporting.md)) a službu zřizování přešel do stavu "karantény".

**Co je počáteční úplné synchronizace a proč trvá déle než následné synchronizace?**

Při prvním spuštění zřizování služby Azure AD pro danou aplikaci, se vytváří "snímek" uživatelů (a volitelně skupin) ze zdrojového adresáře. Tento snímek povoluje službu zřizování a snížit počet zpátečních cest k zdrojové a cílové rozhraní API a umožňuje následné "delta" synchronizace se bude chovat efektivněji. 

Počáteční úplnou synchronizaci uživatelů, můžete často dokončit v minutách pro velmi malé adresáře, ale může trvat několik hodin pro větší adresáře. Enterprise adresáře s stovky tisíc uživatelů může trvat několik hodin pro počáteční synchronizaci k dokončení. Ale po počáteční synchronizaci, následných "delta" synchronizace dojít mnohem rychlejší.

> [!NOTE]
> Pro aplikace, které podporují zřizování skupin a členství ve skupinách povolení to výrazně zvyšuje čas potřebný k dokončení úplné synchronizace. Pokud nechcete zřídit názvy skupin a členství ve skupinách do vaší aplikace, můžete zakázat v [mapování atributů](active-directory-saas-customizing-attribute-mappings.md) zřizování konfigurace.

**Jak můžete sledovat průběh úlohy aktuální zřizování?**

Najdete v článku [zřizování průvodce vytvářením sestav](active-directory-saas-provisioning-reporting.md).

**Jak poznám, že pokud uživatelům nedaří získat zřízený správně?**

Všechny chyby se zaznamenávají do Azure AD protokoly auditu. Další informace najdete v tématu [zřizování průvodce vytvářením sestav](active-directory-saas-provisioning-reporting.md).

**Jak mohu odeslat zpětnou vazbu technickému týmu?**

Kontaktujte nás prostřednictvím [fóru pro zpětnou vazbu Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Zapisují se výrazy pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Zapnutí automatického zřizování uživatelů a skupin ze služby Azure Active Directory do aplikací pomocí SCIM](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md)


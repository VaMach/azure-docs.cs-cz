---
title: "Jak se aplikace přidávají do Azure Active Directory."
description: "Tento článek popisuje, jak se aplikace přidávají do instance služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: shoatman
manager: kbrint
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 6ffcfcb7ed071a12b0b3495ad534fd00f6d6ad99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Jak a proč se aplikace přidávají do služby Azure AD
Jednou z věcí původně puzzling při zobrazení seznamu aplikací ve vaší instanci Azure Active Directory je pochopení, kde aplikace pochází z a proč jsou existuje.  Tento článek vám poskytne podrobný přehled jak aplikací jsou reprezentována v adresáři a poskytnout kontext, který vám pomůže porozumět, jak byla aplikace ve vašem adresáři.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Jaké služby Azure AD poskytuje k aplikacím?
Aplikace se přidají do služby Azure AD můžete využít jeden nebo více služeb, které poskytuje.  Tyto služby patří:

* Aplikace ověřování a autorizace
* Ověření uživatele & autorizace
* Jednotné přihlašování (SSO) pomocí federace nebo hesla
* Zřizování uživatelů a synchronizace
* Řízení přístupu na základě rolí; Použití adresář, který chcete definovat role aplikaci k provedení role na základě kontroly autorizace v aplikaci.
* služby autorizace oAuth (používá Office 365 a jiným aplikacím společnosti Microsoft k autorizaci přístupu k rozhraní API nebo prostředky.)
* Publikování aplikací pro & XY; Publikování aplikace z privátní sítě k Internetu

## <a name="how-are-applications-represented-in-the-directory"></a>Jak jsou v adresáři reprezentované aplikace?
Aplikace jsou reprezentováni v Azure AD pomocí 2 objekty: objekt aplikace a objektu zabezpečení služby.  Se jeden objekt aplikace zaregistrované v "home" nebo "vlastník" nebo "publikování" adresář a jeden nebo více služeb hlavní objekty představující aplikace každý adresář, ve kterém funguje.  

Objekt aplikace popis aplikace do služby Azure AD (víceklientské služby) a můžou obsahovat žádné z následujících: (*Poznámka*: Nejedná se o vyčerpávající seznam.)

* Název, loga a vydavatel
* Tajné klíče (symetrické nebo asymetrické klíče, které slouží k ověření aplikace)
* Rozhraní API závislosti (oAuth)
* Rozhraní API prostředky nebo oborům publikována (oAuth)
* Aplikace role (RBAC)
* Metadata jednotné přihlašování a konfigurace (SSO)
* Zřizování uživatelů metadata a konfigurace
* Proxy server, metadat a konfigurace

Objekt služby je záznam aplikace každý adresář, kde aplikace funguje, včetně jeho domovského adresáře.  Objekt služby:

* Odkazuje na objekt aplikace přes vlastnost id aplikace
* Zaznamenává místních uživatelů a přiřazení role aplikace skupin
* Zaznamenává místní uživatele a správce oprávnění uděleno do aplikace
  * Příklad: oprávnění pro aplikaci pro přístup k e-mailu s konkrétní uživatelé
* Zaznamenává místní zásady, včetně zásad podmíněného přístupu
* Zaznamenává místní alternativní místní nastavení pro aplikace
  * Pravidla transformace deklarací identity
  * Mapování atributů (zřizování uživatelů)
  * Klienta konkrétní aplikaci rolí (Pokud aplikace podporuje vlastní role)
  * Název nebo loga

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Diagram objekty aplikací a objekty služby napříč adresáře
![Diagram znázorňující, jak aplikace objekty a služby objekty existující s instancí služby Azure AD.][apps_service_principals_directory]

Jak je vidět v diagramu výše.  Společnost Microsoft spravuje dva adresáře interně (vlevo) používá k publikování aplikací.

* Jeden pro Microsoft Apps (Microsoft services directory)
* Jeden pro předběžně integrované aplikace 3. stran (adresář Galerie aplikací)

Aplikace vydavatelů nebo dodavatelů, kteří integraci se službou Azure AD musí mít publikování adresáře.  (Adresář některé SAAS).

Aplikace, které přidáte sami patří:

* Aplikace, které jste vytvořili (integrovaný s AAD)
* Aplikace, které jste připojeni pro jednotného přihlašování
* Aplikace, můžete publikovat pomocí proxy aplikace služby Azure AD.

### <a name="a-couple-of-notes-and-exceptions"></a>Několik poznámek a výjimek
* Ne všechny objekty služby, přejděte zpět na objekty aplikací.  Huh? Pokud Azure AD bylo původně vytvořeno služeb poskytovaných aplikacích byly mnohem omezenější a instanční objekt dostatečné pro vytvoření identity aplikace.  Původní instanční objekt byl co nejblíže ve tvaru k účtu služby Windows Server Active Directory.  Z tohoto důvodu je stále možné vytvářet objekty služby pomocí Azure AD PowerShell bez vytvoření první objekt aplikace.  Rozhraní Graph API vyžaduje objekt aplikace před vytvořením objektu služby.
* Ne všechny informace popsané výše je aktuálně zveřejněný prostřednictvím kódu programu.  Toto jsou k dispozici pouze v uživatelském rozhraní:
  * Pravidla transformace deklarací identity
  * Mapování atributů (zřizování uživatelů)
* Další podrobné informace o objektu služby a objekty aplikací naleznete v dokumentaci rozhraní API REST Azure AD Graph odkaz.  *Pomocný parametr*: je nejblíže si referenční dokumentace schématu je Azure AD Graph API dokumentace pro Azure AD, který je aktuálně k dispozici.  
  * [Aplikace](https://msdn.microsoft.com/library/azure/dn151677.aspx)
  * [Instanční objekt](https://msdn.microsoft.com/library/azure/dn194452.aspx)

## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Jak se aplikace přidají do instance Moje Azure AD?
Existuje mnoho způsobů, lze přidat aplikaci do služby Azure AD:

* Přidat aplikaci z [Galerie Azure Active Directory aplikací](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Přihlášení/do 3rd, aplikace jiných výrobců integrované s Azure Active Directory (například: [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Při přihlášení/v uživatelé vyzváni k zadání oprávnění k aplikaci pro přístup k jejich profil a další oprávnění.  První osoba, která vyjádřit svůj souhlas způsobí, že objekt služby představující aplikace má být přidán do adresáře.
* Přihlášení/do služeb Microsoft online services, jako [Office 365](http://products.office.com/)
  * Při přihlášení k odběru Office 365 nebo zahájit zkušební verze, které jsou v adresáři představující různé služby, které se používají k zajišťování všechny funkce související s Office 365 vytvořeny objekty služby jeden nebo více.
  * Některé služby Office 365, jako je SharePoint vytvořit objekty služby na základě průběžné zabezpečené komunikaci mezi součástmi, včetně pracovních postupů.
* Přidat aplikaci vyvíjíte najdete v tématu Azure Management Portal: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Přidání, najdete v části aplikace, které vyvíjíte pomocí sady Visual Studio:
  * [Metody ověřování ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Připojených služeb](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Přidat aplikaci k používání [proxy aplikace služby Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Připojit aplikaci pro jednotné přihlašování pomocí SAML nebo jednotného přihlašování k heslo
* Mnoho dalších včetně různé možnosti pro vývojáře v Azure a nebo v Průzkumníku API výsledky v různých centra pro vývojáře

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kdo má oprávnění k přidání aplikace do instance Moje Azure AD?
Jenom globální správci postupovat následovně:

* Přidání aplikací v galerii aplikací Azure AD (předem integrovaných 3. stran aplikace)
* Publikování aplikace pomocí Azure AD Application Proxy

Všichni uživatelé v adresáři mít práva k přidání aplikace, které jejich vývoj a uvážení přes aplikace, které se sdílená složka nebo udělení přístupu k jejich organizačním datům.  *Mějte na paměti, přihlášení uživatele nahoru nebo na aplikaci a udělení oprávnění může vést k vytváření objektu služby.*

Tato původně zvukové týkající se může, ale zachová následující rozhodnutí:

* Aplikace se moct využívat Windows Server Active Directory pro ověřování uživatelů pro mnoho let bez nutnosti aplikaci, která bude zaregistrován nebo zaznamenány v adresáři.  Organizace teď bude lepší viditelnost na přesně jak velký počet aplikací používat adresář a what for.
* Pro správce řízené proces publikování/registrace aplikace není nutné.  S Active Directory Federation Services je pravděpodobné, že správce musel přidat aplikaci jako předávající strana jménem vývojáři.  Nyní mohou vývojáři samoobslužné služby.
* Uživatelé podepisování v, až do aplikace pomocí účtu organizace pro obchodní účely je dobré.  Pokud následně opustí organizaci dojde ke ztrátě přístupu k účtu v aplikaci, kterou používají.
* S záznam jaká data byla sdílené, pomocí kterého aplikace je dobré.  Data jsou přenositelné více než kdy dřív a nutnosti zrušte záznam kdo jaká data sdílet s aplikací, které jsou užitečné.
* Aplikace, které používají Azure AD pro oAuth rozhodnout přesně uvedete, jaká oprávnění, které uživatelé mohou udělit k aplikacím a které vyžadují oprávnění správce s.  Má přejít bez informacemi o tom, že pouze správci mohou souhlas s větší obory a větších oprávnění.
* Uživatele, přidání a povolení aplikace přistupovat ke svým datům jsou auditované události, aby se dala zobrazit zprávy o auditu v rámci portálu Azure Managment k určení, jak aplikaci byl přidán do adresáře.

**Poznámka:** *Microsoft samotné provozována používat tuto výchozí konfiguraci pro mnoho měsíců teď.*

Se všemi této uvedená je možné zabránit uživatelům ve vašem adresáři v přidání aplikace a z výkonu uvážení nad jaké informace sdílejí s aplikacemi změnou konfigurace Directory na portálu správy Azure.  Následující konfigurace jsou přístupné na portálu Azure Management na svého adresáře na kartě "Konfigurace".

![Snímek obrazovky uživatelského rozhraní pro konfiguraci nastavení integrované aplikace][app_settings]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
Další informace o tom, jak přidat aplikace do služby Azure AD a postup konfigurace služby pro aplikace.

* Vývojáři: [zjistěte, jak integrovat aplikaci AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Vývojáři: [zkontrolujte ukázkový kód pro aplikace, které jsou integrované s Azure Active Directory na Githubu](https://github.com/AzureADSamples)
* Vývojářů a IT profesionály: [přečtěte si dokumentaci k REST API pro Azure Active Directory Graph API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT specialisté: [Další informace o použití předem integrovaných aplikací Azure Active Directory z Galerie aplikace](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT specialisté: [kurzy pro konfiguraci konkrétní předběžně integrované aplikace](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT specialisté: [zjistěte, jak publikovat aplikace pomocí Proxy aplikace Azure Active Directory](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Viz také
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg

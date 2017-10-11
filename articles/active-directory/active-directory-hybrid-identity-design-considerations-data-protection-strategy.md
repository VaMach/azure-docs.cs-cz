---
title: "Azure Active Directory hybridní identity aspekty návrhu - definování strategie pro ochranu dat | Microsoft Docs"
description: "Definujete strategie ochrany dat pro řešení hybridní identity splňovat obchodní požadavky, které jste definovali."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6c9b7423fa56886104bc6060d25904277b75f30c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definování strategie ochrany dat pro vaše řešení hybridní identity
V této úloze určíte strategie ochrany dat pro vaše řešení hybridní identity k splňovat obchodní požadavky definovaný v:

* [Určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [Stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Určete požadavky řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Stanovení požadavků na reakce na incidenty](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Zadejte možnosti ochrany dat
Jak je popsáno v [určení požadavků na synchronizaci adresáře](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), umístěným na místních Microsoft Azure AD můžete synchronizovat s vaší Active Directory Domain Services (AD DS). Tato integrace umožňuje organizacím využít Azure AD ověřit přihlašovací údaje uživatele při přístupu k podnikovým prostředkům. To lze provést pro oba scénáře: data na rest místně a v cloudu.  Přístup k datům ve službě Azure AD vyžaduje ověření uživatele pomocí služby tokenů zabezpečení (STS).

Po ověření hlavní název uživatele (UPN) je pro čtení z ověřovací token a replikované oddílu a je určena kontejneru odpovídající uživatele domény. Informace o existence, povoleném stavu a role uživatele se používá ověřování systému k určení, zda je požadovaný přístup do cílového klienta autorizovaný pro tohoto uživatele v této relaci. Některé akce oprávnění (konkrétně vytvořit uživateli, resetování hesla) vytvořit záznam pro audit, který můžete použít správce klienta ke správě úsilí o dodržování předpisů nebo šetření.

Přesunutí dat z vašeho místního datového centra do úložiště Azure přes připojení k Internetu nemusí být vždy proveditelné kvůli datový svazek, dostupnosti šířky pásma nebo další důležité informace. [Službu Import/Export úložiště Azure](../storage/common/storage-import-export-service.md) nabízí možnost založené na hardwaru pro umístění nebo načítání velkých objemů dat v úložišti objektů blob. Umožňuje vám odesílat [šifrované nástrojem BitLocker](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) jednotky pevného disku přímo do datového centra Azure kde operátoři cloudu se nahrát obsah na váš účet úložiště, nebo mohou stáhnout Azure data do jednotky vrátí. Přijímají se jenom šifrované disky pro tento proces (pomocí klíčem nástroje BitLocker vygenerované službou samotné během instalace úlohy). Klíč nástroje BitLocker je určen k Azure samostatně, a zajišťuje tak mimo klíče sdílení vzdálené správy.

Vzhledem k tomu, že přenášených dat můžete provést v různých scénářích, je také důležité vědět, že Microsoft Azure používá [virtuálních sítí](https://azure.microsoft.com/documentation/services/virtual-network/) k izolaci provozu klientů od sebe navzájem zaměstnávající míry například hostitele a hostů úroveň brány firewall, filtrování paketů IP, portu blokování a koncové body HTTPS. Většina interních komunikací Azure, včetně infrastruktury infrastruktury a infrastruktury zákazníka (místní), ale jsou šifrované. Další důležitou možností je komunikace v rámci datových centrech Azure; Microsoft spravuje sítě, aby zajistil, že žádný virtuální počítač může zosobnit nebo tajně poslouchat na IP adresu jiného. Protokol TLS/SSL se používá při přístupu k Azure Storage nebo databází SQL, nebo při připojení ke cloudové služby. Správce zákazníka v takovém případě zodpovídá za získání certifikátu TLS/SSL a jeho nasazení do infrastruktury svého klienta. Přesunutí přenosy dat mezi virtuálními počítači v jednom nasazení nebo mezi klienty v jednom nasazení prostřednictvím Microsoft Azure Virtual Network se dají chránit pomocí šifrovanou komunikaci protokoly, jako je například HTTPS, SSL/TLS nebo jiné.

V závislosti na tom, jak jste odpověděli na otázky v [určení požadavků na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), musí být schopní určit, jak chcete chránit vaše data a jak hybridní řešení identit vám pomůže na který. V tabulce jsou uvedeny možnosti nepodporuje v Azure, které pro každý scénář ochrany dat jsou k dispozici.

| Možnosti ochrany dat | V klidovém stavu uložených v cloudu | Na rest na místě | Při přenosu |
| --- | --- | --- | --- |
| Nástroj BitLocker Drive Encryption |X |X | |
| SQL Server k šifrování databáze |X |X | |
| Šifrování virtuálních počítačů VM | | |X |
| SSL/TLS. | | |X |
| Síť VPN | | |X |

> [!NOTE]
> Čtení [dodržování předpisů podle funkce](https://azure.microsoft.com/support/trust-center/services/) v [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) Další informace o certifikáty, které je kompatibilní s každou službu Azure.
> Vzhledem k tomu použít s vícevrstvých možnosti ochrany dat, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá pro každý stav, který data budou k dispozici všechny možnosti.
>
>

## <a name="define-content-management-options"></a>Zadejte možnosti správy obsahu
Jednou z výhod používání služby Azure AD spravovat hybridní infrastrukturu identit je, že proces je z hlediska koncového uživatele zcela transparentní. Uživatel se pokusí o přístup sdílený prostředek, prostředek vyžaduje ověření, uživatel bude muset odeslat požadavek na ověření do služby Azure AD, aby bylo možné získat token a přístup k prostředku. Tento celý proces probíhá pozadí bez zásahu uživatele. Je také možné udělit oprávnění k [skupiny](active-directory-manage-groups.md#getting-started-with-access-management) uživatelů, aby mohli provádět některé běžné akce.

Organizace, které jsou starat o ochrany osobních údajů obvykle vyžadují klasifikace dat pro své řešení. Pokud své aktuální místní infrastrukturu se již používá klasifikaci dat, je možné využít Azure AD jako hlavní úložiště pro identitu uživatele. Nástroj běžné, že je použité místní pro klasifikaci dat se nazývá [sada nástrojů klasifikace dat](https://msdn.microsoft.com/library/Hh204743.aspx) pro Windows Server 2012 R2. Tento nástroj může pomoct určit, klasifikovat a chránit data na souborových serverech v privátním cloudu. Je také možné využít [automatickou klasifikaci souborů](https://technet.microsoft.com/library/hh831672.aspx) ve Windows serveru 2012 toho chcete dosáhnout.

Pokud vaše organizace nemá klasifikace dat na místě, ale je potřeba chránit citlivé soubory bez přidání nové servery místně, můžou použít Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS pomocí šifrování, identity a autorizace zásady pomáhají zabezpečit soubory a e-mailu a funguje napříč více zařízeními – telefony, tablety a počítače. Protože je Cloudová služba Azure RMS, není potřeba explicitně konfigurovat vztahy důvěryhodnosti s jinými organizacemi, než se chráněný obsah můžete sdílet s nimi. Pokud už mají služby Office 365 nebo adresář služby Azure AD, podporuje se automaticky spolupráce mezi organizacemi. Můžete také synchronizovat jenom atributy adresáře, které Azure RMS potřebuje podporu společné identity pro místní účty služby Active Directory, a to pomocí Azure Active Directory synchronizační služby (AAD Sync) nebo Azure AD Connect.

Důležitou součástí správy obsahu je pochopit, kdo přistupuje k který prostředek, proto je důležité pro řešení správy identit možnosti bohaté protokolování. Azure AD poskytuje protokolu více než 30 dní, včetně:

* Změny v členství role (např: uživatel přidaný k roli globálního správce)
* Aktualizace na přihlašovací údaje (např: změny hesla)
* Správa domény (např: ověření vlastní domény, odebrání domény)
* Přidání nebo odebrání aplikací
* Správa uživatelů (např: Přidání, odebrání, aktualizaci uživatele)
* Přidání nebo odebrání licencí

> [!NOTE]
> Čtení [zabezpečení Microsoft Azure a správa protokolu auditu](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) Další informace o možnosti protokolování v Azure.
> V závislosti na tom, jak jste odpověděli na otázky v [stanovení požadavků na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), musí být schopní určit, jakým způsobem chcete obsahu, který se dají spravovat hybridní řešení identit. Všechny možnosti, které jsou zveřejněné v tabulce 6 jsou sice může integrovat s Azure AD, je důležité určit, které je vhodnější pro konkrétní obchodní potřeby.
>
>

| Možnosti správy obsahu | Výhody | Nevýhody |
| --- | --- | --- |
| Centralizovaný místně (server služby Active Directory Rights Management Server) |Plnou kontrolu nad serverovou infrastrukturu, která je zodpovědná za klasifikace dat <br> Integrované funkce v systému Windows Server, není nutné pro další licence nebo předplatné <br> Umožňuje integraci s Azure AD v hybridním scénáři <br> Podporuje informace rights management (IRM) funkce v služeb Microsoft Online services, jako je Exchange Online a SharePoint Online, jakož i Office 365 <br> Podporuje místní serverové produkty společnosti Microsoft, například Exchange Server, SharePoint Server a souborové servery se systémem Windows Server a infrastruktury klasifikace souborů (FCI). |Vyšší, údržby (zachovat až s aktualizacemi, konfigurace a potenciální upgrady), protože IT vlastní serveru <br> Vyžaduje serverovou infrastrukturu na místě<br> Doesn'tleverage možnosti Azure nativně |
| Centralizovaný v cloudu (Azure RMS) |Snazší správa ve srovnání s místním řešením <br> Umožňuje integraci se služba AD DS v hybridním scénáři <br>  Plně integrované se službou Azure AD <br> Nevyžaduje místní server za účelem nasazení služby <br> Podporuje místní serverové produkty společnosti Microsoft, například Exchange Server, SharePoint, Server a souborové servery se systémem Windows Server a souboru klasifikace infrastruktury (FCI) <br> Oddělení IT, může mít úplnou kontrolu nad klíčem příslušného tenanta na funkci BYOK. |Vaše organizace musí mít cloudové předplatné podporující službu RMS <br> Vaše organizace musí mít adresář Azure AD pro podporu ověřování uživatelů pro službu RMS |
| Hybridní (Azure RMS integrovat, místní Active Directory Rights Management Server) |Tento scénář shromažďuje výhody obou, centralizovaný místně a v cloudu. |Vaše organizace musí mít cloudové předplatné podporující službu RMS <br> Vaše organizace musí mít adresář Azure AD pro podporu ověřování uživatelů pro službu RMS, <br> Vyžaduje připojení mezi cloudové služby Azure a místní infrastruktury |

## <a name="define-access-control-options"></a>Zadejte možnosti řízení přístupu
S využitím ověřování, autorizace a řízení přístupu možnosti dostupné v Azure AD bude moci povolit vaší společnosti používání úložiště v centrální identity při povolení uživatelů a partnery používat jednotné přihlašování (SSO), jak je znázorněno na obrázku níže:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centralizovaná správa a plně integraci s další adresáře

Poskytuje jednotné přihlašování k tisícům aplikací SaaS Azure Active Directory a místní webové aplikace. Přečtěte si prosím [seznam kompatibility federace Azure Active Directory: poskytovatelů identit třetích stran, které lze použít k implementaci jednotného přihlašování](https://msdn.microsoft.com/library/azure/jj679342.aspx) článku Další podrobnosti o třetích stran jednotné přihlašování, testovaná společností Microsoft. Tato funkce umožňuje organizaci implementace celé řady scénářů B2B zachováním kontrolu nad správu identit a přístupu. Však během B2B navrhování procesu je důležité si uvědomit, metodu ověřování, který se použije partner poskytující prostředky a ověřit, jestli je tato metoda nepodporuje v Azure. Aktuálně jsou tyto metody podporovaný službou Azure AD:

* Security Assertion Markup Language (SAML)
* OAuth
* Pomocí protokolu Kerberos
* Tokeny
* Certifikáty

> [!NOTE]
> Přečtěte si [Azure Active Directory ověřovací protokoly](https://msdn.microsoft.com/library/azure/dn151124.aspx) znát podrobnosti o jednotlivých protokolů a jeho funkce v Azure.
>
>

Podpora Azure AD, mobilní obchodní aplikace pomocí stejné prostředí pro ověření snadno Mobile Services zaměstnancům pro přihlášení do svých mobilních aplikací s svoje podnikové přihlašovací údaje služby Active Directory. Pomocí této funkce je podporována Azure AD jako zprostředkovatele identity v Mobile Services společně s jiných poskytovatelů identit, které už podporujeme (mezi které patří Accounts Microsoft, Facebook ID, Google ID a Twitter ID). Pokud místní aplikace používá přihlašovací údaje uživatele, které jsou umístěné v společnosti AD DS, musejí být transparentní přístup z partnery a uživateli z cloudu. Můžete spravovat uživatele podmíněného řízení přístupu (založené na cloudu) webové aplikace, webové rozhraní API, cloudové služby společnosti Microsoft, aplikace SaaS 3. stran a nativní (mobilní) klientské aplikace a má i výhody zabezpečení, auditování, generování sestav z jednoho místa. Doporučujeme však to ověřit v testovacím prostředí nebo s omezené množství uživatelů.

> [!TIP]
> je důležité zmínit, že Azure AD nemá zásad skupiny má služby AD DS. Chcete-li vynutit zásady pro zařízení, budete potřebovat řešení správy mobilních zařízení, jako [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Po ověření uživatele pomocí služby Azure AD, je důležité vyhodnotit úroveň přístupu, uživatel bude mít. Úroveň přístupu, že uživatel, bude mít na prostředku se může měnit, zatímco Azure AD můžete přidat další vrstvu zabezpečení řízení přístupu k některým prostředkům, můžete musí také mějte na paměti, že prostředek samotné může mít svůj vlastní seznam řízení přístupu samostatně , jako je například řízení přístupu pro soubory umístěné na souborovém serveru. Následující obrázek shrnuje úrovně řízení přístupu, která může mít v hybridním scénáři:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Všechny interakce v diagramu vám obrázek X představuje jeden scénář řízení přístupu, která mohou být předmětem Azure AD. Zde máte popis jednotlivých scénářů:

1. Podmíněný přístup k aplikacím, které jsou hostované na místě: registrovaná zařízení se zásadami přístupu můžete použít pro aplikace, které jsou nakonfigurovány pro použití služby AD FS v systému Windows Server 2012 R2. Další informace o nastavení podmíněného přístupu pro místní úložiště naleznete v tématu [Nastavení místního podmíněného přístupu pomocí nástroje Registrace zařízení služby Azure Active Directory](active-directory-conditional-access.md).

2. Řízení přístupu na portál Azure: Azure také umožňuje řídit přístup k portálu pomocí řízení přístupu na základě role (RBAC)). Tato metoda umožňuje společnosti umožní omezit množství operací, které konkrétního můžete provést na portálu Azure. Pomocí RBAC pro řízení přístupu na portál, můžete správci IT delegovat přístup pomocí následujících postupů správy přístupu:

* Přiřazení role na základě skupiny: můžete přiřadit přístup ke skupinám Azure AD, které mohou být synchronizovány z místní služby Active Directory. Díky tomu můžete využít stávající investice, které vaše organizace má provést v nástrojů a procesů pro správu skupin. Můžete také použít funkci delegované skupiny správy služby Azure AD Premium.
* Využívání, které jsou součástí role v Azure: tři role je možné použít – vlastník, Přispěvatel a čtečky, ujistěte se, že uživatelé a skupiny mají oprávnění k provádět jenom úkoly, které potřebují svou práci.
* Podrobné přístup k prostředkům: můžete přiřadit role pro uživatele a skupiny pro určitý odběr, skupinu prostředků nebo jednotlivých prostředků Azure, jako je například na webu nebo v databázi. Tímto způsobem zajistíte, že uživatelé mají přístup ke všem prostředkům, které potřebují a bez přístupu k prostředkům, které není potřeba spravovat.

> [!NOTE]
> Pokud vytváříte aplikace a chcete přizpůsobit řízení přístupu pro ně, je také možné použít Azure AD aplikační role pro autorizaci. Zkontrolovat to [WebApp. RoleClaims DotNet příklad](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) o tom, jak sestavit aplikaci použít tuto funkci.
>
>

3. Podmíněný přístup pro aplikace Office 365 s Microsoft Intune: správci IT mohou poskytnout zásady podmíněného přístupu zařízení kvůli zabezpečení prostředků podniku, zatímco ve stejnou dobu umožnili pracovníkům s vhodnými zařízeními přístup ke službám. Další informace najdete v tématu [Zásady podmíněného přístupu zařízení pro služby Office 365](active-directory-conditional-access-device-policies.md).

4. Podmíněný přístup pro aplikace Saas: [tuto funkci](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) umožňuje konfigurovat pravidla přístupu k jednotlivým aplikacím služby Multi-Factor authentication a možnost blokovat přístup pro uživatele není v důvěryhodné síti. Pravidla vícefaktorového ověřování můžete použít pro všechny uživatele, které jsou přiřazené k aplikaci nebo jenom pro uživatele v rámci určených skupinách zabezpečení. Uživatelé mohou být vyjmuty z požadavku vícefaktorového ověřování, pokud uživatelé přistupují aplikace z IP adresy, která v rámci organizace sítě.

Vzhledem k tomu použít s vícevrstvých možnosti pro řízení přístupu, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá všechny možnosti, které jsou dostupné pro každý scénář, který vyžaduje, abyste řízení přístupu k prostředkům.

## <a name="define-incident-response-options"></a>Definování možností reakce na incidenty
Azure AD může být užitečné tak identity potenciální rizika zabezpečení v prostředí tím, že monitorování činnost uživatele, IT oddělení můžete využít Azure AD přístup a použití sestav schopnost získat přehled o integrity a zabezpečení adresáři vaší organizace. Tyto informace a správce IT pomohou určit, kde může být bezpečnostním rizikům, tak, aby adekvátní můžete naplánovat zmírnění.  [Předplatné Azure AD Premium](active-directory-get-started-premium.md) obsahuje sadu sestavy zabezpečení, které můžete povolit tak získal tyto informace. [Azure AD sestavy](active-directory-view-access-usage-reports.md) jsou klasifikovány, jak je uvedeno níže:

* **Sestavy anomálií**: obsahovat přihlášení události, které je neobvyklé. Naším cílem je mít budete vědět, tyto aktivity a díky kterému budete mít možnost provádět rozhodnutí o tom, zda je událost podezřelé.
* **Integrované sestavy aplikace**: poskytuje přehled o tom, jak cloudové aplikace jsou používány ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíci cloudových aplikací.
* **Zprávy o chybách**: označení chyb, které mohou nastat při zřizování účtů k externím aplikacím.
* **Uživatelská sestavy**: Zobrazí zařízení nebo přihlášení v datech aktivity pro konkrétního uživatele.
* **Protokoly aktivity**: obsahovat záznam všech auditované události v posledních 24 hodin, posledních 7 dnů, nebo posledních 30 dní, a také změny aktivity skupiny a aktivita resetování a registraci hesla.

> [!TIP]
> Je jinou sestavu, který může také pomoct týmem reakcí na incidenty práci na případu [uživatele s uniklé přihlašovací údaje](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) sestavy.  Tato sestava zobrazí všechny shody mezi tyto seznamu uniklé přihlašovací údaje a vašeho klienta.
>
>

Další důležité součástí sestavy ve službě Azure AD, které je možné během šetření reakcí na incidenty a jsou:

* **Aktivita resetování hesla**: správce poskytnout přehled o tom, jak aktivně resetování hesla se používá v organizaci.
* **Registrace aktivita resetování hesla**: poskytuje přehledy, do kterých uživatelé zaregistrovali své metody pro resetování hesla, a metody, které jste vybrali.
* **Skupina aktivit**: poskytuje historii změn do skupiny (např: uživatelé přidat nebo odebrat) které byly zahájeny v přístupového panelu.

Kromě základní generování sestav funkce dostupné v Azure AD Premium, které můžete využít během procesu šetření reakcí na incidenty, IT oddělení můžete také využívejte sestava auditu chcete získat informace, jako například:

* Změny v členství role (např: uživatel přidaný k roli globálního správce)
* Aktualizace na přihlašovací údaje (např: změny hesla)
* Správa domény (např: ověření vlastní domény, odebrání domény)
* Přidání nebo odebrání aplikací
* Správa uživatelů (např: Přidání, odebrání, aktualizaci uživatele)
* Přidání nebo odebrání licencí

Vzhledem k tomu použít s vícevrstvých možnosti reakcí na incidenty, se nedají použít pro tuto úlohu porovnání mezi těmito možnostmi. Ujistěte se, že se využívá všechny možnosti, které jsou dostupné pro každý scénář, který vyžaduje použití funkce vytváření sestav Azure AD jako součást procesu reakcí na incidenty vaší společnosti.

## <a name="next-steps"></a>Další kroky
[Určení úlohy správy hybridní identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Viz také
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

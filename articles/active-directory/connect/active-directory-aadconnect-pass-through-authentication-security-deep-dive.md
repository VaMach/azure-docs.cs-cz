---
title: "Azure Active Directory předávací ověřování zabezpečení nabídnout detailní | Microsoft Docs"
description: "Tento článek popisuje, jak předávací ověřování Azure Active Directory (Azure AD) chrání vaše místní účty."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.openlocfilehash: a5feadd851b166d0a9a77bd1d124cdd599d5d701
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory předávací ověřování zabezpečení nabídnout detailní

Tento článek poskytuje podrobnější popis fungování předávací ověřování. Zaměřuje se více o aspektech zabezpečení funkce. Toto téma bude týkající se zabezpečení a správci IT, osob ředitel dodržování předpisů a zabezpečení a jiné IT Profesionálové zodpovědní za IT zabezpečení a dodržování předpisů na malé a střední organizace nebo velké podniky.

Témata řešit:
- Podrobné technické informace o tom, jak jsou ověřování agentů nainstalovaných a zaregistrovaných.
- Podrobné technické informace o šifrování hesla při přihlášení uživatele.
- Zabezpečení kanálů mezi místní ověřování agentů a Azure Active Directory (Azure AD).
- Podrobné technické informace o ověřování agentů jsou uchovány jak provozně zabezpečené.
- Další témata související se zabezpečením.

## <a name="key-security-capabilities"></a>Klíčové funkce zabezpečení

Tyto aspekty zabezpečení klíčů této funkce jsou:
- Je založen na zabezpečené nevyužívá dělené tabulky více architekturu, která zajišťuje izolaci žádostí o přihlášení mezi klienty.
- Místních hesel nikdy ukládají v cloudu v žádný formulář.
- Místní ověřování agentů, které naslouchat a reagovat na žádosti o ověření hesla, provést pouze odchozí připojení z ve vaší síti. Není nutné k instalaci těchto agentů ověřování v hraniční síti (DMZ).
- Pro odchozí komunikaci od agentů ověřování do služby Azure AD se používají pouze standardní porty (80 a 443). Žádné příchozí porty musí být otevřen na bráně firewall. 
  - Port 443 se používá pro všechny ověřené odchozí komunikaci
  - Port 80 se používá pouze pro stahování seznamy odvolaných certifikátů (CRL) pro zajištění, že žádný z certifikátů, které jsou používané funkce byly odvolány.
  - V tématu [sem](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) úplný seznam síťových požadavků.
- Hesla zadané uživatelem během přihlášení se šifrují v cloudu, než čip místní ověřování agentů pro ověření na základě služby Active Directory.
- Kanál HTTPS mezi službou Azure AD a místní ověřování agenta je zabezpečená službou vzájemné ověřování.
- Se bezproblémově integruje s možností ochrany cloudu Azure AD jako je například zásady podmíněného přístupu (včetně vícefaktorového ověřování), ochrany identit a inteligentní uzamčení.

## <a name="components-involved"></a>Součásti související se situací

Obecné podrobnosti o provozu, služba Azure AD a zabezpečení dat najdete v tématu [Centrum zabezpečení](https://azure.microsoft.com/support/trust-center/). Při použití předávaného ověřování pro přihlášení uživatele, se podílejí následující komponenty:
- **Azure tokenů zabezpečení AD**: bezstavové zabezpečení tokenu služby (STS), která zpracovává požadavky na přihlášení a vydává tokeny zabezpečení pro prohlížeče, klienty nebo služby podle potřeby uživatelů.
- **Azure Service Bus**: poskytuje povolenou podporu cloudu komunikace s podnikových způsobů zasílání zpráv a komunikaci předávací službu, která umožňuje připojit místní řešení s cloudem.
- **Agent Azure AD Connect ověřování (ověřování Agent)**: komponentu místní, která naslouchá a reaguje na požadavky na ověření hesla.
- **Azure SQL Database**: obsahuje informace o ověřování agentů vašeho klienta, včetně jeho metadata, šifrovacích klíčů.
- **Active Directory (AD)**: vaší místní služby Active Directory, kde jsou uloženy vaše uživatelské účty (a jejich hesla).

## <a name="installation-and-registration-of-authentication-agents"></a>Instalace a registrace ověřování agentů

Ověřování agenti jsou nainstalovaného a zaregistrovaného na Azure AD při jste [povolit předávací ověřování pomocí služby Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature) nebo když jste [přidat další ověřování agenty zajistit vysokou dostupnost požadavky na přihlášení](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability). Získávání k ověřování agenta pracovní zahrnuje tři hlavní fáze:

- Instalace agenta ověřování
- Registrace agenta ověřování
- Inicializace agenta ověřování

Každá z těchto probereme podrobně v následujících tématech.

### <a name="authentication-agent-installation"></a>Instalace agenta ověřování

Jenom globální správci můžete nainstalovat agenta ověřování (pomocí Azure AD Connect nebo samostatné) na místním serveru. Přidá tyto dvě nové položky k instalaci **ovládací panely -> programy -> programy a funkce** seznamu:
- Vlastní aplikace Agent ověřování. Toto spouští s [síťové služby](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) oprávnění.
- Aktualizační metoda aplikace používá pro automatické aktualizace agenta ověřování. Toto spouští s [místní systém](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) oprávnění.


### <a name="authentication-agent-registration"></a>Registrace agenta ověřování

Po instalaci agenta ověřování, musí se k registraci v Azure AD. Azure AD k tomu přiřazením každého agenta ověřování identity jedinečný digitální certifikát, který můžete použít pro zabezpečenou komunikaci se službou Azure AD.

Postup registrace také váže Agent ověřování pomocí vašeho klienta tak, že Azure AD ví, že tato konkrétní Agent ověřování je pouze jeden oprávnění pro zpracování žádosti o ověření hesla pro vašeho klienta. Tento postup se opakuje pro každý nový Agent ověřování zaregistrovat.

Zde je, jak agenty ověřování zaregistrovat se službou Azure AD:

![Registrace agenta](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD poprvé požádá globálního správce pro přihlášení k Azure AD pomocí svých přihlašovacích údajů. Během přihlášení Agent ověřování získává přístupu k tokenu, můžete použít jménem globální správce.
2. Agent ověřování pak vygeneruje pár klíčů – veřejný klíč a soukromý klíč.
    - Tento pár klíčů je generována pomocí standardní **RSA 2048 bitů** šifrování.
    - Privátní klíč se nikdy neopustí na místním serveru, na kterém byl nainstalován Agent pro ověřování.
3.  Agent ověřování požádá ", registrace" do služby Azure AD prostřednictvím protokolu HTTPS, s následujícími součástmi zahrnuté v žádosti:
    - Přístup k tokenu získali v kroku 1.
    - Veřejný klíč vygenerovaný v kroku 2.
    - A **žádosti o podepsání certifikátu** (CSR nebo žádost o certifikát). Toto je pro použití pro digitální identitu certifikát, s Azure AD jako jeho certifikační autority.
4. Azure AD ověří přístup k tokenu v žádosti o registraci a ověřuje, zda požadavek pochází globálního správce.
5. Azure AD a podepíše a vydá certifikát digitální identitu zpět do agenta ověřování.
    - Certifikát je podepsaná pomocí **Azure AD kořenové certifikační autority (CA)**. Všimněte si, že je tato CA _není_ v systému Windows na **důvěryhodné kořenové certifikační úřady** uložit.
    - Tato certifikační Autorita se používá pouze ve funkci předávací ověřování. Právě se používá pro podepisování zástupci oddělení služeb zákazníkům během registrace agenta ověřování.
    - Tato certifikační Autorita není používán jinou službu ve službě Azure AD.
    - Subjekt certifikátu (**rozlišující název nebo rozlišující název**) je nastaven na vaše **ID klienta**. Toto je identifikátor GUID, který jednoznačně identifikuje vašeho klienta. Tím nastavíte certifikátu pro použití s pouze vašeho klienta.
6. Azure AD ukládá veřejný klíč agenta ověřování v Azure SQL database, která pouze Azure AD má přístup k.
7. Certifikát (vydané v kroku 5) je uložen na místní server **úložiště certifikátů Windows** (konkrétně v  **[CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE)**  umístění) a je používán Agent ověřování a aktualizační aplikace.

### <a name="authentication-agent-initialization"></a>Inicializace agenta ověřování

Při spuštění ověřování agenta, poprvé po registraci nebo po server restartovat, musí být bezpečně komunikovat se službou Azure AD a začít přijímat žádosti o ověření hesla.

![Inicializace agenta](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Zde je, jak získat inicializovat agenty ověřování:



1. Agent ověřování začne tím, že požadavek odchozí "bootstrap" do služby Azure AD. 
    - Tento požadavek bootstrap se provádí přes port 443 a je přes kanál vzájemně ověřené HTTPS (s použitím stejný certifikát vydán během registrace agenta ověřování).
2. Azure AD odpoví na tuto žádost o zavedení tím, že poskytuje **přístupový klíč** pro frontu Azure Service Bus, které jsou jedinečné pro vašeho klienta (identifikovaný vaše ID klienta).
3. Agent ověřování vytvoří trvalé odchozí připojení HTTPS (přes port 443) fronty. 
    - Je nyní připravena k načtení a zpracování požadavků na ověření hesla.

Pokud máte více ověřování agenty registrovaný na klientovi a potom inicializace postup zajistí, že každé z nich připojuje ke stejné fronty Azure Service Bus. 

## <a name="processing-sign-in-requests"></a>Zpracování žádostí o přihlášení 

Následující diagram znázorňuje, jak předávací ověřování zpracovává požadavky přihlášení uživatele.

![Zpracování přihlášení](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Předávací ověřování zpracovává požadavek přihlášení uživatele takto: 

1. Uživatel se pokusí o přístup k aplikaci (například aplikaci Outlook Web App - [https://outlook.office365.com/owa](https://outlook.office365.com/owa).)
2. Pokud již není přihlášený uživatel, aplikace prohlížeč přesměruje na přihlašovací stránku služby Azure AD.
3. Služba tokenů zabezpečení Azure AD zpátky odpoví na přihlašovací stránce uživatele.
4. Uživatel zadá své uživatelské jméno a heslo na přihlašovací stránku služby Azure AD a klikne na tlačítko "Přihlásit".
5. Uživatelské jméno a heslo se odešlou do tokenů zabezpečení Azure AD v požadavku POST protokolu HTTPS.
6. Azure AD STS načte veřejných klíčů pro všechny agenty ověřování registrovaný na vašeho klienta z databáze Azure SQL a šifruje heslo jejich používání. 
    - Vyvolá zašifrované heslo n hodnoty pro agenty ověřování n registrovaný na klientovi.
7. Azure AD STS umístí požadavek ověření heslo (uživatelské jméno a heslo šifrované hodnoty) do fronty Azure Service Bus určený výhradně pro vašeho klienta.
8. Vzhledem k tomu, že inicializovaného agenty ověřování trvalé připojení do fronty Azure Service Bus, jednu z dostupných agentů ověřování načte žádost o ověření hesla.
9. Agent ověřování vyhledá zašifrované heslo hodnotu, která je specifická pro svůj veřejný klíč (pomocí identifikátoru) a dešifruje ji pomocí jeho privátní klíč.
10. Agent ověřování pokusí k ověření uživatelského jména a hesla před použitím místní služby Active Directory  **[rozhraní API Win32 LogonUser](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)**  (s **dwLogonType** parametr nastaven na **LOGON32_LOGON_NETWORK**). 
    - Toto je stejné rozhraní API používá služby Active Directory Federation Services (AD FS) pro přihlášení uživatelů ve scénáři federovaný přihlášení.
    - To závisí na procesu standardní překladu, který v systému Windows Server Najít řadiče domény.
11. Agent ověřování obdrží výsledek ze služby Active Directory (úspěch, uživatelské jméno nebo nesprávné heslo, platnost hesla, uživatel je uzamčen odhlašování, a tak dále).
12. Agent ověřování výsledek předá zpět tokenů zabezpečení Azure AD přes odchozí vzájemně ověřené kanál HTTPS přes port 443. Vzájemné ověřování používá stejný certifikát dříve vydané Agent ověřování během registrace.
13. Služby tokenů zabezpečení služby Azure AD ověřuje, že tento výsledek koreluje s konkrétní přihlášení žádost na klientovi.
14. Azure AD STS pokračuje s postupem přihlášení podle konfigurace. Například pokud ověření hesla bylo úspěšné, uživatel může být postiženy pro službu Multi-Factor Authentication nebo přesměrován zpět do aplikace.

## <a name="operational-security-of-authentication-agents"></a>Provozní zabezpečení ověřování agentů

Zajistit, aby zůstala provozně zabezpečené předávací ověřování, Azure AD pravidelně obnovuje své certifikáty. Tyto požadavky na obnovení se spouštějí z Azure AD a nejsou řídí agenty ověřování sami.

![Provozní zabezpečení](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Tady je způsob ověřování agenta obnovuje jeho vztah důvěryhodnosti s Azure AD:

1. Agent ověřování pravidelně příkaz ping Azure AD (každých několik hodin) ke kontrole, pokud je na čase obnovení svého certifikátu. 
    - Tato kontrola se provádí přes kanál vzájemně ověřené HTTPS (s použitím stejný certifikát vydaný během registrace).
2. Pokud službu naznačuje, že je čas k obnovení, Agent ověřování generuje nový pár klíčů: veřejný klíč a soukromý klíč.
    - Tyto klíče se generují pomocí standardní **RSA 2048 bitů** šifrování.
    - Privátní klíč se nikdy neopustí na místním serveru.
3. Agent ověřování pak požádá "obnovení certifikátu" do služby Azure AD prostřednictvím protokolu HTTPS, s následujícími součástmi zahrnuté v žádosti:
    - Existující certifikát (načíst z **CERT_SYSTEM_STORE_LOCAL_MACHINE** umístění v úložišti certifikátů systému Windows). Účastnící se tento postup, neexistuje žádný globální správce, takže neexistuje žádný přístup k tokenu potřeby jménem globální správce.
    - Veřejný klíč vygenerovaný v kroku 2.
    - A **žádosti o podepsání certifikátu** (CSR nebo žádost o certifikát). Toto je použít pro nový certifikát digitální identitu, s Azure AD jako jeho certifikační autority.
4. Azure AD ověří existující certifikát v požadavek na obnovení certifikátu. Poté ověří, zda požadavek pochází ověřování agenta registrován na vašeho klienta.
5. Pokud je stále platný stávající certifikát, Azure AD poté podepíše nový certifikát digitální identity a vydá nový certifikát zpět do agenta ověřování. 
6. Pokud vypršela platnost stávající certifikát, Azure AD Agent ověřování odstraní z vašeho klienta seznam registrovaných ověřování agentů. Globální správce musí potom ručně nainstalujte a zaregistrujte nový Agent ověřování.
    - Certifikát je podepsaná pomocí **Azure AD kořenové certifikační autority (CA)**.
    - Subjekt certifikátu (**rozlišující název nebo rozlišující název**) je nastaven na vaše **ID klienta** identifikátor GUID, který jednoznačně identifikuje vašeho klienta. To znamená certifikát je vymezen pouze klienta.
6. Azure AD ukládá v databázi Azure SQL, kterou pouze má přístup k "new" veřejný klíč agenta ověřování. A zruší platnost "staré" veřejného klíče přidruženého k agentovi ověřování.
7. Nový certifikát (který byl vydán v kroku 5) je pak uloženy v na serveru **úložiště certifikátů Windows** (konkrétně v  **[CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER)**  umístění).
    - Vzhledem k tomu, že postup obnovení vztah důvěryhodnosti se stane interaktivně (bez přítomnosti globální správce), Agent ověřování už má přístup k aktualizaci existující certifikát v **CERT_SYSTEM_STORE_LOCAL_MACHINE** umístění. Všimněte si, že samotný certifikát v **CERT_SYSTEM_STORE_LOCAL_MACHINE** umístění neodebere v rámci tohoto postupu.
8. Nový certifikát se používá pro ověřování z tohoto bodu na. Nahradí certifikátu v každé následné obnovení certifikátu **CERT_SYSTEM_STORE_LOCAL_MACHINE** umístění.

## <a name="auto-update-of-authentication-agents"></a>Automatická aktualizace agentů ověřování

Aktualizační aplikace automaticky aktualizuje agenta ověřování při vydání nové verze. Nezpracovává všechny žádosti o ověření hesla pro vašeho klienta. 

Azure AD hostitelem novou verzi softwaru jako přihlášeni **balíček Instalační služby systému Windows (MSI)**. Soubor MSI je podepsaná pomocí [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) s **SHA256** jako algoritmus digest. 

![Automatickou aktualizaci](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Tady je způsob ověřování agenta získá automaticky aktualizovat:

1. Aktualizační pravidelně příkaz ping Azure AD (každou hodinu) zkontrolujte, jestli je k dispozici nová verze agenta ověřování. 
    - Tato kontrola se provádí přes kanál vzájemně ověřené HTTPS (s použitím stejný certifikát vydaný během registrace). Agent ověřování a aktualizační sdílet certifikát uložený na serveru.
2. Pokud je dostupná nová verze, Azure AD podepsaný soubor MSI vrátí zpět do aktualizačního programu.
3. Aktualizační ověřuje, že soubor MSI je podepsán společností Microsoft.
4. Aktualizační metoda spustí soubor MSI. Tato akce nemá následující kroky (Všimněte si, že aktualizační běží s [místní systém](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) oprávnění):
    - Zastaví službu Agent ověřování.
    - Nainstaluje novou verzi agenta ověřování na serveru.
    - Restartuje službu Agent ověřování.

>[!NOTE]
>Pokud máte více ověřování agenty registrovaný na vašeho klienta Azure AD není obnovit své certifikáty nebo je aktualizovat ve stejnou dobu. Místo toho Azure AD se proto postupně k zajištění vysoké dostupnosti žádostí o přihlášení.


## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Inteligentní uzamčení** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -možnost konfigurace inteligentní uzamčení na vašeho klienta k ochraně uživatelské účty.
- [**Jak to funguje** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – získejte základní informace o tom, jak funguje Azure AD předávací ověřování.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.


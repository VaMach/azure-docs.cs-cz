---
title: "Azure AD předávací ověřování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak začít pracovat s Azure Active Directory (Azure AD) předávací ověřování."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: e0b58142a2ed17d2cd4749b33e9e80ff1a01662a
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure předávací ověřování služby Active Directory: Rychlý start

## <a name="how-to-deploy-azure-ad-pass-through-authentication"></a>Jak nasadit Azure AD předávací ověřování

Předávací ověřování Azure Active Directory (Azure AD) umožňuje uživatelům přihlásit se k místní i cloudové aplikace přihlašovali stejnými hesly. Ho přihlášení uživatele pomocí ověřování hesla přímo pro vaše místní službu Active Directory.

>[!IMPORTANT]
>Pokud používáte tuto funkci prostřednictvím preview, měli byste zajistit upgradu verze preview agentů ověřování pomocí pokynů [zde](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Je třeba postupovat podle těchto pokynů, které chcete nasadit předávací ověřování:

## <a name="step-1-check-prerequisites"></a>Krok 1: Kontrola předpokladů

Ujistěte se, že jsou splněné následující požadavky:

### <a name="on-the-azure-active-directory-admin-center"></a>V Centru pro správu Azure Active Directory

1. Vytvořte účet globálního správce jenom pro cloud v klientovi Azure AD. Tímto způsobem konfigurace vašeho klienta můžete spravovat, by měla místní služby nezdaří, nebo k dispozici. Další informace o [přidání účtu globálního správce jenom pro cloud](../active-directory-users-create-azure-portal.md). Provedením tohoto kroku je důležité zajistit, že nezůstanete z vašeho klienta.
2. Přidejte jednu nebo více [názvy vlastních domén](../active-directory-domains-add-azure-portal.md) ke klientovi Azure AD. Uživatelům se přihlásit pomocí některého z těchto názvů domény.

### <a name="in-your-on-premises-environment"></a>V místním prostředí

1. Určení serveru se systémem Windows Server 2012 R2 nebo novější, na který se má spustit Azure AD Connect. Přidejte server do stejné doménové struktuře AD jako uživatele, jejichž hesla se musí ověřit.
2. Nainstalujte [nejnovější verzi služby Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) na serveru identifikovaného v předchozím kroku. Pokud je již spuštěna Azure AD Connect, ujistěte se, zda je verze 1.1.644.0 nebo novější.

    >[!NOTE]
    >Azure AD Connect verze 1.1.557.0, 1.1.558.0, 1.1.561.0 a 1.1.614.0 mít problém související s **synchronizaci hodnoty Hash hesla**. Pokud jste _nemáte_ v úmyslu použít synchronizaci hodnoty Hash hesla ve spojení se předávací ověřování, přečtěte si [poznámky k verzi Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) Další informace.

3. Určit další server s Windows serverem 2012 R2 nebo novější, na který se má spustit samostatnou Agent ověřování. Verze agenta ověřování musí být 1.5.193.0 nebo novější. Tento server je potřeba zajistit vysokou dostupnost žádostí o přihlášení. Přidejte server do stejné doménové struktuře AD jako uživatele, jejichž hesla se musí ověřit.
4. Pokud je brána firewall mezi servery a Azure AD, budete muset nakonfigurovat následující položky:
   - Ujistěte se, že ověřování agentů provádět **odchozí** požadavky do Azure AD přes následující porty:
   
   | Číslo portu | Jak se používají |
   | --- | --- |
   | **80** | Stahování odvolání certifikátu seznamy (CRL) při ověřování certifikátu SSL |
   | **443** | Veškerá odchozí komunikace v naší službě |
   
   Pokud brána firewall vynucuje pravidla závislosti na zdroji uživatelů, otevřete tyto porty pro přenos ze služby Windows, které běží jako síťové služby.
   - Pokud je brána firewall nebo proxy server umožňuje povolených DNS, povolených připojení k  **\*. msappproxy.net** a  **\*. servicebus.windows.net**. Pokud ne, povolit přístup k [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), se aktualizují každý týden.
   - Ověřování agenty potřebují přístup k **login.windows.net** a **login.microsoftonline.com** pro počáteční registraci, takže otevřete brány firewall pro tyto adresy URL také.
   - Pro ověření certifikátu, odblokovat následující adresy URL: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** a **www.microsoft.com:80**. Tyto adresy URL se používají k ověření certifikátu s dalšími produkty Microsoftu, takže už můžete mít tyto adresy URL odblokováno.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Krok 2: Povolit podporu protokolu Exchange ActiveSync (volitelné)

Postupujte podle těchto pokynů můžete povolit podporu protokolu Exchange ActiveSync:

1. Použití [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) spusťte následující příkaz:
```
Get-OrganizationConfig | fl per*
```

2. Zkontrolujte hodnotu `PerTenantSwitchToESTSEnabled` nastavení. Pokud je hodnota **true**, váš klient je správně nakonfigurována – to je obvykle případu pro většinu zákazníků. Pokud je hodnota **false**, spusťte následující příkaz:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Ověřte, že hodnota `PerTenantSwitchToESTSEnabled` nastavení je teď nastavená na **true**. Počkejte, než jednu hodinu, než budete pokračovat k dalšímu kroku.

Pokud jste v tomto kroku musí čelit všechny problémy, zkontrolujte naše [Průvodce odstraňováním potíží s](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) Další informace.

## <a name="step-3-enable-the-feature"></a>Krok 3: Povolení funkce

Předávací ověřování se dá zapnout pomocí [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Předávací ověřování můžete povolit pro server primární nebo pracovní Azure AD Connect. Důrazně doporučujeme, abyste povolili z primárního serveru.

Pokud Azure AD Connect instalujete poprvé, vyberte [vlastní cesta](active-directory-aadconnect-get-started-custom.md). Na **přihlášení uživatele** vyberte **předávací ověřování** jako metodu přihlašování. Při úspěšném dokončení agenta předávací ověřování nainstalovaný na stejném serveru jako Azure AD Connect. Kromě toho že předávací ověřování je povolena funkce na klientovi.

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso3.png)

Pokud jste již nainstalovali Azure AD Connect (pomocí [Expresní instalace](active-directory-aadconnect-get-started-express.md) nebo [vlastní instalaci](active-directory-aadconnect-get-started-custom.md) cesta), vyberte **změnit přihlášení uživatele** úloh na Azure AD Připojit a klikněte na tlačítko **Další**. Potom vyberte **předávací ověřování** jako metodu přihlašování. Při úspěšném dokončení je agent předávací ověřování nainstalovaný na stejném serveru jako Azure AD Connect a tato funkce je povolena na klientovi.

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Předávací ověřování je funkce úrovni klienta. Zapnutí má dopad na přihlášení pro uživatele mezi _všechny_ spravované domény ve vašem klientovi. Pokud přecházíte z AD FS na předávací ověřování, doporučujeme, počkejte nejméně 12 hodin před ukončením infrastruktury služby AD FS – tato čekací doba je zajistit, že uživatelé můžete zachovat přihlášení k protokolu Exchange ActiveSync během přechodu.

## <a name="step-4-test-the-feature"></a>Krok 4: Testování funkce

Postupujte podle těchto pokynů k ověření, že jste povolili ověřování průchozí správně:

1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně.
3. Vyberte **Azure AD Connect**.
4. Ověřte, zda **předávací ověřování** zobrazí jako **povoleno**.
5. Vyberte **předávací ověřování**. Toto okno obsahuje seznam serverů, kde jsou nainstalovány agenty ověřování.

![Azure Centrum pro správu služby Active Directory – okno Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Centrum pro správu – okno předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

V této fázi se můžete přihlásit uživatele ze všech spravovaných domén ve vašem klientovi pomocí předávacího ověřování. Uživatelé z federovaných domén se však nadále přihlášení prostřednictvím služby Active Directory Federation Services (AD FS) nebo jiného zprostředkovatele federace, kterou jste dříve nakonfigurovali. Pokud převedete k doméně z federovaných spravovat, všichni uživatelé z této domény, automaticky spustí, přihlášení pomocí předávacího ověřování. Jenom pro cloud uživatelé nejsou vliv funkci předávací ověřování.

## <a name="step-5-ensure-high-availability"></a>Krok 5: Zajištění vysoké dostupnosti

Pokud plánujete nasadit předávací ověřování v produkčním prostředí, musíte nainstalovat samostatnou Agent ověřování. Tento druhý ověřování agenta nainstalujte na server _jiných_ než jedna spuštěná Azure AD Connect a první ověření agenta. Tato instalace poskytuje můžete vysokou dostupnost žádostí o přihlášení. Postupujte podle těchto pokynů můžete nasadit samostatnou Agent ověřování:

1. **Stáhněte si nejnovější verzi agenta ověřování (verze 1.5.193.0 nebo novější)**: Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně.
3. Vyberte **Azure AD Connect** a potom **předávací ověřování**. A vyberte **stažení agenta**.
4. Klikněte **přijmout podmínky a stahovat** tlačítko.
5. **Nainstalujte nejnovější verzi agenta ověřování**: spuštění spustitelného souboru stáhli v předchozím kroku. Zadejte pověření pro globálního správce vašeho klienta po zobrazení výzvy.

![Azure Active Directory Centrum pro správu – tlačítko Stáhnout agenta ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Centrum pro správu – okno stáhnout agenta](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Můžete také stáhnout agenta ověřování z [zde](https://aka.ms/getauthagent). Zajistěte, aby zkontrolujte a přijměte Agent ověřování [podmínkami služby](https://aka.ms/authagenteula) _před_ jeho instalaci.

## <a name="next-steps"></a>Další kroky
- [**Inteligentní uzamčení** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -možnost konfigurace inteligentní uzamčení na vašeho klienta k ochraně uživatelské účty.
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**Podrobné informace zabezpečení** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -další hluboké technické informace o funkci.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
